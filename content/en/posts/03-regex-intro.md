+++
title = 'Regular Expressions: A brief introduction'
date = 2025-02-08T06:39:17-03:00
draft = false
+++

![regex-feeling-of-power](/regex-feelings-of-power.png)

In a incredibly data-rich world we lived in, knowing how to deal with huge strings, either to extract relevant information or replacing some piece of data or even to simply detect the presence of some pattern in a fast way is absolutely crucial. That’s why regular expressions are a very important topic and such a powerful tool to have under your belt as a developer, once they can bring a boost of productivity in your daily routine.

## Applicabilities

Its significance lies in the simple fact that regular expressions serve a wide range of purposes. In practice, to list a few of them, imagine that you’re stuck in some of the following situations:

<ol>
    <li>Web scraping - There’s a  full HTML page and you’d like to extract all the URL’s from this text;</li>
    <li>Form Validation - You’re handling with a huge form with hundreds of fields and you need to build customized validations to each one of them: an email must ends with @domain.com and cannot have specific special characters. A phone number must contain only numbers with a determined quantity of digits and so on;</li>
    <li>Log Analysis - In a log application analysis, where you’d like to extract only the relevant information about a class or a specific file from an endless stack trace.</li>
</ol>

In all of this cases, regular expressions can help and save you hours of work.

## But, What the heck is that, anyway ?

Although regular expressions may seem like something that only the most hardcore programmers would touch, they are nothing but a sequence of symbols that specifies a text pattern. This pattern can be intended as a rule that will be, in turn, tested against a certain input text, returning a positive result if the rule matches some substring of input text. A negative result will be returned, otherwise.

## Studying the werewolves vocabulary

Just to clarify this concept, imagine that we’re trying to understand the werewolves language.

![howling-wolf](/howling-wolf.png) 

Their vocabulary is very limited. They only use the following word pattern to make their sentences: starts with letter ‘a’, followed by letter ‘w’, followed by 2 or more instances of letter ‘o’. Finally, there’s an exclamation mark to enforce respect around the gang. Thus, the following are some word examples that a werewolf can express.

<ul>
    <li>awoo!</li>
    <li>awooooo!</li>
    <li>awoooooooo!</li>
</ul>

So, what we gotta do now is figuring out a pattern that covers all these different werewolves words. That’s the point where a regular expression comes in. Fortunately(and conveniently, in this case), all words of werewolves vocabulary can be expressed by a simple regex. Take a look on its structure.

![howling-wolf](/regex-werewolves.png)

First of all, an important thing to remember is that all regular expressions live inside delimiters. These are the boundaries of your regex. The most common character used as delimiter is the slash(“/”), but it can be any other. It depends on syntax standard used(PCRE, POSIX and so on). 

Now, analyzing this regex, we have:

<ul>
    <li>
        awo: This is the start of the pattern. So, till this part, It means that for there to be chances of a match occurring, the input text must contains a substring starting with awo, necessarily.
    </li>
    <li>
        o+: The ‘o’ character matches literally the ‘o ’ character. The ‘+’ is called a quantifier and it means “one or more”. It’ll match one or more occurrences of the previous character. In this case, ‘o’. 
    </li>
    <li>
        !: It’s the end of pattern. Matches exactly the ‘!’ character.
    </li>
</ul>

Therefore, our regular expression will give us a positive answer to all of the following input texts: awooo!, awooo!, awooooo! and so on.

A warning point in this regex is that depending on what’s gonna be our universe set of input strings, that’s not gonna work as expected. This regex works fine to detect if a specified word belongs to werewolve’s language in our input, but if our goal is knowing how many words of werewolves vocabulary are present in a huge given text, for example, it is gonna fail, once our regex matches only the first occurrence of this pattern. This problem can be easily solved by using a device called “global flag” that will detect all occurrences of pattern, but I don’t wanna go in these details now, so let’s assume that our input text is only a single word that can be or not part of the werewolves language.

## Other examples

The previously topic was a very simple example, just to introduce you to the wonderful world of regex. But, believe me, as far as expressions are getting more powerful, they’re also getting more complex. Just to give you a quickly view of how powerful regular expressions can be, take a look of these another examples.

<!-- <table>
  <tr>
    <th>Regex</th>
    <th>Function</th>
  </tr>
  <tr>
    <td>/\/[^\r\n]*[\r\n]/g</td>
    <td></td>
  </tr>
  <tr>
    <td>/\d{5}-\d{4}|\d{5}/g</td>
    <td>Search for zip-codes in a text</td>
  </tr>
  <tr>
    <td>/^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6})*$/gm</td>
    <td>An email validation</td>
  </tr>
  <tr>
    <td>/(?=(.*[0-9]))(?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,./?])(?=.*[a-z])(?=(.*[A-Z]))(?=(.*)).{8,}/</td>
    <td>Check if a given password is strength, i.e., if it has, at minimum, 1 lowercase letter, 1 uppercase letter, 1 number, 1 special character and be at least 8 characters long.</td>
  </tr>
</table> -->

<ul>
    <li>
        <b>/\/[^\r\n]*[\r\n]/g</b>  → Detect if a code contains comments of “slash-slash” type;
    </li>
    <li>
       <b>/\d{5}-\d{4}|\d{5}/g</b>  → Search for zip-codes in a text;
    </li>
    <li>
        <b>/^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6})*$/gm</b>  → An email validation
    </li>
    <li>
        <b>/(?=(.*[0-9]))(?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,./?])(?=.*[a-z])(?=(.*[A-Z]))(?=(.*)).{8,}/</b>  →  Check if a given password is strength, i.e., if it has, at minimum, 1 lowercase letter, 1 uppercase letter, 1 number, 1 special character and be at least 8 characters long.
    </li>
</ul>

Don’t be scared if it isn’t one of the most understandable things that you already saw. Our goal is to comprehend, slowly and step by step, the basic building blocks of this mysterious puzzle.

## Regex engines

Finally, our regexes only will make sense if there’s some entity able to interpret them, test against an input text and delivering us a result from this processing. This entity is called a regex engine.

A regex engine receives two inputs: a regular expression and an input text.
In turn, the result return may vary. Commonly, It can be only a boolean value indicating the presence/absence of the pattern described by regex or it can be a list of input text’s substrings that match the pattern. All depends on your own needs.

![regex-engine](/regex-engine.png)

## Conclusion

So, in this brief introduction we present you to the regular expressions world and their applicabilities. In the upcoming articles we will dive deeper in knowing the main characters, their meanings and how to apply them.

