# Raw Strings & You

Ever try to type a string containing lots of double quotes, backslashes
or a Regex even?
A `String` literal in any of these cases is an ugly duckling,
filled to the brim with escaped characters, but up until Swift 5
there was no other way, we just had to...

*DEAL WITH IT*

## Here comes Swift Evolution's SE-200
[SE-200](https://github.com/apple/swift-evolution/blob/master/proposals/0200-raw-string-escaping.md) proposed a much needed change.
In this proposal "Raw Strings" or `String` literals delimited with the pound sign `#` at both ends are introduced.

Everything between the starting `#` and ending `#` is treated as plain text with no special meaning, printed as is.
The string literals that you have been using up to Swift 4.2 are now known as "Conventional" string literals.
And even better yet, lets say that `#` is also a character you would like to display unescaped,
in that case just double it:

```
##"This string does not need to escape # since it uses double sharps"##
```

```swift
// Conventional String literals
"In conventional string literals \"quotes\" and backslashes like \\ must be escaped"

// Raw String literal
#" In a raw string literal anything goes \u{2029} \n does not trigger a line break "Even double quotes" "#
```

## What about String interpolation?
String interpolation with raw strings is just as simple as with conventional.

```swift
let example: Int = 42

"This is a conventional string with interpolation: \(example)"

#"This is a raw string with interpolation: \#(example)"#

##"This is a string with raw pound signs with interpolation \##(example)"##
```

In short, in the rare event that you need to display a `#` character
in a raw string and need to use multiple `#` at the start and end to
escape the string just use the same number of sharps `#` after the escaping backslash.

## What about Multiline raw Strings?

Yep! Thats a thing too.
Its just as you might have guessed, same rules applied to raw strings
with using `#` just now paired with the ruls of multiline strings.

```swift
#"""
    This is totally a multi-lined string
    no need to escape stuff like " or \n or \u{2028}
    and string interpolation still works too: \#(2 + 2) is 5
    """#
```

## Not too bad
Regular Expressions will surely be easier to write in strings literals
from here on out, as well as double-quote rich strings.

Till next time!
