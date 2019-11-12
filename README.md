# prejoice

Prejoice is a JSON preprocessor for Node.js. Many features take inspiration from [Pug](https://github.com/pugjs/pug).

## Features

- No more quotations or commas (unless you want them)
- Whitespace in keys
- Comments
- Variables
- Strings
- Arrays
- Booleans

### Syntax

Prejoice blocks are written using whitespace. Tabs, two spaces, and 4 spaces are all acceptable.

An example Prejoice file:

```
# User data for online store
id: 33224455
name:
  first: Bill
  last: Gates
address:
  street: 123 Sesame Street
  city: Seattle
  state: WA
  country: US
  zip: 12345
// Premium users get special treatment :)
is premium: true
```

Prejoice will compile this into the following JSON:

```json
{
  "id": 33224455,
  "name": {
    "first": "Bill",
    "last": "Gates"
  },
  "address": {
    "street": "123 Sesame Street",
    "city": "Seattle",
    "state": "WA",
    "country": "US",
    "zip": 12345
  },
  "is_premium": true
}
```

### Quotations & commas

Quotations and commas are **not** needed for Prejoice. You can use quotations for keys if you wish, however using commas will cause the value to be interpreted as a string.

### Whitespace in keys

Any whitespace in keys will be converted to an underscore.

```
team name: The Goal Scorers
```
This will produce:
```json
{
  "team_name": "The Goal Scorers"
}
```

### Comments

Prejoice supports both single line and multiline comments, as well as inline comments.

Single line or inline comments are declared with either `#` or `//`. To use these values as a part of a string value, simply escape them with a backslash.

Multiline comments begin with the same characters, but with no other content on the same line. Instead, indented whitespace on the any line following these will count as a comment.

```
team name: The Programmers # The best team
caption: Family vacation! \#vacation \#fun
#
  This is a multiline comment.
  The compiler will ignore all of this.
  However, the next line will be compiled:
age: 23
```
This will produce:
```json
{
  "team_name": "The Programmers",
  "caption": "Family Vacation! #vacation #fun",
  "age": 23
}
```

### Variables

Variables allow for reusing common values. They are declared by using a `$`. Null values are the same as regular JSON.

Prejoice variables can also be JavaScript. This lets you run string functions and concatenate strings.

Unlike Prejoice values, string variables must use quotations (single or double).

```
$name = 'Joe'
team:
  name: Web Dev Supreme
  captain: $name

$foo = 'Foo'
$bar = 'Bar'
$foobar = $foo + ' and ' + $bar
baz: $foobar

middle name: null
```
This will produce the following JSON:
```json
{
  "team": {
    "name": "Web Dev Supreme",
    "captain": "Joe"
  },
  "baz": "Foo and Bar",
  "middle_name": null
}
```

### Strings

String values do not need quotations. Multiline strings can be declared in a similar way to multiline comments. When compiled, the value will turn into a single line with spaces where the newlines were.

```
name: Michael Scott
bio:
  Manager of Dunder Mifflin Paper Company Scranton. Has
  an excentric personality. An aspiring director and
  improv enthusiast, Scott directed his own film.
```
This will produce:
```json
{
  "name": "Michael Scott",
  "bio": "Manager of Dunder Mifflin Paper Company Scranton. Has an excentric personality. An aspiring director and improv enthusiast, Scott directed his own film."
}
```

### Arrays

Prejoice arrays are declared by using `[]` as a value. Elements are declared with a hyphen and can be a value or a block of data:

```
fruits: []
  - Apple
  - Orange
  - Banana

books: []
  -
    title: JSON for Dummies
    author: Some Guy
    price: 350
  -
    title: Pride and Prejudice
    author: Another P. Erson
    price: 899
```
This will produce:
```json
{
  "fruits": [
    "Apple",
    "Orange",
    "Banana"
  ],
  "books": [
    {
      "title": "JSON for Dummies",
      "author": "Some Guy",
      "price": 350
    },
    {
      "title": "Pride and Prejudice",
      "author": "Another P. Erson",
      "price": 899
    }
  ]
}
```

### Booleans

Booleans can use a variety of case-insensitive keywords:

```
premium: true
dark theme: yes
mailing list: false
multi factor: No
```
This will produce:
```json
{
  "premium": true,
  "dark_theme": true,
  "mailing_list": false,
  "multi_factor": false
}
```
