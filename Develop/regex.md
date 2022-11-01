# Regular Expression Tutorial

This tutorial will go over regular expressions, or regex for short, and how it is used to define a search pattern. 

## Summary

In this regex tutorial, I will be going through the regex which I will call "Matching a URL" :

<code>/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/</code>

This series of characters is known as a regex search pattern that is meant for basic URL validation. I will break down the preceding "Matching a URL" regex in order for us to explore its components.



## Table of Contents

- [Regular Expression Tutorial](#regular-expression-tutorial)
  - [Summary](#summary)
  - [Table of Contents](#table-of-contents)
  - [Regex Components](#regex-components)
    - [Anchors](#anchors)
    - [Bracket Expressions](#bracket-expressions)
    - [Quantifiers](#quantifiers)
    - [Greedy and Lazy Match](#greedy-and-lazy-match)
    - [Grouping and Capturing](#grouping-and-capturing)
    - [OR Operator](#or-operator)
    - [Character Classes](#character-classes)
    - [Flags](#flags)
    - [Boundaries](#boundaries)
    - [Back-references](#back-references)
    - [Look-ahead and Look-behind](#look-ahead-and-look-behind)
  - [Author](#author)

## Regex Components

A regex is considered a litteral and must be wraped in slash characters (<code>/</code>). You will see this in the "Matching a URL" regex : 

<code>/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/</code>

Now we can go over the components of a regex.

### Anchors

The characters <code>^</code> and <code>$</code> are both considered to be **anchors**. 

I see this as the open and closing characters for a regex with the slash character to wrap the regex. The <code>^</code> anchor identifies a string that starts with the characters that follow it, while the <code>$</code> anchor identifies a string that ends with the characters that precede it.

### Bracket Expressions

If you take a look at the regex pattern: 

<code>/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/</code>

you will notice the use of square brackets (<code>[]</code>). Anything inside the square brackets represents a range of characters we want to match. These patterns are known as **bracket expressions**, and are also considered as a **positive character group** because they outline the charcters we want to include. For example, <code>[\da-z\.-]</code> will look for a string that includes the pattern with in it. Lets break it down. 

In the square brackets, it begins with the character class <code>\d</code> which matches any digit equivalent to [0-9]. 

Next we have, <code>a-z</code> which means the string can contain any lowercase letter between a-z. Keep in mind, regex is case sensitive so this would only look for lowercase only. To include uppercase characters we would need to change the pattern to <code>[a-zA-Z]</code>.

We then have <code>\.</code> the "." is inside the character class of "\" so the dot loses its special meaning and matches a literal dot.

We also have the hyphen character <code>-</code> which means the string can contain a hyphen. 

If we put all these characters together, <code>[\da-z\.-]</code>, this will match any string that contains any digit between 0 and 9, any combination of lowercase letters between a and z, matches a literal dot, and contains a hyphen. These characters can be in any order and do not need to meet all these requirements but it can meet any of them. 

The following examples fulfill these requirements:

  - "1my-regex."
  - "regex"
  - "re-gex0"
  - "12345"

### Quantifiers

Quantifiers set the limits of the string that your regex matches, for a specific section of the string. they usually include a minimum and maximum number of patterns possible. The quantifiers include the following:

  - <code>*</code>--Matches the pattern zero or more times
  - <code>+</code>--Matches the pattern one or more times
  - <code>?</code>--Matches the pattern zero or one time
  - <code>{}</code>--Curly brackets allow for three different ways to set limits:
      - <code>{ x }</code>--Matches the pattern exactly x number of times
      - <code>{ x, }</code>--Matches the pattern at least x number of times
      - <code>{ x, y }</code>--Matches the pattern at a minimum of x times and a maximum of y times.


Now lets take a look at the "Matching a URL" regex again:

<code>/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/</code>

We have our quantifier <code>{2,6}</code>. This means that we want to find a string pattern a minimum of 2 times and a maximum of 6 times. Because the bracket expression before it, <code>[a-z\.]</code>, will match any string that includes any combination of lowercase letters between a and z, this quantifier means that this string has to be between 2 and 6 characters.

### Greedy and Lazy Match

Each of those quantifiers can be made **lazy** by adding the <code>?</code> symbol after it, making it match as few occurances as possible. By default quantifiers like <code>*</code> and <code>+</code> are **greedy**, meaning that they try to match as much of the string as possible. The <code>?</code> symbol after a quantifier makes a quantifier **non-greedy** which means it will stop as soon as it find a match.

In the regex, you see the use of greedy macthes multiple times with <code>?</code> in order to match the pattern zero or one times, <code>*</code> in order to match the pattern zero or more times, and <code>+</code> in order to match the pattern one or more times. In our specific regex, we are not using lazy matches after our quantifiers. 

### Grouping and Capturing

Each regex is made up of different sections that fulfill different requirements. In order to break these sections up, you will need to use **group constructs**. The main way to group a section of a regex is by using a parentheses (<code>()</code>). This would be known as a **subexpression**. In between two subexpressions, they would be seperated by a colon (<code>:</code>). An example of this would be <code>(abc):(xyz)</code>. Subexpressions look for an exact match so the string "abc:xyz" would match but the string "cba:zyx" would not.

In the begining of our regex we have the subexpression <code>(https?:\/\/)</code>. There are two catagories in grouping constructs: **capturing** and **non-capturing**. Capturing groups capture the matches character sequences for re-use while non-capturing groups do not. Non-capturing groups are mad by adding the characters <code>?:</code> at the front of an expression inside the parantheses. The "Matching a URL" regex uses a non-capturing group in front of the forward slashes. 

### OR Operator

Remember that subexpression group constructs look for an exact match to meet the requirements of the pattern. Using the **OR operator** (<code>|</code>), the expression <code>(abc)</code> could be written as <code>(a|b|c)</code>. Using the previous example from grouping: <code>(abc):(xyz)</code>, we can take it and convert it to <code>(a|b|c):(x|y|z)</code>. Now both "abc:xyz" and "cba:zyx" will match. However, our "Matching a URL" regex is not using an OR operator. 

### Character Classes

A **character class** in a regex defines a set of characters. We have discussed a character class earlier in this tutuorial. There are also two types of character classes defined as positive and negative character classes.

Here are examples of some common character classes:

  - <code>.</code>--Matches any character excpet the newline character (<code>\n</code>)
  - <code>\d</code>--Matches any Arabic numeral digits
  - <code>\w</code>--Matches any alphanumeric characters including the underscore and uppercase and lowercase letters
  - <code>\s</code>--Matches a single whitespace character, including tabs and line breaks
  - <code> \ </code>--Indicates that the following character should be treated specially, or "escaped". To match this character    literally, escape it with itself.
  - <code>[a-g]</code>-- Matches any one of the enclosed characters

Take a look at the "Matching a URL" regex: 

<code>/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/</code>

You see the <code> \ </code> character class used here <code>(https?:\/\/)</code> to match the character literally to have the forward slashes after "https". The character class <code>\d</code> is used here <code>[\da-z\.-]</code> to match any numeric digits. The character class <code>\w</code> is used here <code>[\/\w \.-]</code> to match any alphanumeric characters in the url.

Now, the examples above are considered to be positive character classes. A negative character class does the opposite of what you have defined using the <code>^</code> character and using uppercase character classes.

Examples of negative character classes:

  - <code>\D</code>--Matches any character that is not a digit
  - <code>\w</code>--Matches any character that is not a word character from the basic Latin alphabet
  - <code>[^a-g]</code>--Matches anything that is not enclosed in the brackets

### Flags

As stated in the begining of the tutorial, a regex is wrapped in slash characters. The exception to this is the use of **flags**. Flags are placed at the end of a regex to define additional functionality or limits.

Here are the three most common flags:

  - <code>g</code>--Global search: the regex should be tested against all possible matches in a string
  - <code>i</code>--Case-insensitive search: case should be ignored while attempting a match in a string
  - <code>m</code>--Multi-line search: a multi-line input string should be treated as multiple lines

### Boundaries

Assertions include **boundaries**, which indicate the beginnings and endings of lines and words, and other patterns indication in some way that a match is possible. We use the <code>$</code> boundary in our regex to match the end of the input.

Examples of boundaries:

  - <code>^</code>--Matches the beginning of input
  - <code>$</code>--Matches the end of input
  - <code>\b</code>--Matches a word boundary
    - Example: 
      - /\br/ matches the "r" in "regex"

### Back-references

**Backreferences** refer to a previously captured group in the same regular expression.

Examples of backreferences:

  - <code>\n</code>--Where "n" is a positive integer. A back reference to the last substring matching the n parenthetical in the regular expression
  - <code>\k<Name></code>--A back reference to the last substring matching the Named capture group specified by "<Name>".

### Look-ahead and Look-behind

Collectively called **lookaround**, **look-ahead** and **look-behind** matches a pattern that is followed or preceeded by another pattern. 

Look-ahead syntax <code>x(?=y)</code>: matches "x" only if "x" is followed by "y".

Look-behind syntax <code>(?<=y)x</code>: matches "x" only if "x" is preceded by "y"

## Author

Gassan Bundakji

GitHub: https://github.com/gbundakji

GitHub Gist: https://gist.github.com/gbundakji/8cef9be60cef741a06934fa3a42ac073
