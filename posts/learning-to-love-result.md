
# Learning to love Result

When Swift was introduced as another tool at the disposal of developers as an alternative to Objective-C, one of most prominent features of Swift was its emphasis on the `Optional` type.

The healthy programming practices `Optional` brought with it forced developers to deal with the possibility of a value being `nil` or have a compile time error staring them in the face.
This has done wonders in reducing run-time errors but by no means is `Optional` a silver bullet.
There are many reasons why a value could be `nil` and if the context is obvious enough `Optional` might suffice, but in most other cases more context is needed and unfortunately `Optional` can not provide this.

This is where `Result` comes in.

### Evolution of the Result type
Starting with Swift 5, the type `Result` is baked into Swift's standard library.
Result is very simple, you could easily roll your own:

```swift
enum Result<T, E: Error> {
    case success(T)
    case error(E)
}
```

Thats all there is to it, just a simple `Either` data type, i.e either A or B.
The 'B' in the case of Result is specialized, it must be a type that conforms to `Error`.

Open Source `Result` micro frameworks such as [`antitypical/Result`](https://github.com/antitypical/Result) have been around almost as long as Swift it's self has.
The usage of `Result` in Swift code is nothing new but just how `antitypical/Result` provided a common definition of the `Result` type that would allow multiple libraries to share a single definition making code integration less painful, the `Result` type provided by the Swift standard library allows code to share a now universal result type, one that is on every single installation of swift from here on out.
Now that `Result` is in the standard library, an "official" definition exists and the swift community would benefit by leaving third-party `Result` types behind and simply leveraging Swift 5's `stdlib` version.

## Using `Result`

Result is commonly returned from a function that can fail.
Take for example a fictional function that registers a master passphrase for a password manager app.

``` swift
func register(password: String) -> String? {

    if password.count < 10 {
        return nil
    }

    if !password.contains(where: { ("0"..."9").contains($0)}) {
        return nil
    }

    if !password.contains(where: { ("a"..."z").contains($0) }) {
        return nil
    }

    if !password.contains(where: { ("A"..."Z").contains($0) }) {
        return nil
    }

    return "Password registered!"
}
```

The rules are simple, the password must be ten characters or more and composed of upper and lower case characters.

When an error occurs with this function we just simply return `nil` which is not very descriptive and forces us to use a one-size-fits-all message like "Password not vaild, try again".

Of course the return value could be a touple with an error or error message AND a success message like this:

``` swift
func register(password: String) -> (success: String?, error: String?) {
  ...
}
```

but then this code could express a situation that should be impossible, a kind of "Superposition" case that only Schrodinger would appreciate.
I don't like [Schrodinger's Cat](https://en.wikipedia.org/wiki/Schr%C3%B6dinger%27s_cat) because what kind of monster would put a cat in danger?
Superposition is cool but it has no place in a modern typed programming languages (when avoidable) so lets stick to a single `Result` return value.


``` swift
// Numeric Password
register(password: "8675309999999") // nil

// No numbers
register(password: "superduperpassword") // nil

// Too Short
register(password: "Swiftdud3") // niil
```

So calling the above code with different reasons for failing password validation all produced the same `nil` value. Not much to present to the user other than "Use a stronger password".

Well, we know that `Result`'s failure type must conform to error, and an `enum` is a simple and clean way of expressing different distinct states so lets go ahead and fill that out:

``` swift
enum PasswordError: Error {
    case tooShort
    case strictlyNumeric
    case noUppercaseLetters
    case noLowerCaseLetters

    var failureMessage: String {
        switch self {
            case .lessThanTenCharacters:
                return "Password must contain ten characters or more."
            case .noNumericCharacters:
                return "Password must contain numeric characters."
            case .strictlyNumeric:
                return "Password must contain both upper and lowercase characters."
            case .noUppercaseLetters:
                return "Password must contain upper case characters."
            case .noLowerCaseLetters:
                return "Password must contain lower case characters."
        }
    }
}
```

OK! Now that thats out of the way lets revisit what our password checking function would look like now that it returns a result with meaningful failure information.

``` swift
func register(password: String) -> Result<String, PasswordError> {

    if password.count < 10 {
        return .failure(.lessThanTenCharacters)
    }

    if !password.contains(where: { ("0"..."9").contains($0)}) {
        return .failure(.noNumericCharacters)
    }

    let noLowerCaseCharacters = !password.contains(where: { ("a"..."z").contains($0) })
    let noUpperCaseCharacters = !password.contains(where: { ("A"..."Z").contains($0) })

    if noLowerCaseCharacters && noUpperCaseCharacters {
        return .failure(.strictlyNumeric)
    }

    if noLowerCaseCharacters {
        return .failure(.noLowerCaseLetters)
    }

    if noUpperCaseCharacters {
        return .failure(.noUppercaseLetters)
    }

    return .success("Password Registered!")
}
```

Now calling our function would produce the following output:

``` swift

// Numeric Password
register(password: "8675309999999") // .failure(.strictlyNumeric)

// No numbers
register(password: "superduperpassword") // .noNumericCharacters

// Too Short
register(password: "Swiftdud3") // .lessThanTenCharacters

```

And finally we can get around to writing our error handling code a little more concise and clearly.

Writing the logic to be either success or failure and now there is no chance of "superposition" or some other undefined state where they are both `nil`:

``` swift
switch result {
case .success(let message):
    handleSuccess(with: message)
case .failure(let failure):
    retry(with: failure.message)
}
```

# Features of the Result type

Result has a constructor to create a Result from throwing code, if the method throws then the
Result has two methods for Transforming values:  `map` and `flatMap` and two methods for transforming errors: 'mapError' and


## Replacing code that `throw`s
One handy feature of Result is it's ability to wrap code that throws errors.
This has a few benefits but one obvious one is that instead of catching the error and dealing with it right away, the output of `Result` can be saved and if its an error it can be dealt with at a more oppertune time.

Many error handling examples deal with network code so I will try to be less cliche and use an example of dealing with exceptions that arise from parsing, namely `Codable` failing to decode JSON.

``` swift
struct CardboardBox: Codable {
    let brand: String
    let width: Double
    let height: Double
    let depth: Double
    let flavor: String?
}
```

Given the above `CardboardBox` structure it could be initialized with the following valid JSON:

``` swift
let validBoxJSON = """
{
    "flavor": "Musty with undertones of cherry and an aftertaste of wet river rock, NOT dry river rock.",
    "brand": "Nick's Fancy Boxes",
    "width": 5,
    "height": 5,
    "depth": 5
}
"""

do {
    let box = try JSONDecoder().decode(CardboardBox.self, from: validBoxJSON.data(using: .utf8)!)
    print(box.brand)
} catch {
    print("There was an error parsing the JSON")
}
```

Thats quite a bit of code just to handle one possible exception, more than that we are forced to handle the event of an error in the `catch` block at the moment an exception is thrown.

I personally find this lacking but fortunately there is good news: `Result` has an initializer that takes a closure that throws exceptions and automatically wraps it in a handy Result type.

The example below has intentionally misformed JSON to demonstrate error handling with the `Result` type.
The property "flavor" should be a `String` but here we use an Integer (42, the answer to the meaning of life.)
This will cause JSONDecoder to fail.

``` swift
let inValidBoxJSON = """
{
"flavor": 42,
"brand": "The Best Brand",
"width": 5,
"height": 5,
"depth": 5
}
"""

if let data = inValidBoxJSON.data(using: .utf8) {
    let myBoxResult = Result {
        try JSONDecoder().decode(CardboardBox.self, from: data)
    }

    switch myBoxResult {
    case .success(let box):
        print(" -- My box --\n\n \(box)")
    case .failure(let failure):
        print(failure.localizedDescription)
    }
}
```

We use `Result`'s closure constructor to wrap the throwing code and now we can deal with the end result in a very readable switch statement.
One of the big benefits of this approach is that the result is saved and stored away for later in `myBoxResult`, it can be dealt with when it is convenient for the programmer which is very important for dealing with asynchronous code.

## 

## Further Reading

There are many other great mini-tutorials out there that will pop up in your search results but in addition to reading those I highly suggest you take a look at the Swift Evolution Proposal [SE-235: Add result](https://github.com/apple/swift-evolution/blob/master/proposals/0235-add-result.md)

Happy Trails! 🤠
