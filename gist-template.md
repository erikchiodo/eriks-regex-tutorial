# Eriks-Regex-Tutorial

Welcome to my Regex Tutorial!  Below, I explain the fundamentals of regular expressions by summarizing what a regular expression is by defining search patterns, literal, and meta characters. Following a high-level conceptual summary, I provide a breakdown of a specific search pattern (email) by desecribing each of the components that make up a common regular expression.

## Table of Contents

- [Introduction to Regular Expressions](#introduction-to-regular-expressions)
- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Introduction to Regular Expressions

Before going through an example of a regular expression, it's important to understand what a regular expression is.

A regular expression (regex) is a string of characters that allows you to find specific snippets of information from a body of text. To simplify the definition, when creating a regular expression we are defining a **search pattern**. Think of a search pattern as the Find and Replace functionality on your computer. When you type in a string of characters you'd like to search you are implementing a form of a regular expression.

For example, if you wanted to search for the string "Columbia" in the below text, you would navigate to find & replace, type in the string, and see all instances of that string highlighted.

![Screen Shot 2023-05-28 at 11 27 06 AM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/48d06306-5893-4c42-8117-eef059f05952)

The above example higlights the most basic form of a regular expression -- one that only searches by **literal characters**. In regex, literal characters are the ordinary characters that match themselves exactly as they appear. They are the non-special characters that represent themselves without any special meaning or interpretation. So if we wanted to write a regular expression for searching "Columbia" in the above example the following would be the sequential steps:

```
    1. Search for "C" (case sensitive)
    2. Next character is "o"
    3. Next character is "l"
    4. Next character is "u"

    ... Follow pattern for each character in word you want to search

    8. Last character (after "i") is "a"
```

You might be asking at this point: What's the use of search patterns -- especially considering that they only seem to search for highly-specific combinations of characters?

Luckily, there's another concept in regex called **meta characters**. Meta characters allow you to create regular expressions that have pre-defined meaning with specific functions within search patterns. They allow you to create more impactful expressions because they give you the flexibility to add more depth to what you're searching for within a body of text.

For example, if we wanted to search for a word that starts with "C" and is followed by 7 lowercase letters (i.e. Columbia), how might we write a regular expression using meta characters?

Let's look at the following regular expression: 

```
C[a-z]{7}
```

- `C`: This is the letter "C", which is a literal character.
- `[a-z]`: This is a bracket expression. It allows the values to be any lowercase letter between "a" and "z".
- `{7}`: This is a quantifier that defines the character limit on the

Taking all these components together, we get a more complex regular expression that defines a search pattern for a string that starts with "C" and is followed by 7 lowercase letters.

Now that we have a fundamental understanding of regular expressions, literal, and meta characters, let's review a more complex example and better understand the types of metacharacters that are available to us for creating regular expressions:

```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

## Regex Components

### Anchors
![Screen Shot 2023-05-29 at 10 27 14 AM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/c3b41a42-30e4-43cc-a1de-91e8acbdfcba)


Anchor tags specify where within a string to apply the search pattern. 

- '`^`' (carat): the search pattern must match at the beginning of the string.
- '`$`' (dollar sign): the search pattern must match at the end of the string.

To provide an example of each in practice using literal characters, let's say we have the following string and search patterns:

 > Example: **"Columbia makes coding fun"**

```
1. /^Columbia/
2. /Columbia$/
```

For the above example, which regex would find Columbia (#1 or #2)?

The correct answer is #1 because this search pattern looks for "Columbia" at the beginning of the string. If we wanted to create a string that would work with #2, we could change it to "Who makes coding fun? Columbia" because Columbia is at the end of the string.

However, in the regex example for email validation we have the following nomenclature: `/^...$/` where the search pattern is in between the tags. What does this mean?

When you have beginning `(^)` and ending `($)` anchor tags this enforces a strict search pattern, which means that the string has to exactly match the search pattern.  So if we had '/^Columbia$/', this would fail for the above example because those anchor tags strictly look for the string "Columbia".

So for our email example, that regular expression is strictly enforced to ensure there are no additional characters before or after where that search pattern applies.

### Quantifiers
![Screen Shot 2023-05-29 at 11 38 29 AM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/4d533f3f-0c55-4c9f-91f9-4cec95f7e463)


As the name suggests, quantifiers are important for quantifying (determining the limits of) a regular expression.

Below are some examples of common quantifiers you'll see:

```
* - matches zero or more times of the preceding element. 
+ - matches one or more times of the preceding element.
? - matches zero or one time of the preceding element.
{n} - matches exactly n times
{n,} - matches at least n times
{n,m} - matches between n and m times

```

By nature, quantifiers are **greedy** meaning it will match for as many instances of the pattern there are, so you can add the `?` quantifier to make it lazy (will match as few occurrences of the pattern are possible).

In our email example, we have `{2,6}` as our quantifier, which means that it will match the pattern for string length 2 to 6 (inclusive).

Why might we have this quantifier on this section of the expression?

If you look at many email addresses, many end with .com, .edu, .io, etc. So, in this regex we are defining that the ending of the address will likely be a number between 2 and 6. That means for other regex expressions where you may want to use a quantifier, determine where you may have a pattern that may fall within specific string length.

### Grouping Constructs
![Screen Shot 2023-05-29 at 12 01 47 PM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/2410a87c-9141-4c0b-82e0-23b9a5b4c2d9)


Grouping Constructs allow us to apply groupings of search patterns to strings. As we look to build more complex regular expressions, these will come more in handy to help add more specificity to our search patterns. 

If you look at the image above and with an understanding of how email addresses are structured does the grouping constructs make more sense.

If not let me show you how sample emails would match up against the image.

![Screen Shot 2023-05-29 at 11 59 37 AM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/11bc5bf8-7781-49ff-a97e-b138a72bc6ff)

As you can see this regular expression is grouped based on standard a standard email pattern where each grouping has it's own search pattern applied. Once we discuss bracket expression and character classes it will become clearer what these specific grouping constructs entail.


### Bracket Expressions
![Screen Shot 2023-05-29 at 2 16 59 PM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/b89981c5-1815-4207-b2f1-d5a6df6483c7)

Bracket Expressions define the search content (attributes of the content you want to search for). The most common attributes you will likely search for are: numbers, letters, special characters:

```
[0-9] - string can include a number from 0 to 9.
[a-z] - string can include any letter between a & z (lowercase)
[A-Z] - string can include any letter between A & Z (uppercase)
[_&$-] - string can include _, &, $, -. There are other special characters that can be included. 

```
Note that "-" can be included as a special character, but is also used to set ranges between letters and numbers.

With a basic understanding of bracket expressions, we can explain each of the bracket expression in our email example:


- `[a-z0-9_.-]`
    - Letters: a-z (lowercase only) 
    - Numbers: 0-9
    - Special Characters: Underscore, Period, or Hypen

- `[\da-z.-]`
    - Letters: a-z (lowercase only)
    - Special Characters: Period or Hypen

- `[a-z.]` 
    - Letters: a-z (lowercase only)
    - Special Characters: Period

### Character Classes

Character classes correspond to a single character from a group of characters. We've already reviewed a form of character classes -- Bracket Notation, which created a pattern for matching lowercase, numbers, and special characters. 

Below are some other Character Classes we've haven't discussed yet:

- `[^]`
    - Matches any character not include in the bracket. We already learned that bracket expressions are known as positive character groups, which means that they are inclusive of characters defined within the brackets. However, by using the above notation, you can create expressions that are not inclusive of elements included within the brackets.
- `/d`
    - Very similar to [0-9], this notation will allow you to search for any character that is a digit (which includes decimal values) whereas [0-9] will match a single digit character (whole number).

Aside from bracket notation, we also have an instance of a character class within one of our bracket expressions, which I've highlighted below: 

![Screen Shot 2023-05-29 at 2 56 25 PM](https://github.com/erikchiodo/eriks-regex-tutorial/assets/122952630/2e229024-b099-48d7-bc60-95143cfe1dbb)

### The OR Operator
Similar to conditional logic within programming OR Operators give us a way to assess various patterns within the same expression. To use conditional OR operator use `|` symbol.


**Example:**
```
[A-Z]|[0-9]

    Valid matches:
        - ABC
        -1A3
        - HELLO42
        - 1
```


Essentially any Uppercase Letter (A-Z), Number (btwn 0-9), or any combination would be acceptable. 

### Flags
Flags allow you to change the behavior of a pattern. Think of them as additional rules you can add to your regular expression.

The most common types:

- `i`
    - This stands for case-insensitive. It allows you to pass through a combination of upper and lowercase letters for a string and match regardless of letter case.

**Example:**
```
   /hello/i

    Valid matches:
        - hello
        - HEllo
        - HellO
```
- `g`
    - This standards for global. It will search for all instances of the string (not just the first instance).

**Example:**
```
    /ho/g:
      ho ho ho Merry Christmas

```

## Author

Erik Chiodo is a current student enrolled in Columbia's Full-Stack Development course. If you're interested in this projects or other projects Erik has been working on, visit his github page here [Github Profile](https://github.com/erikchiodo)