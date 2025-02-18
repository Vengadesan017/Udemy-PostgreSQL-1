# Basics
- select upper('hello')
- select lower('Hello')
- select initcap('hello')
- select char_length('hello')
- select trim(' hello ')
- select char_length(trim(' hello '))

## RE - notations
- . - find any char expect new line
- [FGz] - any char like F or G or z
- [a-z] - lowercase a to z
- [^a-z] - not lowercase a to z
- \w - any work like [A-Za-z0-9]
- \d - any digit
- \t - tab char
- \s - space
- \n - newline
- \r - carriage return character
- \r\n  - used in large db
- ^ - match at start of string
- $ - match at end of string
- ? - get preceding match zero or one time
- * - get preceding match zero or more time
- + - get preceding match one or more time
- {m} - get preceding match exactly m times
- {m,n} - get preceding match between m and n times
- a|b - either a or b
- ( ) - create and report a capture grup or set prcedure
- (?:) - Negate the reporting of a capyure group
