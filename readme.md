# Kiara: the programming language for full-stack web-app/game development

Kiara is a high-level, gradually-typed, multi-paradigm programming language meant for web-based full stack app and game development.

It's got a syntax similar to Rust, JS, LiveScript, CoffeeScript, Ruby, Elixir, OCaml, Haskell, Nim, & F#, while compiling to WebAssembly, and HTML, CSS and JavaScript.

I'll be using this language for future projects once the Rust compiler, JS formatter, YAML TextMate file, and the documentation for the language and its standard library is done.

```
# main.ki
# -> delimits an expression from a statement
elem App(args: []str): void =
  %body
    %h1 Yo, Kiara is running!
    %Game

# closures return their last statement
func sieve_of_eratosthenes(len?: int = 100): []int =
  var sieve = [for in len: true]
  for i in 2...len
    if sieve[i]
      for j in (i**2)...len..i
        sieve[j] = false
  # 'return' is optional unless the return type is void
  sieve.filter(|is_prime, num| num > 1 && is_prime)

const primes = sieve_of_eratosthenes(50)
print("Sieve", primes)
```

## Comments

```
# line comments start with '#'
# and must always be followed by a space
# they can be on the same line or multiple lines

##
  Multi-line block comments
  can span across several lines.
  They are delimited by '##' at the beginning
  and '##' at the end.
##

#[ This is also a block comment. (Nim) ]#

# You can nest block comments too.
###
  This is the outer block comment.
  ## This is a nested block comment. ##
  More content in the outer comment.
###

#* This function adds two integers.
func add(x: int, y: int): int = x + y

##*
* This is a more detailed description of the
* `subtract` function. It explains the parameters
* and the return value.
*##
func subtract(a: int, b: int): int = a - b
```

## Variables

In Kiara, variables are declared using the `const` or `var` keywords. `const` declares immutable variables (their value cannot be reassigned after initialization), while `var` declares mutable variables (their value can be reassigned).

Type annotations are optional due to Kiara's gradual typing. If a type is not explicitly provided, the compiler will attempt to infer it based on the initial value.

```
Plaintext

# Immutable variable (constant) with type inference
const message = "Hello, Kiara!"

# Immutable variable with explicit type annotation
const pi: float64 = 3.14159

# Mutable variable with type inference
var counter = 0
counter = 1 # Reassignment is allowed for 'var'

# Mutable variable with explicit type annotation
var name: str = "Alice"
name = "Bob"

# You can declare multiple variables on the same line
const x, y: int = 10, 20
var a, b = true, "test"

# Attempting to reassign a 'const' variable will result in a compile-time error
# message = "Goodbye!" # Error!

# Type of a variable can be explicitly changed if it's mutable (gradual typing)
var flexible: any = 10
flexible = "a string"
flexible = true

# However, performing operations that are not valid for the current type might lead to runtime errors
# print(flexible + 5) # Might cause a runtime error if flexible is not a number

# Scope of variables:
# Variables declared inside a block (e.g., within a function or loop)
# are typically local to that block.

func example_scope(): void =
  var local_variable = "I am local"
  print(local_variable)

example_scope()
# print(local_variable) # Error: local_variable is not defined in this scope

if true
  var block_variable = "I am in the if block"
  print(block_variable)

# Depending on the specific scoping rules (lexical vs. dynamic),
# block_variable might or might not be accessible here.
# Kiara aims for lexical scoping, so it would likely not be accessible.

# Shadowing: A local variable can have the same name as a variable
# in an outer scope. The local variable takes precedence within its scope.
const outer_value = 100

func shadowing_example(outer_value: int): void =
  print("Inner outer_value:", outer_value) # Parameter shadows the outer constant
  var outer_value = 50 # Local variable shadows the parameter
  print("Local outer_value:", outer_value)

shadowing_example(200)
print("Outer outer_value:", outer_value)
```

## Literals

### Numbers

Kiara provides a rich set of features for working with numbers, encompassing various bases, exponent notation, explicit type suffixes for different numeric kinds (integers, rationals, decimals, complex numbers), unsigned and signed integer types of varying bit widths, fixed-size floating-point types, complex number types, and platform-dependent native number types. The language also supports standard arithmetic, bitwise, and comparison operators, including chained comparisons and a ternary operator.

Compound assignment operators offer concise ways to modify variables. Kiara's range operators are flexible, allowing for inclusive and exclusive ranges with optional step values. The `in` and `!in` operators facilitate checking for the presence of a value within a range.

Numeric literals can be made more readable using underscores as separators. Furthermore, you can explicitly specify the base of integer literals. While generally not recommended, Kiara even allows extending primitive number types. Lastly, it provides access to special numeric values like NaN and infinity for floating-point and complex number types.

```
# numbers
const dec = 1073741824
const hex = 0xdead_bea7
const bin = 0b100100100111010110
const oct = 0o34_073
const doz = 0z012_345_678_9↊↋ # a/b can be used as digits 10/11 as well

# exponents
const doz = 4.9e-7

# type suffixes after numeric literals
const integer = 1 # arbitrary precision integer
const integer1 = 2i # arbitrary precision integer
const rational = 2/3r # Rational number (fraction)
const decimal = 1.23m # Decimal number
const precise_decimal = 1.230p10m # precise to 10 decimal places
const imag = 30j # complex number (real + imaginary)

# unsigned integer types
const unsigned_8 = 255u8 # 8-bit unsigned integer
const unsigned_16 = 65000u16 # 16-bit unsigned integer
const unsigned_32 = 4000000000u32 # 32-bit unsigned integer
const unsigned_64 = 18000000000000000000u64 # 64-bit unsigned integer
const unsigned_128 = 34000000000000000000000000000000000000u128 # 128-bit unsigned integer

# fixed-size floating point types
const signed_8 = -10i8 # 8-bit signed integer
const signed_16 = -32000i16 # 16-bit signed integer
const signed_32 = -2000000000i32 # 32-bit signed integer
const signed_64 = -9000000000000000000i64 # 64-bit signed integer
const signed_128 = -17000000000000000000000000000000000000i128 # 128-bit signed integer

# signed integer types
const float_16 = 1.414f16 # 16-bit floating point number (half-precision)
const float_32 = 3.14f32 # 32-bit floating-point number (single precision)
const float_64 = 2.71f64 # 64-bit floating-point number (double precision)
const float_128 = 1.618f128 # 128-bit floating-point number (quad precision)

# complex integer types
const complex_32 = 0.1+2j32 # 32-bit complex number
const complex_64 = 1+2j64 # 64-bit complex number
const complex_128 = 3-4.5j128 # 128-bit complex number (alternative imaginary unit)
const complex_256 = 3-4.5j256 # 256-bit complex number (alternative imaginary unit)

# native number types
const native_int = 100ni # Platform-dependent signed integer
const native_uint = 200nu # Platform-dependent unsigned integer
const native_float = 2.01nf # Platform-dependent floating point number
const native_complex = 1.0+3.0nj # Platform-dependent complex number

# arithmetic operations
const a = 2 + 3 # addition
const b = 5 - 1 # subtraction
const c = 4 * 6 # multiplication
const d: float = 10 / 2 # division (returns a float by default)
const e: int = 10 ~/ 2i # division (returns an integer)
const f = 11 % 3 # modulo
const g = 2 ** 5 # exponentiation

# bitwise operations
const and_oper = 5 & 3 # bitwise AND
const or_oper = 5 | 3 # bitwise OR
const xor_oper = 5 ^ 3 # bitwise XOR
const left_shift = 5 << 1 # left shift
const right_shift = 5 >> 1 # sign-propagating right shift
const unsigned_right_shift = 5 >>> 1 # unsigned right shift

# comparison operators
const eq = 5 == 5 # equal to
const ne = 5 != 3 # not equal to
const lg = 5 <> 3 # not equal to, same as !=
const gt = 5 > 3 # greater than
const lt = 5 < 3 # less than
const ge = 5 >= 5 # greater than or equal to
const le = 5 <= 3 # less than or equal to
const ternary: int = 1 <=> j # returns 1, 0 or -1

# all comparison operators can be chained
# since they are non-associative
const chained = 5 > 3 > 2 > 1 # returns 1

# compound assignment operators
var r = 10
r += 5 # r is now 15
r -= 2 # r is now 13
r *= 3 # r is now 39
r /= 4 # r is now 9.75
r %= 2 # r is now 1.75 (remainder of 9.75 / 2)
r **= 2 # r is now 3.0625
r &= 3 # r is now 2
r |= 1 # r is now 3
r ^= 2 # r is now 1
r <<= 2 # r is now 4
r >>= 1 # r is now 2
r >>>= 1 # r is now 1

# range operators
# '...' inclusive on both ends
const range1 = 1...5 # inclusive range: 1, 2, 3, 4, 5
# '..<' inclusive start, exclusive end
const range2 = 1..<5 # exclusive end: 1, 2, 3, 4
# '>..' exclusive start, inclusive end
const range3 = 1>..5 # exclusive start: 2, 3, 4, 5
# '>.<' exclusive on both ends
const range4 = 1>.<5 # exclusive on both ends: 2, 3, 4

# open-ended ranges: '...'
const infinite_from = 5... # 5, 6, 7, ...
const infinite_up_to = ...10 # ...8, 9, 10

# step syntax: '..'
const range_with_step = 1..10..2 # 1, 3, 5, 7, 9
const descending_range = 10..1..-1 # 10, 9, 8, 7, 6, 5, 4, 3, 2, 1

# combining exclusive/inclusive with steps
const exclusive_end_step = 1..<10..2 # 1, 3, 5, 7
const exclusive_start_step = 1>..10..2 # 3, 5, 7, 9
const exclusive_both_step = 1>.<10..2 # 3, 5, 7

# 'in' and '!in' operators check for presence
assert 7 in range_with_step
assert 8 !in range_with_step

# numeric literals can use underscores as separators
const large_number = 1_000_000_000

# you can also specify the type explicitly
const base10 = 123_i # decimal (base 10)
const base16 = 0xAB_i # hexadecimal (base 16)
const base2 = 0b1010_i # binary (base 2)
const base8 = 0o77_i # octal (base 8)
const base12 = 0z123_i # duodecimal (base 12)

# extend primitive types (not recommended)
class num from num =
  # concatenate numbers
  infix func [++](other: num): num -> num(str(this) + str(other))

const num1 = 10
const num2 = 20
const concatenated = num1 ++ num2 # concatenated.value will be 1020
print("Concatenated Num:", concatenated)

# special numeric values. :: is static accessor
const not_a_number_f32: f32 = f32::NAN # not-a-number for 32-bit floats
const not_a_number_f64: f64 = f64::NAN # not-a-number for 64-bit floats
const infinity_f32: f32 = f32::INF # positive infinity for 32-bit floats
const negative_infinity_f32: f32 = -f32::INF # negative infinity for 32-bit floats

# special case for complex numbers:
# real f32 + imaginary f32 make a j64
const infinity_complex_64: j64 = -f32::INF + f32::INF * 1j
```

### Characters and strings

In Kiara, characters are represented as single-character strings with backticks. Each character has a `length` of `1` and has a `size` of 1 to 4 bytes in memory, depending on its UTF-8 encoding. Importantly, **every character in Kiara is treated as a string of length one**, regardless of its Unicode code point.

```
# in Kiara, every character is length 1 regardless of code point and encoding
const single_quote: str = 'Hello, Kiara!'
const double_quote = "Hello"
const backtick_quote = `Hello

# char is a subtype of str, which has length 1
assert type char == type str[1]

# non-alphanumeric characters should be escaped in backtick strings
assert "Hello World!" == 'Hello, world!'
assert `\s == `\  == ' ' # true

const rawString = @"This is a raw string. \n and \t are not escaped."
const rawString = @'""Double up your quotes like in YAML!"" they said.'
const multiline_string = """
This is a normal multiline string.
String begins at the first line with a non-spacing character.
Characters like \n and \t are still escaped normally.
"""

const raw_multiline_string = @"""
This is a raw multiline string.
\n and \t are not escaped.
"""

const interpolated_multiline_string = $"""""
Any number of the same quote character is allowed,
as long as that amount of quote characters is odd and equal on both ends.

'$$' is the same as adding a literal $.
The same can be said for % in $ or %-prefixed multiline strings.

Spaces and newlines are interpreted literally.
"""""

# string interpolation - $ prefix
const name = "Kiara"
const greeting = $'Hello, $name!'
const the_answer = %"the answer to the universe is $42"

const price = 99
const message_1 = $"""The price is \$price.""" # same as input
# in a raw string, $$ denotes a literal $.
const message+2 = $@"""The price is $$$price.""" # correct way to get "The price is $99."

# string formatting - % prefix
const percentage = 75
const formatted1 = %'Value: $percentage%' # still valid, % is still literal
const formatted2 = %'Value: $percentage\%' # correct way to get "Value: 75%"

const multi_formatted = %@"""
Report:
Progress: $percentage%d%% complete.
"""
print(multi_formatted)

# An empty multi-line string doesn't exist
const empty_single_string = ''
const empty_double_string = ""
const empty_backtick_string = `

# format string (% prefix)
const pi = 3.14159m
const formatted_string = %'π ≈ $pi%d(p=2)'
print(formatted_string)

# tagged template string, same as "x`str`" in JS
const table_name = "users"
const column = "email"
const query = sql$"select * from $table_name where $column = 'test@example.com';"

# string comparison
const names = ['Alaine', 'Brendan', 'Brooke', 'Alex']
const sortedNames = names.sort(|a, b| a <-> b)
const sortedNames = names.sort(<->) # pass built-in operators as functions

# string concatenation
const part1 = "Hello"
const part2 = ", Kiara"
const full_greeting = part1 + part2

# escape sequences (shown with backticks)
const char1 = `A
const char2 = `?
const space_char = `\s # not &nbsp;
const unicode_char = `キ
const escaped_char = `\n # new line
const tab_char = `\t # horizontal tab
const vertical_tab_char = `\v # vertical tab
const backslash_char = `\\ # backslash
const single_quote_char = `\ # single quote
const double_quote_char = `" # double quote
const null_char = `\0 # null character
const carriage_return_char = `\r # carriage return
const form_feed_char = `\f # form feed
const control_char = `\cm # any letter from A to Z
const bell_char = `\a # alert or bell
const backspace_char = `\b # backspace
const vertical_tab_char = `\v # vertical tab
const hex_escape_char = `\x1f642 # '🙂'
const unicode_escape_char = `\u1f31e # '🌞'
const octal_escape_char = `\o141 # 'A'
const decimal_escape_char = `\d65 # 'A'
const duodecimal_escape_char = `\z55 # 'A'

# \d, \o, \u, \x, \z are consumed until it finds the first non-digit character
const decimal_match = "\d123abc" # "{abc"
const octal_match = "\o777xyz" # "ǿxyz"
const unicode_match = "\u2603snowman" # "☃snowman"
const hex_match = "\xff-ff" # "ÿ-ff"
const duodecimal_match = "\z1b0end" # "Ĕend"

# sequences of code points can go between
# the {} of \d, \o, \u, \x, \z, delineated
# by spaces, commas or semicolons
const decimal_sequence = "\d{65 66 67}" # "ABC"
const octal_sequence = "\o{101 102 103}" # "ABC"
const unicode_sequence = "\u{0041 0042 0043}" # "ABC"
const hex_sequence = "\x{41 42 43}" # "ABC"
const duodecimal_sequence = "\z{55 56 57}" # "ABC"

# string indexing and slicing
const text = "Kiara"
const firstChar = text[0] # 'K'
const secondChar = text[1] # 'i'
const substring1 = text[1...3] # "iar" (inclusive range)
const substring2 = text[1..<3] # "ia" (exclusive range)
const suffix = text[2...] # "ara"
const prefix = text[..<3] # "Kia"
const everyOther = text[..2] # "Kar" (step of 2)
const reversed = text[..-1] # "araiK" (step of -1)

# string methods
const uppercase = text.upper() # "KIARA"
const lowercase = text.lower() "kiara"
const trimmed = "  Kiara  ".trim() # "Kiara"
const starts_with_k = text.starts_with('k') # true
const ends_with_a = text.ends_with('a') # true
const contains_ia = "ia" in text # true
const doesnt_contain_ara_ara = /ara ara/ !in text # true
const replaced = text.replace('i', 'ee') # "Keeara"
const replaced_operator = text =~ ('i', 'ee') # "Keeara"
const replaced_regex = text =~ /i:ee/ # "Keeara
const split_by_dash = "K-i-a-r-a" / `- # ["K", "i", "a", "r", "a"]
const split_by_space = "K i a r a" / /\s/ # ["K", "i", "a", "r", "a"]
const repeated = "Kiara" * 3 # "KiaraKiaraKiara"
const length = text::len # 5
const size = text::size # 5 - number of bytes in UTF-8 encoding

# len is static property shared with other types,
# like lists, sets, maps, etc, hence is accessed with ::

# type casting with 'as' operator
const num_from_str = "123" as int # 123 (inferred type)
const float_from_str = "3.14" as float32 # 3.14 (f32)
const result = "true" as bool # true

const num_to_str = 123 as str # "123"
const float_to_str = 3.14f64 as str # "3.14"

const char_list = "Kiara" as []char # ['K', 'i', 'a', 'r', 'a']
const char_list_1 = "Kiara" as List<char> # List('K', 'i', 'a', 'r', 'a')

const str_from_list = ['H', 'e', 'l', 'l', 'o'] as str # "Hello"
const str_from_list_1 = List('W', 'o', 'r', 'l', 'd') as str # "World"
```

### Booleans

```
const is_true = true
const is_false = false

# boolean operators
const and_op = true && false # false
const or_op = true || false # true
const xor_op = true ^^ false # true (exclusive OR)
const not_op = !true # false
const not_op = !true # false

# truthiness and falsiness
# 0, '', `\0, @"", """ """, [], {}, null are considered falsy
# all other values are truthy

if 0
  print("This won't print")
else
  print("Empty string is falsy")

if [1, 2, 3]
  print("This will print")

# comparison operators except <=> return booleans (see Numbers section)
const comparison_result = 5 > 3 # true

# use =~ and !~ for the abstract equality operators
# these implicitly call str() before being compared
const string_num = "5"
const number_num = 5
const abstract_equal = string_num =~ number_num # true
const abstract_not_equal = string_num !~ number_num # false
```

### Regular Expressions

Kiara's flavor of RegExps is similar to [Oniguruma](https:#github.com/kkos/onirguruma) and Perl's syntax, which is more powerful than JS's native RegExps. More advanced features and integration with format syntax will be detailed in a future document.

```
# regular expressions (Oniguruma, Perl and more)
const email = /((?:mailto:)?[a-zA-Z\d.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z\d-]+(?:\.[a-zA-Z\d-]+)*)/g
const multiline_regex = />
  <([a-z][a-z0-9]*)\b[^>]*> # Start tag
  ([^]*?) # Tag contents
  \</\1> # End tag (no need to escape '/')
</im

# quotes inside regexes like strings
'bro' ~= /'bro'/ # true
'bro' ~= /bro/ # true
/'bro'/ == /bro/ # false, these are different regexes

# $ and % prefixes can work on regexps too
# so long as there's no alpanumeric characters
# right after the $ which may clash with regex syntax
const htmlTag = "div"
# matches <DIV>…</DIV>
const interpolated_regex = %/>
  ^ # beginning of line/string
  <(?'tag'$htmlTag%str::upper)> # Start tag
  ([^]*?) # Tag contents
  \</\k'tag'> # End tag
  $ # end of line/string
</

const string = 'DIV'
const match = %/^$htmlTag%str::upper$</

# string matching
const hello = "Hello, world!"
const match = hello =~ /^hello/i
const no_match = hello !~ /^hi/i # negation of ~=

# regex replacement
const hewwo = hello =~ /[lr]:w/gi # "Hewwo, wowwd!"

# each 'regex:replace' pair is separated with ';'
const hewwo_everyone = # "hewwo evewynyan!"
  "hello everyone!" =~ /[lr]:w;one\b:nyan/gi

# newlines/leading/trailing spaces are stripped
# a new line/semicolon indicates a new replacement pair
const hewwo_everyone =
  "hello everyone!" =~ />
  [lr]: w # comments are allowed
  'one'\b: nyan
</gi
```

### Null and Optionals

Kiara simplifies the concept of null values by providing a single type: `null`. This contrasts with languages like JavaScript, which have multiple representations for the absence of a value. Kiara also introduces the `Option` type, similar to Rust, to handle potentially missing values in a type-safe manner.

```
const nothing = null # equivalent to null or undefined in other languages
const maybe_a_number: int? = 42 # optional integer, can be 42 or null
const maybe_a_string: str? = null # optional string

# null coalescing operator (??)
const name: str? = null
const default_name = name ?? "Guest" # default_name will be "Guest"

const age: int? = 25
const actual_age = age ?? -1 # actual_age will be 25

# optional chaining (?.) - similar to JS
struct Person =
  name: str
  address: Address?
struct Address =
  city: str?

const person1 = Person("Alice", Address("Singapore"))
const person2 = Person("Bob", null)

const city1 = person1.address?.city # city1 will be Some("Singapore")
const city2 = person2.address?.city # city2 will be null
const city3 = person1.address?.country # if Address doesn't have country, it's null

# force unwrap (!) - use with caution
const definitely_a_number: int? = 100
const unwrapped_number: int = definitely_a_number! # unwrapped_number is 100
# Panics at runtime if definitely_a_number is null

# force access (!.)
struct Person =
  address: Address?
struct Address =
  city: str?

const person = Person(Address("Singapore"))
const city = person.address!.city! # "Singapore"

const person_no_address = Person(null)
# const city_no_address = person_no_address.address!.city! # panics

# Force access with lists
const list_of_options: Option<[]>int> = Option::Some([1, 2, 3])
const first_element: int = list_of_options!.unwrap![0] # 1

const empty_list: Option<[]>int> = Option::Some([])
# const access_empty = empty_list!.unwrap![0] # panics

# Force access with maps
const map_of_options: Option<{str: int}> = Option::Some({"a": 1, "b": 2})
const value_a: int = map_of_options!.unwrap!."a" # 1

const empty_map: Option<{str: int}> = Option::Some({})
# const access_empty_map = empty_map!.unwrap!."c" # panics

# force access chained optional values
struct Nested =
  inner: Option<Inner>
struct Inner =
  value: int?

const nested_some = Nested(Option::Some(Inner(Option::Some(10))))
const nested_none = Nested(Option::None)

const inner_value = nested_some.inner!.value! # 10
# const inner_value_none = nested_none.inner!.value! # panics

# static access/chaining operators (?: and !:)
# ?: is the Elvis operator, then !: is the Bowie operator!?
struct Math =
  static PI: float = 3.14159
  static E: float? = 2.71828
  static log(x: float): float =
    # some log implementation
    0.0

# static access with '::'
const pi_value = Math::PI # 3.14159
const e_value = Math::E # Option::Some(2.71828)
const log_result = Math::log(10.0) # result of log function

# optional static chaining with '?:'
const safe_e_value = Math?:_e_ # null

# force static access with '!:'
const force_e_value = Math!:E # 2.71828
# const force_does_not_exist = Math::DoesNotExist!: # panics

# checking for null
if maybe_a_string is null
  print("The string is null")
else
  print("The string is:", maybe_a_string!)

# using Option type explicitly (similar to Rust's Option<T>)
const optional_value: Option<int> = Some(10)
const absent_value: Option<str> = null

match optional_value
  Some(value) -> print("Got a value:", value)
  null -> print("No value here")

match absent_value
  Some(text) -> print("Got text:", text)
  null -> print("Absent text")
```

## Data Structures

All data structures are immutable by default, but can be made mutable by explicitly adding pipes in between the brackets.

### Lists

```
# immutable list (default):
const numbers: []int = [1, 2, 3, 4, 5]
# numbers.push(6) # Error: Immutable list cannot be modified
numbers += [7] # returns a new immutable list and assigns it to "numbers"
const first = numbers[0]
const slice = numbers[1...3] # [2, 3]
const last = numbers[-1]

# const concatenated = first + last # Error: Cannot directly concatenate numbers
const concatenated = [first] + [last]
const reversed = numbers[..-1] # Slicing returns an immutable list

# checking for value presence
assert 7 in numbers
assert 8 !in numbers

# immutable list (fixed size - less common syntax, prefer tuples for fixed size):
const immutable_numbers: [int * 3] = [1, 2, 3]
const immutable_strings = ["apple", "banana", "cherry"]

# mutable list (dynamically sized):
var mutable_numbers: mut []int = [|10, 20, 30|]
var mutable_strings: mut []str = [|"red", "green", "blue"|]

mutable_numbers.push(40) # [|10, 20, 30, 40|]
mutable_numbers += [50]
mutable_numbers.insert(1, 15) # [|10, 15, 20, 30, 40|]
mutable_numbers.remove(3) # [|10, 15, 20, 40|] (removes element at index 3)
mutable_numbers.remove(15) # [|10, 20, 40|] (removes the first occurrence of 15)

# list of mixed types (gradual typing)
# "dyn" corresponds to "any" in TypeScript
const mixed_data = [|1, "hello", true|]
var mixed_list: mut []dyn = [|3.14, false, `world`|]

mixed_list.push("another")

# creating lists with repetition:
const zeros = [0] * 5 # [0, 0, 0, 0, 0]
var repeated_string: mut []str = [|"kiara"|] * 2 # [|"kiara", "kiara"|]
repeated_string.push("again")

# accessing elements:
const first_number = immutable_numbers[0] # 1
var second_string = mutable_strings[1] # "green"
mutable_strings[1] = "yellow" # [|"red", "yellow", "blue"|]

# list operations:
const numbers_1 = [1, 2, 3]
const numbers_2 = [4, 5]
const combined = numbers_1 + numbers_2 # [1, 2, 3, 4, 5] (creates a new immutable list)
const sliced = combined[1...4] # [2, 3, 4] (creates a new immutable list)
const length_of_list = combined::len # 5
const contains_5 = 5 in combined # true
const contains_y = 6 !in combined

# iterating over lists:
for const num in immutable_numbers
  print($"Number: $num")

for const (index, value) in mutable_strings::entries
  print($"Index: $index, Value: $value")

# list comprehensions (similar to Python), with LINQ syntax
const squares = [*from const x in 1...5 select x ** 2] # [1, 4, 9, 16, 25]
const even_numbers = [*from const n in 1...10 where n % 2 == 0 select n] # [2, 4, 6, 8, 10]

# higher-order functions (assuming these methods exist):
const doubled = numbers_1.map(|x| x * 2) # [2, 4, 6]
const even = numbers_1.filter(|x| x % 2 == 0) # [2]
const sum = numbers_1.reduce(0, |acc, x| acc + x) # 6

# destructuring lists:
const [a, b, c] = immutable_numbers # a = 1, b = 2, c = 3
# prefix * is the spread operator
const [first_m, *rest_m] = mutable_strings # first_m = "red", rest_m = [|"yellow", "blue"|]
```

### Tuples

```
# Tuples are fixed-size, ordered collections of elements of potentially different types
const my_tuple = (1, "hello", 3.14)
const point: (int, int) = (10, 20)
const color: (u8, u8, u8) = (255u8, 0u8, 128u8)

# Accessing tuple elements by index
const first_element = my_tuple.0 # 1
const second_element = my_tuple.1 # "hello"

# Destructuring tuples
const (x, y) = point # x = 10, y = 20
const (r, g, b) = color # r = 255, g = 0, b = 128

# Named tuples (similar to structs but anonymous)
const person_info = (name: "Alice", age: 30)
const person_name = person_info.name # "Alice"
const person_age = person_info.age   # 30

# Tuples as function return values
func get_coordinates(): (int, int) =
  return (5, 12)

const (lat, lon) = get_coordinates()
print($"Latitude: $lat, Longitude: $lon")

# Single-element tuple (note the trailing comma)
const single = (42,)
const single_value = single.0 # 42
```

### Sets

Sets are unordered collections of unique elements. Both `in` and `of` can be used to check for set presence since the keys and values are paired with each other.

```
# Immutable Set (default):
const my_set: {}int = {1, 2, 3, 2, 1} # {1, 2, 3} (duplicates are removed)
const string_set_imm = {"apple", "banana", "apple", "orange"} # {"apple", "banana", "orange"}

# Mutable Set:
var string_set: mut {}str = {|"apple", "banana", "apple", "orange"|} # {|"apple", "banana", "orange"|}

string_set.add("grape") # {|"apple", "banana", "orange", "grape"|}
string_set.remove("banana") # {|"apple", "orange", "grape"|}

const has_apple = "apple" of string_set # true
const size = string_set::len # 3

const set1 = {1, 2, 3}
const set2 = {3, 4, 5}

const union_set = set1 | set2 # {1, 2, 3, 4, 5}
const intersection_set = set1 & set2 # $3
const difference_set = set1 - set2 # {1, 2}
const symmetric_difference_set = set1 ^ set2 # {1, 2, 4, 5}

# Iterating over sets:
for const item of my_set
  print($"Set item: $item") # order is not guaranteed

# Set comprehensions:
const squared_evens = {*from const x in 1...10 if x % 2 == 0 -> x * x} # {4, 16, 36, 64, 100}
```

### Maps

Maps are collections of key-value pairs.

```
# immutable Map (default):
const my_map: {str: int} = {"apple": 1, "banana": 2, "cherry": 3}

# mutable Map:
var user_roles: mut {str: str} = {|"alice": "admin", "bob": "user"|}

# accessing values by key:
const apple_count = my_map["apple"] # 1
const bob_role = user_roles."bob"  # "user"

# adding and updating key-value pairs:
user_roles["charlie"] = "guest" # {|"alice": "admin", "bob": "user", "charlie": "guest"|}
user_roles."bob" = "editor" # {|"alice": "admin", "editor", "charlie": "guest"|}

# removing key-value pairs:
user_roles::remove("alice") # {|"bob": "editor", "charlie": "guest"|}

# checking if a key exists:
const has_banana = "banana" of my_map # true
const has_qty_2 = 2 in my_map::values # true

# getting all keys and values:
const keys = my_map::keys # ["apple", "banana", "cherry"] (order not guaranteed, returns immutable list)
const values = my_map::values # [1, 2, 3] (order not guaranteed, returns immutable list)

# iterating over maps; order of keys is not guaranteed
for const key of my_map
  print($"Key: $key" )
for const value in my_map
  print($"Value: $value")
for const key, value from my_map
  print($"Key: $key, Value: $value")

# map comprehensions:
const squared_map = {*from const num in 1.5 select num: num ** 2}
# {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
const filtered_map = {*from const k, v in my_map where v > 1 select k: v}
# {"banana": 2, "cherry": 3}

# default values with ::get
const mango_count = my_map::get("mango", 0)    # 0 (mango doesn't exist)
const cherry_count = my_map::get("cherry", -1) # 3

# merging maps (assuming '+' operator is defined for maps):
const map1 = {"a": 1, "b": 2}
const map2 = {"b": 3, "c": 4}
const merged_map = map1 + map2 # {"a": 1, "b": 3, "c": 4} (later keys overwrite earlier ones, returns new immutable map)

# set-like operations on map based on keys
const union_map = map1 | map2 # {"a": 1, "b": 3, "c": 4}
const intersection_map = map1 & map2 # {"b": 2} (keeps values from the first map)
const difference_map = map1 - map2 # {"a": 1}
const symmetric_difference_map = map1 ^ map2 # {"a": 1, "c": 4}
```

### LINQ

```
const numbers = [1, 2, 3, 4, 5]

# select (map)
const squares = from const x in numbers
  select x ** 2 # [1, 4, 9, 16, 25]

# where (filter)
const even_numbers = from const n in 1...10
  where n % 2 == 0
  select n # [2, 4, 6, 8, 10]

# select many (flat map)
const nested_lists = [[1, 2], [3, 4, 5], [6]]
const flattened = from const sublist in nested_lists
  from item in sublist
  select item # [1, 2, 3, 4, 5, 6]

# sort by (sort) - ascending by default
const unsorted_names = ["Charlie", "Alice", "Bob"]
const sorted_names = from const name in unsorted_names
  sort by name # asc
  select name
# ["Alice", "Bob", "Charlie"]
const sorted_desc = from const name in unsorted_names
  sort by name desc
  select name # ["Charlie", "Bob", "Alice"]

# groupby
const people = [
  { name: "Alice", age: 30 }
  { name: "Bob", age: 25 }
  { name: "Charlie", age: 30 }
]
const grouped_by_age = from const person in people
  group by person.age
  select { Age: key, People: group }
# [{ Age: 30, People: [{ name: "Alice", age: 30 }, { name: "Charlie", age: 30 }] }, { Age: 25, People: [{ name: "Bob", age: 25 }] }]

# let (introduce a new variable within the query)
const numbers_plus_one = from const x in numbers
  let plus_one = x + 1
  select plus_one * 2 # [4, 6, 8, 10, 12]

# join (inner join)
const employees = [
  { id: 1, name: "Alice", dept_id: 10 }
  { id: 2, name: "Bob", dept_id: 20 } ]
const departments = [
  { id: 10, name: "HR" },
  { id: 20, name: "Engineering" }]
const joined_data = from const emp in employees
  join const dept in departments
  on emp.dept_id == dept.id
  select { Employee: emp.name, Department: dept.name }
# [{ Employee: "Alice", Department: "HR" }, { Employee: "Bob", Department: "Engineering" }]

# into (continue a query with a new range variable)
const even_squares = from x in numbers
  where x % 2 == 0
  select x * x into const even_sq
  from const sq in even_sq
  where sq > 4 select sq
# [16]

# comprehensions can create lists, sets, or maps
const set_of_squares = {*from x in numbers select x ** 2} # {1, 4, 9, 16, 25}
const map_of_squares = {*from x in numbers select x: x ** 2} # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# multiple from clauses for cross-product
const colors = ["red", "green"]
const sizes = ["small", "large"]
const combinations = from const c in colors
  from s in sizes select $"$c-$s" # ["red-small", "red-large", "green-small", "green-large"]

# using the 'it' keyword to refer to the current item when the selector is simple
const doubled_it = from const x in numbers
  select it * 2 # [2, 4, 6, 8, 10]
const even_it = from const n in 1...10
  where it % 2 == 0 select it # [2, 4, 6, 8, 10]
```

## Control Flow

Use indentation to define blocks of code. A colon or the following keywords are needed if the statement is written on the same line:

- `if` ends with `then`
- `for` and `while` end with `do`
- `then` ends with `do`
- `catch` ends with `do` or `with`, the latter implicitly begins a `match` block
- `match` ends with `with`

These keywords are self-closing:

- `scope` (closure or IIFE) with an optional label
- `else` if not inside a `match` block
- `try` and `final`
- `next w`/`end x`/`redo y`/`retry z` with an optional label

### Conditionals

```
var x = 7
var y = 10

# indentation for code blocks
if x > 5
  x = 10
else
  x = 20

# use a colon to separate the statement
if x > 5: x = 10 else x = 20
if x > 5 then x = 10 else x = 20

# postfix 'if' is also supported for conciseness
x = 10 if x < 5 else 2

if y > 5 then y -= 5
print(y) # output 5

# if! rather than "unless"
if! x < 5
  print("x is not less than 5")
else
  print("x is less than 5")
# elif == 'else if'
if age >= 18: print("Adult")
elif age > 13: print("Teenager")
else print("Child")

if age < 13
  print("Still a child")

var value = 10
if! value < 5
  print("Value is not less than 5")
elif! value > 15
  print("Value is not greater than 15")
else
  print("Value is between 5 and 15 (inclusive)")
```

### Loops

```
# LOOPS
var counter = 0
for const i in 1...5 do counter += i
for const i in 1...5: counter += i
counter += i for const i in 1...5 # Ruby-style?
print("Sum:", counter) # Output: Sum: 15

# classic C-style for loops:
for var i = 0; i < 5; i += 1
  print($"C-style for: $i")

var j = 0
while 7 > j: j += 1
while j < 15 do j += 1
j += 1 while j < 25 # postfix style

print("j:", j) # Output: j: 5

# while! rather than until
while! j == 5 do j += 1
print("j after while!:", j)

loop: end # this does nothing
loop end # this also does nothing

loop
  print("This will run once")
  end # end == 'break'

var k = 0
loop
  print("k:", k)
  k += 1
while k < 3

var j = 0
while j < 5
  j += 1
  if j == 3 then next # next == 'continue'
  print($"j is: $j")

import Sys
loop
  var input = Sys.read(1)
  print($"Input: $input")
  if input == 5 then end
  i += 1
  next # This 'next' will be executed unless the 'end' is reached

for const i in 0...10
  if i % 2 == 0 then next # Skip even numbers
  print($"Odd number: $i")

# labeled loops
scope outer for i in 0...3
  scope inner for j in 0...3
    if i == 1 && j == 1 then end outer
    print($"i: $i, j: $j")

var count = 0
loop
  count += 1
  print($"Count: $count")
  if count < 3 then redo # restart the loop iteration
  end
print("Loop finished") # Output: Count: 1, Count: 2, Count: 3, Loop finished

var index = 0
for var item in ["apple", "banana", "cherry"]
  index += 1
  if item == "banana" then redo # process "banana" again
  print($"Item $index: $item")
# Output: Item 1: apple, Item 2: banana, Item 3: banana, Item 4: cherry

# ITERATORS
for const item in [1, 2, 3]::iter
  print($"Iterator item: $item")

for const (index, value) in ["a", "b", "c"]::indexed
  print($"Index: $index, Value: $value")
```

### Pattern Matching

Match expressions are similar to switch case but far more powerful.

```
# inline match expression
const result = match expr with Some(x) -> x, else -> null

const day = 3
match day
  1 -> print("Monday")
  2 -> print("Tuesday")
  3 -> print("Wednesday")
  4 -> print("Thursday")
  5 -> print("Friday")
  6 or 7 -> print("Weekend") # Multiple values (union)
  n if n > 7 -> print("Invalid day") # Guard condition
  else -> print("Unknown day") # Default case

const result = Some(42)
match result
  Some(value) -> print($"Got a value: $value")
  null -> print("No value")

const point = (10, 20)
match point
  (0, 0) -> print("Origin")
  (x, 0) -> print($"On the x-axis at $x")
  (0, y) -> print($"On the y-axis at $y")
  (x, y) -> print($"At coordinates ($x, $y)")

const data = ["error", 404]
match data
  ["ok", code] -> print($"Success with code: $code")
  ["error", 500...600] -> print("Server error") # Range matching
  ["error", code] -> print($"Error with code: $code")
  [status, else] -> print($"Status: $status") # Wildcard

const log_entry = "ERROR [Server]: Connection refused"
const error_regex = /^(ERROR|WARN) \[(.+?)\]: (.+)$/
match log_entry
  (level, component, message) in error_regex ->
    print($"[$level] ($component): $message")
  else -> print("Not a recognized log entry format")

const text = "Name: Alice, Age: 30"
match text
  /Name: (?<name>\w+), Age: (?<age>\d+)/ -> print($"Name: $name, Age: $age")
  else -> print("No match found with capturing groups")

# infix match
const person = text match
  /Name: (?<name>\w+), Age: (?<age>\d+)/ -> $"Name: $name, Age: $age"
  else -> "No match found with capturing groups"
```

### Error Handling

Catch statement can either be followed by an implicit `do` or explicit `with`, the latter which opens a `match` statement.

```
var attempts = 0
const max_attempts = 3
loop
  try # might throw an error
    attempts += 1
    print($"Attempt: $attempts")
    if attempts < 3 then throw "Temporary failure!" else throw "Final success!"
  catch const err
    console::error($"Caught an error: $err")
    if attempts < max_attempts then retry else console::error("Max retries reached.")
  final
    print("HTTP operation finished (with potential errors).")
    end # Exit the loop after final block

try
  var status_code = 503
  if status_code == 503 then
    throw status_code
  else status_code = 200
catch const err with
  200 -> print("OK")
  404 -> print("Not Found")
  503 ->
    print("Service Unavailable, retrying...")
    retry 2 # Retry the 'try' block a maximum of 2 times
  else -> print($"Unknown Status: $err")
# 'final' block is optional
```

### Asynchronous Operations

```
# await...then is similar to 'try'
async const x = await fetch("https:#example.com/data.json")
  then const response do response.json()
  catch const error
    error("Error fetching data:", error)
  final print("Fetch operation completed.")

# use 'for await...in/of/from' for asynchronous iteration
async func process_list(arr: []) =
  for await const item of arr
    print($"Processing item: $item")
    await new Promise(|resolve| setTimeout(resolve, 500))

async func process_stream(stream_url: str) =
  for await const chunk in fetch(stream_url)::readable
    const decoder = new TextDecoder()
    const text_chunk = decoder.decode(chunk)
    print($"Received chunk: $text_chunk")
  print("Stream finished.")

async func process_map_entries(map: $any: any) =
  for await const (key, value) from map
    print($"Key: $key, Value: $value")
    await new Promise(|resolve| setTimeout(resolve, 300))
```

### Everything is an expression

```
const is_even = if number % 2 == 0 then true else false
const message = match status_code
  200 -> "OK"
  404 -> "Not Found"
  else -> "Unknown Status"

# Try
const x = try 1 / 0 catch err do
const x = await fetch('https:#example.com/data.json)
  then response do response.json
  catch err

# panic keyword
func panic_func(condition: bool) =
  if! condition then panic "Condition failed"
  print("Condition passed")

# panic_func(false) # This will panic
panic_func(true)
```

## Functions

Functions are first-class, like any other vaule. You can also define your own custom functions and assign them to operators with the `infix`, `prefix`, or `suffix` keywords before `func`.

```
func add(a: int, b: int): int =
  a + b

# default arguments
func greet(name: str = "Guest"): str =
  $"Hello, $name!"

const result = add(5, 3)
print(result) # Output: 8

const greeting = greet("Alice")
print(greeting) # Output: "Hello, Alice!"

# higher order functions
func multiply_by(factor: int): (x: int) -> int =
  |x: int|: int -> x * factor

const double = multiply_by(2)
print(double(5)) # Output: 10

const triple = multiply_by(3)
print(triple(5)) # Output: 15

# Function that takes a function and applies it twice
func apply_twice(f: (x: any) -> any, x: any): any =
  x |> f |> f

const square = |x: int|: int -> x * x
const result = apply_twice(square, 2)
print(result) # Output: 16

func decorator(f: func (x: any) -> any, x: any): any =
  print($"Calling f(x)")
  f(x)

const decorated_add = |x: int, y: int|: int ->
  decorator(|a: int, b: int|: int -> a + b, x + y)
const result = decorated_add(5, 3)
print(result) # Output might include "Calling <func>(8)": 8

const numbers = [1, 2, 3, 4]
# use a lambda with map to process each element in the array
const squared_numbers = numbers |> |x| -> x * x |> list
print(squared_numbers) # Output: [1, 4, 9, 16]

# define function composition operator
infix func [+>] (
  f: (x: any) -> any,
  g: (y: any) -> any
): (z: any) -> any =
  |z: any|: any -> z |> g |> f

func add_one(x: int): int = x + 1
func double(x: int): int = x * 2
func subtract_two(x: int): int = x - 2

# compose the functions using '+>'
const composed_func = double +> add_one

const result = composed_func(5)
print(result) # Output: 11

# more complex composition
const composed_func_2 = subtract_two +> double +> add_one

const result_2 = composed_func_2(5)
print(result_2) # Output: 13
```

## Classes

Classes are blueprints for creating objects, encapsulating data (fields/properties) and behavior (methods).

```
class Rectangle
  var width: int
  var height: int

  # constructor. always `void`
  new(w: int, h: int) =
    width = w
    height = h
  func area(): int = width * height
  func is_square(): bool = width == height

  get width(): int = width
  set width(new_width: int) =
    width = new_width

  get height(): int = height
  set height(new_height: int) =
    height = new_height

  static func create_square(side: int): Rectangle =
    new Rectangle(side, side)

const rect1 = new Rectangle(10, 5)
print($"Area of rect1: {rect1.area()}") # Output: Area of rect1: 50
print($"Is rect1 a square? {rect1.is_square()}") # Output: Is rect1 a square? false

rect1.set_width(5)
print($"New width of rect1: {rect1.get_width()}") # Output: New width of rect1: 5
print($"Is rect1 now a square? {rect1.is_square()}") # Output: Is rect1 now a square? true

const square1 = Rectangle::create_square(7)
print($"Area of square1: {square1.area()}") # Output: Area of square1: 49

# Inheritance (using 'of')
class Shape
  var color: str
  new(c: str) =
    color = c
  func describe(): str = $"A shape with color $color"

class Circle of Shape
  var radius: int
  new(r: int, c: str) =
    super(c) # Call the constructor of the superclass
    radius = r
  func area(): float =
    return Math::PI * radius ** 2
  redef func describe(): str =
    return $"A circle with radius $radius and color $color"

const circle1 = new Circle(3, "blue")
print(circle1.describe()) # Output: A circle with radius 3 and color blue
print($"Area of circle1: {circle1.area()}") # Output: Area of circle1: 28.274333882308138

# Abstract classes (using 'abs')
abs class Vehicle
  abs func start(): void
  abs func stop(): void

class Car of Vehicle
  var model: str
  new(m: str) =
    model = m
  redef func start() =
    print($"Car model $model started.")
  redef func stop() =
    print($"Car model $model stopped.")

# const my_vehicle = new Vehicle() # Error: Cannot instantiate abstract class

const my_car = new Car("Sedan")
my_car.start() # Output: Car model Sedan started.
my_car.stop() # Output: Car model Sedan stopped.
```

## Structs

Structs are value types (copied on assignment) that can hold data members. They are similar to classes but typically used for simpler data aggregates.

```
struct Point
  var x: int
  var y: int
  new(x_val: int, y_val: int) =
    x = x_val
    y = y_val
  func distance_from_origin(): float =
    Math::sqrt(x ** 2 + y ** 2)

const p1 = new Point(3, 4)
const p2 = p1 # p2 is a copy of p1
p2.x = 10

print($"p1: ({p1.x}, {p1.y})") # Output: p1: (3, 4)
print($"p2: ({p2.x}, {p2.y})") # Output: p2: (10, 4)
print($"Distance of p1 from origin: {p1.distance_from_origin()}") # Output: Distance of p1 from origin: 5.0

# Structs can also have static methods
struct CircleConfig
  var radius: float
  var color: str
  static func default(): CircleConfig =
    new CircleConfig(1.0, "white")

const default_config = CircleConfig::default()
print($"Default radius: {default_config.radius}, color: {default_config.color}") # Output: Default radius: 1.0, color: white
```

## Traits

Traits (similar to interfaces or mixins in other languages) define a set of methods that a class or struct can implement. They allow for a form of multiple inheritance of behavior.

```
trait Loggable
  func log(message: str)

class User with Loggable
  var name: str
  new(n: str) =
    name = n
  func log(message: str) =
    print($"[USER] $name: $message")

class Product with Loggable
  var id: int
  var description: str
  new(i: int, d: str) =
    id = i
    description = d
  func log(message: str) =
    print($"[PRODUCT] ID $id: $message")

func process_item(item: Loggable, action: str) =
  item.log($"Performing action: $action")

const user1 = new User("Bob")
const product1 = new Product(123, "Awesome widget")

process_item(user1, "login") # Output: [USER] Bob: Performing action: login
process_item(product1, "add to cart") # Output: [PRODUCT] ID 123: Performing action: add to cart

# Traits can also define default implementations for methods
trait Printable
  func print() =
    print("Default printing behavior")

class Document implements Printable
  var content: str
  new(c: str) =
    content = c
  shadow func print() = # implicit override
    print($"Document content: $content")

class Image implements Printable
  var filename: str
  new(f: str) =
    filename = f

const doc = new Document("This is the document content.")
doc.print() # Output: Document content: This is the document content.

const img = new Image("photo.jpg")
img.print() # Output: Default printing behavior (using default implementation from Printable)
```

## Enums

Enums (enumerations) define a set of named constants.

```
enum Status
  OK
  WARNING
  ERROR
  NOT_FOUND

func get_status_message(status: Status): str =
  match status
    Status::OK -> "Operation successful"
    Status::WARNING -> "Something might be wrong"
    Status::ERROR -> "An error occurred"
    Status::NOT_FOUND -> "Resource not found"

const current_status = Status::WARNING
print($"Current status: $current_status") # Output: Current status: WARNING
print($"Status message: {get_status_message(current_status)}") # Output: Status message: Something might be wrong

# Enums can also have associated values (similar to algebraic data types)
enum Result[T] # Generic enum
  Ok(value: T)
  Err(message: str)

func divide(a: int, b: int): Result[float] =
  if b == 0 then return Result::Err("Cannot divide by zero")
  else return Result::Ok(a / b as float)

const outcome1 = divide(10, 2)
match outcome1
  Result::Ok(val) -> print($"Division result: $val") # Output: Division result: 5.0
  Result::Err(msg) -> print($"Error: $msg")

const outcome2 = divide(5, 0)
match outcome2
  Result::Ok(val) -> print($"Division result: $val")
  Result::Err(msg) -> print($"Error: $msg") # Output: Error: Cannot divide by zero

# Enums with raw values
enum Color: str
  RED = "red"
  GREEN = "green"
  BLUE = "blue"

func get_color_code(color: Color): str =
  return color::value

const selected_color = Color::GREEN
print($"Selected color: $selected_color") # Output: Selected color: GREEN
print($"Color code: {get_color_code(selected_color)}") # Output: Color code: green
```

## Unions

Unions allow a variable to hold a value of one of several possible types. The size of the union is typically determined by the size of its largest member.

```
union Variant
  int_val: int
  float_val: float
  string_val: str

func print_variant(v: Variant) =
  # It's crucial to know the active type before accessing
  # Here, we'll assume we have a way to track the active type
  # (This tracking mechanism is not built-in to the union itself)
  if v has int_val
    print($"Integer: {v.int_val}")
  elif v has float_val
    print($"Float: {v.float_val}")
  elif v has string_val
    print($"String: {v.string_val}")
  else
    print("Unknown variant type")

var my_variant: Variant
my_variant.int_val = 10
print_variant(my_variant) # Output: Integer: 10

my_variant.float_val = 3.14
print_variant(my_variant) # Output: Float: 3.14

my_variant.string_val = "hello"
print_variant(my_variant) # Output: String: hello

# Be cautious: Directly accessing a non-active member is unsafe
# print(my_variant.int_val) # This would likely produce garbage or an error
```

oh, and elem and style blocks, these are for ui elements defined in HAML and Stylus blocks respectively

## JSX (HAML + Stylus)

This code defines a simple game with a player that falls due to gravity.

```
# main.ki
import Graphics, Input, Audio

const GRAVITY: f32 = 9.81
struct Player =
  x: f32
  y: f32
  speed: f32
  name: str

func create_player(name: str, start_x: f32, start_y: f32): Player =
  Player { x: start_x, y: start_y, speed: 5.0, name }

elem GameEntity(entity: Player) =
  %div(style={
    position: absolute
    left: $(entity.x)px
    top: $(entity.y)px
  })
    %img(src="./player.png", alt=$entity.name)

elem Game() =
  const player = create_player("Hero", 50.0, 100.0)

  func handle_input(event: InputEvent): void =
    match event.key
      'ArrowRight' -> player.x = player.x + player.speed
      'ArrowLeft' -> player.x = player.x - player.speed
      else -> void

  func game_loop(): void =
    player.y = player.y + GRAVITY * delta_time()
    %GameEntity(entity=$player)
    request_frame(game_loop)

  # Start the game loop (Kiara might have a specific lifecycle hook)
  on_mount! do
    Input.add_listener("keydown", handle_input)
    game_loop()

  # Render the game scene
  %div#gameContainer(style={
    width: 800px
    height: 600px
    border: 1px solid black})
    %GameEntity(entity=$player)

elem App(args: []str) =
  %body
    %h1 Yo, Kiara is running!
    %Game
```

Product lists

```
# ui_example.ki

struct Item =
  name: str
  price: f32

const items = [
  Item { name: "Laptop", price: 1200.0 }
  Item { name: "Mouse", price: 25.0 }
  Item { name: "Keyboard", price: 75.0 }
]

elem ItemDisplay(item: Item) =
  %div.item-card(style={
    border: 1px solid #ccc
    padding: 10px
    margin-bottom: 5px
  })
    %h3 {$item.name}
    %p Price: ${item.price}

elem ProductList() =
  %div#product-list
    %h2 Our Products
    for const item in items do
      %ItemDisplay(item=$item)
    %p.total {$"Total number of items: \$$len(items)"}
    if len(items) > 0 do
      %button(onClick=|event| -> print("Checkout clicked!")) Checkout
    else
      %p No items available.

elem App(args: []str) =
  %body
    %h1 Welcome to our Store!
    %ProductList
```

With styles:

```
elem StyledComponent() =
  style StyledComponent =
    $primary-color = #007bff
    $border-radius = 5px

    .container
      background-color: #f8f9fa
      padding: 20px
      border: 1px solid #ccc
      border-radius: $border-radius
      margin-bottom: 10px

      h2
        color: $primary-color
        margin-top: 0

    button
      background-color: $primary-color
      color: white
      border: none
      padding: 10px 15px
      border-radius: $border-radius
      cursor: pointer

      &:hover # Nesting for pseudo-classes
        background-color: darken($primary-color, 10%)

  %div.container
    %h2 Styled Content
    %button Click Me

elem App(args: []str) =
  %body
    %StyledComponent
```

### GraphQL query/

```
# GraphQL-like schema and query syntax
# (without template literals)
schema Project
  name: str
  tagline: str
  contributors: []User

schema User
  id: int
  username: str
  email: str

let querySample = query
  project(name: "Kiara")
    tagline

let anotherQuery = query
  user(id: 123)
    username
    email

let projectWithContributors = query
  project(name: "Kiara")
    name
    tagline
    contributors
      username
```

### Verification syntax

`\` continues a line.

```
func divide(a: int, b: int): int \
  require b != 0 =
  a / b

func increment(mut x: int): int \
  ensure result == old(x) + 1 =
  x += 1
  x

func sum_up_to(n: int): int =
  var i = 0
  var sum = 0
  while i <= n where \
    invar 0 <= i <= n \
    invar sum == (i * (i - 1)) / 2
    sum += i
    i += 1
  sum

func process(arr: []int): int =
  var sum = 0
  for const x in arr: sum += x
  assert sum >= 0
  sum

lemma add_commutative(a: int, b: int) \
  ensure add_commutative(a, b) == add_commutative(b, a)
  # ... Proof steps

func add(a: int, b: int): int \
  ensure result == a + b =
  assert add(a, b) == add(b, a) by add_commutative(a, b)
  a + b

func find_index(arr: []int, target: int): ?int =
  ghost var found_index: ?int = null
  for var i in 0..arr.len() where \
    ensure found_index == (if result? then result else null)
    if arr[i] == target
      found_index = Some(i)
      return Some(i)
  return null
```
