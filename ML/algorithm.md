## Regular Expressions

### Match a Literal String with Different Possibilities

This is powerful to search single strings, but it's limited to only one pattern. You can search for multiple patterns using the alternation or OR operator: |.

### Extract Matches
To use the .match() method, apply the method on a string and pass in the regex inside the parentheses. 

Note that the .match syntax is the "opposite" of the .test method you have been using thus far:
```js
'string'.match(/regex/);
/regex/.test('string');
```

To search or extract a pattern more than once, you can use the g flag.

### Match Anything with Wildcard Period
The wildcard character . will match any one character. The wildcard is also called dot and period. You can use the wildcard character just like any other character in the regex.

### Match Single Character with Multiple Possibilities

You learned how to match literal patterns (/literal/) and wildcard character (/./). Those are the extremes of regular expressions, where one finds exact matches and the other matches everything. There are options that are a balance between the two extremes.

You can search for a literal pattern with some flexibility with character classes. Character classes allow you to define a group of characters you wish to match by placing them inside square ([ and ]) brackets.

### Match Numbers and Letters of the Alphabet

Using the hyphen (-) to match a range of characters is not limited to letters. It also works to match a range of numbers.

### Match Characters that Occur Zero or More Times

The last challenge used the plus + sign to look for characters that occur one or more times. There's also an option that matches characters that occur zero or more times.

The character to do this is the asterisk or star: *.

### Find Characters with Lazy Matching

In regular expressions, a greedy match finds the longest possible part of a string that fits the regex pattern and returns it as a match. The alternative is called a lazy match, which finds the smallest possible part of the string that satisfies the regex pattern.

You can apply the regex `/t[a-z]*i/` to the string "titanic". This regex is basically a pattern that starts with t, ends with i, and has some letters in between.

Regular expressions are by default greedy, so the match would return ["titani"]. It finds the largest sub-string possible to fit the pattern.

However, you can use the ? character to change it to lazy matching. "titanic" matched against the adjusted regex of `t[a-z]*?i/` returns ["ti"].

**Note**<br>
Parsing HTML with regular expressions should be avoided, but pattern matching an HTML string with regular expressions is completely fine.

### Match Beginning String Patterns
In an earlier challenge, you used the caret character (^) inside a character set to create a negated character set in the form [^thingsThatWillNotBeMatched]. **Outside of a character set, the caret is used to search for patterns at the beginning of strings.**
