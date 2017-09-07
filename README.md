
# Testing password requirements using regular expressions
__Regular expressions can be used to verify that a given password meets certain validation criteria.__ 

- *Consider these requirements*:
  - Passwords must
    - be at least 8 characters long
    - contain at least one lowercase letter
    - contain at least one uppercase letter
    - contain at least one digit
    - contain at least one special character
    - contain at least one "word"

---
# TL;DR

__Regular expressions (regex) provide a concise way to look for patterns in strings.__
- Strength: __concise__ (doesn't require a lot of code) 
- Weakness: __concise__ (uses dense, cryptic code)
  
__For the purposes of meeting the above requirements__

``` javascript
  // store password value
  var pw = 'Pa$$w0rdString';

```
- The `match()` function is your friend.
  - The `.match()` function takes a *pattern* as a parameter and returns an *array* whose elements match the *pattern*.
  ``` javascript
  // Use the global flag `g` to return all occurences that match the pattern
  pw.match(/r/g);
    (2) ["r", "r"]
      0: "r"
      1: "r"
      length: 2
      __proto__: Array(0)
      
  ```
  - The `match()` function returns `null` if the *pattern* does not exist within the given string
  ```javascript
  pw.match(/x/); // returns null
  
  ```
      
## Test password *length* requirement
There is a way to do this with regex, but it may be easier to use the `String.prototype.length` property.
``` javascript
  // store password length requirement
  var minPWLength = 8;

```

``` javascript
  pw.match(/./g).length > minPWLength; // returns true

  // OR
  
  pw.length > minPWLength; // returns true

```

## Test minimum *lowercase* letter requirement
``` javascript
  // Use a-z range as a set
  pw.match(/[a-z]/g)) !== null; // returns true

```

## Test minimum *UPPERCASE* letter requirement
``` javascript
  // Use A-Z range as a set
  pw.match(/[A-Z]/g)) !== null; // returns true

```

## Test minimum *digit* requirement
``` javascript
  // Use 0-9 range as a set
  pw.match(/[0-9]/g)) !== null;// returns true

```

## Test minimum *special character* requirement
``` javascript
  // Defintion: special character is anything that is not a letter or number
``` 

``` javascript
  // Use a negated combined range as a set
  // ^a-zA-Z0-9 
  pw.match(/[^a-zA-Z0-9]/g)) !== null; // returns true

```

## Test number of *word*s
``` javascript
  // Definition: "word" is defined as any consecutive series of letters
```

``` javascript
  // Use a-zA-Z range as a set
  // Use the `+` operator as a quantifier
  pw.match(/[a-zA-Z]+/g);

  // returns array whose elements are the occurence(s) of consecutive letters
    (3) ["Pa", "w", "rdString"]
      0: "Pa"
      1: "w"
      2: "rdString"

  // test length of array
  pw.match(/[a-zA-Z]+/g).length > 0; // returns true

```
### That's it! But for more, please read on!
---
## What is a Regular Expression?
A regular expression (regex) is a *__pattern__* used to match character combinations in a given string. 
Regular expressions can be useful for testing a password to see that it meets a number of requirements, 
which is the focus of this article.

## What is the structure of a Regular Expression?
``` javascript
  /<pattern>/[ <flags> ]
```
## How do I use them?
### Simple Patterns
- Patterns are required for effectively using Regular Expressions and vary widely depending on what you are looking for.
  - note that simple patterns are taken __literally__.
    - `/P/`   returns only the first occurence of __'P'__ in a given string.
    - `/Pa/`  returns only the first occurence of __'Pa'__ in a given string.
    - `/w0rdS/`  returns only the first occurence of __'w0rdS'__ in a given string.
    - `/0/` returns only the first occurence of the digit __0__ in a given string.

  - note the default behavior is to return only the first occurence that matches a pattern.


### Basic usage with the `match()` function
The `match()` function takes a regular expression, or pattern and (optional) flag(s) as parameter(s).
``` javascript
  .match(/ *pattern* / [ *flags* ])

```

The `match()` function returns an array.
``` javascript
  'Pa$$w0rdString'.match(/a/);
    ["a", index: 1, input: "Pa$$w0rdString"]
      0: "a"
      index: 1
      input: "Pa$$w0rdString"
      length: 1

```

In contrast, if you describe a __pattern__ that does __not__ exist in a given string, the `match()` function will return `null`.
``` javascript
  'Pa$$w0rdString'.match(/x/); // returns null

```

### The global flag
As noted above, the default behavior is to return only the first occurence of a matched *pattern*. 
- Appending __g__ (the global flag) to the *pattern*, will return all occurences.
``` javascript
  'Pa$$w0rdString'.match(/r/g);
    (2) ["r", "r"]
      0: "r"
      1: "r"
      length: 2
      
```

### Setting an OR condition
Use the `|` __pipe character__ in conjunction with the *global* flag to set an *__OR__* condition for a *pattern*.
``` javascript
  'Pa$$w0rdString'.match(/s|t|r/g);
    (3) ["r", "t", "r"]
      0: "r"
      1: "t"
      2: "r"
      length: 3
      
```

### Return everything
Use the `.` __dot__ option in conjunction with the *global* flag to return every character in a given string (including spaces and special characters).
``` javascript
  'Pa$$w0rdString'.match(/./g);
    (14) ["P", "a", "$", "$", "w", "0", "r", "d", "S", "t", "r", "i", "n", "g"]
      0: "P"
      1: "a"
      2: "$"
      3: "$"
      4: "w"
      5: "0"
      6: "r"
      7: "d"
      8: "S"
      9: "t"
      10: "r"
      11: "i"
      12: "n"
      13: "g"
      length: 14

```
  
### Using *sets* & *ranges*
Using __sets__ allow us to search a string for common *patterns*. Consider our "minimum number of lowercase letters" requirement.
  - Enclosing a *pattern* in `[]` __square brackets__ creates a __set__.
  - To test a given string for the existence of lowercase letter(s), we could use a *pattern* containing the set of all *lowercase* letters.
    ``` javascript
    'Pa$$w0rdString'.match(/[abcdefghijklmnopqrstuvwxyz]/g);
      (9) ["a", "w", "r", "d", "t", "r", "i", "n", "g"]
        0: "a"
        1: "w"
        2: "r"
        3: "d"
        4: "t"
        5: "r"
        6: "i"
        7: "n"
        8: "g"
        length: 9

    ```
Using a __range__ is more concise and can be used within __sets__. 
- A __range__ uses a `-` __hyphen__ to shorten a __set__. 
- To test a given string for the existence of *lowercase* letter(s), we could use the __range__ *"a-z"* in place of the __set__ of all *lowercase* letters.
  ``` javascript
  'Pa$$w0rdString'.match(/[a-z]/g);
    (9) ["a", "w", "r", "d", "t", "r", "i", "n", "g"]
      0: "a"
      1: "w"
      2: "r"
      3: "d"
      4: "t"
      5: "r"
      6: "i"
      7: "n"
      8: "g"
      length: 9

    ```
---
### Examples of common *sets* that use a *range*:
``` javascript
  .match(/[a-z]/g); // represents pattern to match the set of lowercase letters having a range a-z
  .match(/[A-Z]/g); // represents pattern to match the set of UPPERCASE letters having a range A-Z
  .match(/[0-9]/g); // represents pattern to match the set of digits having a range 0-9

```
---
__Ranges__ can be combined with other ranges
- Consider this *pattern* to match *lowercase* letters, *UPPERCASE* letters and *digits*, combining __ranges__ "a-z", "A-Z" and "0-9".
  ``` javascript
    'Pa$$w0rdString'.match(/[a-zA-Z0-9]/g);
      (12) ["P", "a", "w", "0", "r", "d", "S", "t", "r", "i", "n", "g"]
        0: "P"
        1: "a"
        2: "w"
        3: "0"
        4: "r"
        5: "d"
        6: "S"
        7: "t"
        8: "r"
        9: "i"
        10: "n"
        11: "g"
        length: 12

  ```
__Ranges__ can be combined with other *patterns*
- Consider this *pattern* to match all *digits*, and the `$` __dollar sign__.
  ``` javascript
    'Pa$$w0rdString'.match(/[0-9$]/g);
      (3) ["$", "$", "0"]
        0: "$"
        1: "$"
        2: "0"
        length: 3

  ```
---
## Finally - Our Tests (Q & A style)
Let's store the password in a variable `pw`. This variable is used in the examples below unless the full string is useful in clarifying a concept.
``` javascript
  // store string
  var pw = 'Pa$$w0rdString';

```
### Q: How can we test for a minimum number of characters?

- The `match()` function returns an array which has a length property.  Using the `.` __dot__ option will return all the characters in a given string, so using the `length` property of the resulting array will tell you how many characters exist in a string. 
- __*This may work for you, but keep in mind the*__ `String prototype` __*also has a*__ `length` __*property*__, which may be the most effective way to test for a minimum number of required characters!

### A: Test for minimum *number of characters*
``` javascript
  // store password length requirement
  var minPWLength = 8;

```

``` javascript
  pw.match(/./g).length > minPWLength; // returns true

  // OR
  
  pw.length > minPWLength; // returns true

```

---
### Q: How can we test for occurence(s) of lowercase letters?
As noted above, creating a __set__ by enclosing a *pattern* in `[]` __ square brackets__ in conjunction with the *global* flag will match anything included in the __set__.
``` javascript
  // Use a pattern containing all possible lowercase letters
  pw.match(/[abcdefghijklmnopqrstuvwxyz]/g);

```
Using a __range__ *"a-z"* in place of the entire __set__ is the most effective way to test for the occurence(s) of *lowercase* letters.
``` javascript
  // Use a pattern containing the range of all possible lowercase letters
  pw.match(/[a-z]/g);

```

### A: Test for at least one *lowercase* letter
``` javascript
  // Use a-z range as a set
  pw.match(/[a-z]/g)) !== null; // returns true

```

---
### Q: How can we test for occurence(s) of UPPERCASE letters?
As noted above, creating a __set__ by enclosing a *pattern* in `[]` __ square brackets__ in conjunction with the *global* flag will match anything included in the __set__.
``` javascript
  // Use a pattern containing all possible UPPERCASE letters
  pw.match(/[ABCDEFGHIJKLMNOPQRSTUVWXYZ]/g);

```
Using a __range__ *"A-Z"* in place of the entire __set__ is the most effective way to test for the occurence(s) of *UPPERCASE* letters.
``` javascript
  // Use a pattern containing the range of all possible UPPERCASE letters
  pw.match(/[A-Z]/g);

```

### A: Test for at least one *UPPERCASE* letter
``` javascript
  pw.match(/[A-Z]/g)) !== null; // returns true

```

---
### Q: How can we test for occurence(s) of digits?
As noted above, creating a __set__ by enclosing a *pattern* in `[]` __ square brackets__ in conjunction with the *global* flag will match anything included in the __set__.
``` javascript
  // Use a pattern containing all possible digits
  pw.match(/[0123456789]/g);
  
```
Using a __range__ *"0-9"* in place of the entire __set__ is the most effective way to test for the occurence(s) of *digits*.
``` javascript
  pw.match(/[0-9]/g);
  
```

### A: Test for at least one *digit*
``` javascript
  pw.match(/[0-9]/g); !== null; // returns true

```

---
### Q: How can we test for occurence(s) of special characters?
For our purposes, a *__special character__* is anything that is not a *letter* or a *digit*. Consider a *pattern* to match any *letter* (regardless of case) and any *digit*.
``` javascript
'Pa$$w0rdString'.match(/[a-zA-Z0-9]/g);
  (12) ["P", "a", "w", "0", "r", "d", "S", "t", "r", "i", "n", "g"]
    0: "P"
    1: "a"
    2: "w"
    3: "0"
    4: "r"
    5: "d"
    6: "S"
    7: "t"
    8: "r"
    9: "i"
    10: "n"
    11: "g"
    length: 12

```
  
Using the `^` __caret__. 
- A `^` __caret__ represents a *__NOT__* operator for a given *pattern*. To create a __set__ that will match for *special characters* only, use the range combination `[a-zA-Z0-9]` and precede the *pattern* with a `^` __caret__. 
  
``` javascript
  'Pa$$w0rdString'.match(/[^a-zA-Z0-9]/g);
    (2) ["$", "$"]
      0: "$"
      1: "$"
      length: 2

```
  
### A: Test for at least one *special character*
  ``` javascript
  pw.match(/[^a-zA-Z0-9]/g) !== null; // returns true
  
  ```

---
### Q: How can we test for the existence of "words" (series of consecutive letters)?
For our purposes, a *__word__* is *__any series of consecutive letters__*.

- Using `{}` __curly braces__ as *quantifiers*
  - Regular Expressions allow us to count __*words*__ by grouping consecutive occurences of letters within a given string.
  - Consider a *pattern* to find matches for letters (regardless of case) in a string.  By default, the results will contain an array in which each element of the array contains each letter contained in the string 
    ``` javascript
    pw.match(/[a-zA-Z]/g);
    (11) ["P", "a", "w", "r", "d", "S", "t", "r", "i", "n", "g"]
      0: "P"
      1: "a"
      2: "w"
      3: "r"
      4: "d"
      5: "S"
      6: "t"
      7: "r"
      8: "i"
      9: "n"
      10: "g"
      length: 11

    ```
    
  - By appending `{}` __curly braces__ to our *pattern*, we can essentially "group" consecutive letters together, forming __*words*__ given a minimum and maximum value to determine the least consecutive and most consecutive, respectively.
    ``` javascript
      'Pa$$w0rdString'.match(/[a-zA-Z]{1,10}/g);
        (3) ["Pa", "w", "rdString"]
          0: "Pa"
          1: "w"
          2: "rdString"
          length: 3
    ```
  - The *__curly braces__* include a minimum and (optional) maximum value (i.e., `{1,10}` in the code sample above) to define the number of consecutive letters that would satisfy a requirement that defines a *__word__*. Using the *quantifier* `{1,10}` would include any series of consecutive letters having between `1` and `10` occurences that match a *pattern*.
  - You may omit the maximum value (i.e., `{n,}` in order to include any series of consecutive letters having at least `n` occurence(s) that match a *pattern*. This would provide for a more dynamic solution.
    
  - Using the `+` __plus sign__ as a shortcut.
    - The use of *quanitifers* in an unbounded way (i.e., having no maximum limit to a given series) is very common.
    - Using the `+` __plus sign__ in place of the `{}` __curly braces__ is a shortcut.
    - So this expression:
    
      ``` javascript
        'Pa$$w0rdString'.match(/[a-zA-Z]+/g);
        
      ```
      - is equivalent to this expression:
    
      ``` javascript
        'Pa$$w0rdString'.match(/[a-zA-Z]{1,}/g);
        
      ```
      - and will return the same results
     
      ``` javascript
        (3) ["Pa", "w", "rdString"]
          0: "Pa"
          1: "w"
          2: "rdString"
          length: 3
      
      ```

### A: Test for at least one *word*
``` javascript
  pw.match(/[a-zA-Z]+/g).length > 0; // returns true

 ```
    
    
