
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
}
```

OK! Now that thats out of the way lets revisit what our password checking function would look like now that it returns a result with meaningful failure information.

Now calling our function would produce the following output:

``` swift

// Numeric Password
register(password: "8675309999999") // .strictlyNumeric

// No numbers
register(password: "superduperpassword") // .noNumericCharacters

// Too Short
register(password: "Swiftdud3") // .lessThanTenCharacters

```
