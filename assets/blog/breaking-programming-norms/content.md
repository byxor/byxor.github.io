As programmers, we've become intimately familiar with the process of manipulating text. Most of our programming languages expect textual input, the source code, which later gets translated into a series of instructions for our processors.

Although there are many exceptions, the vast majority of our languages follow similar patterns.

Patterns are beneficial to us because they reduce the cost of frequently switching between languages, something that is an inevitable task of any programmer who doesn't want to get stuck maintaining a legacy COBOL system for 40 years.

With common patterns between languages...

* We save time.
* We save mental effort.
* We save money.

> Okay, patterns are good... But what _are_ these patterns?

Often, these patterns are so commonplace that we don't notice them. This makes it hard to identify specific ones without diving in head first.

We've established that patterns are useful, but could we be missing out on something greater by breaking them?

> Surely, any language which intentionally breaks these patterns will be impractical, difficult to use, and esoteric, right?

Most likely, yes, but let's take a step back and identify these patterns with an open mind. You never know, we might find something fascinating by turning everything on its head.

## Patterns in our Languages

What better way to find similarities than to compare a code snippet written in 3 different languages.

We'll be looking at a **FizzBuzz implementation** in Java, Python and C.

### Examples

#### Java

```java
public static String fizzbuzz(int n) {
    final boolean dividesByThree = n % 3 == 0;
    final boolean dividesByFive = n % 5 == 0;

    if (dividesByThree && dividesByFive) {
        return "FizzBuzz";
    } else if (dividesByThree) {
        return "Fizz";
    } else if (dividesByFive) {
        return "Buzz";
    } else {
        return "" + n;
    }
}
```

#### Python

```python3
def fizzbuzz(n):
    divides_by_three = n % 3 == 0
    divides_by_five = n % 5 == 0
    
    if divides_by_three and divides_by_five:
        return "FizzBuzz"
    elif divides_by_three:
        return "Fizz"
    elif divides_by_five:
        return "Buzz"
    else:
        return str(n)
```

#### C

```c
#include <string.h>
#include <stdio.h>
#include <stdlib.h>

void fizzbuzz(int n, char *buffer, size_t length) {
    const int dividesByThree = n % 3 == 0;
    const int dividesByFive = n % 5 == 0;
        
    char *result;
                
    if (dividesByThree && dividesByFive) {
        result = "FizzBuzz";
    } else if (dividesByThree) {
        result = "Fizz";
    } else if (dividsByFive) {
        result = "Buzz";
    } else {
        snprintf(result, length, "%d", n);
    }

    strncpy(buffer, result, length);
}
```

### Comparison between Snippets

You might have seen similarities such as...

1. They all have variable assignment.
2. They all have `if`/`else if` constructs.
3. They all have functions.

You may notice that all these similarities are all language constructs. Let's zoom out a little bit and look at the code snippets from a higher level of abstraction.

### Similarities at a Higher Level of Abstraction

Now we can see similarities such as...

1. They're all text snippets.
2. They're all left-aligned.
3. They all indent from left-to-right, like a sideways pyramid.

Let's take some of these similarities and see what we can change.

## Breaking the Norm

### Alignment

What would it look like if the text was right-aligned?

<pre class="language-c" style="text-align: right;">
<code class="language-c">#include &lt;string.h&gt;
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;

void fizzbuzz(int n, char *buffer, size_t length) {
    const int dividesByThree = n % 3 == 0;
    const int dividesByFive = n % 5 == 0;
        
    char *result;
                
    if (dividesByThree && dividesByFive) {
        result = "FizzBuzz";
    } else if (dividesByThree) {
        result = "Fizz";
    } else if (dividsByFive) {
        result = "Buzz";
    } else {
        snprintf(result, length, "%d", n);
    }

    strncpy(buffer, result, length);
}</code>
</pre>

> Yikes, this looks terrible. All the indentation is lost!

Let's fix that.

### Indentation from Right-to-Left

<pre class="language-c" style="text-align: right !important;">
<code class="language-c">#include &lt;string.h&gt;
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;

void fizzbuzz(int n, char *buffer, size_t length) {
const int dividesByThree = n % 3 == 0;   &nbsp;
const int dividesByFive = n % 5 == 0;   &nbsp;

char *result;   &nbsp;
                
if (dividesByThree && dividesByFive) {   &nbsp;
result = "FizzBuzz";       &nbsp;
} else if (dividesByThree) {   &nbsp;
result = "Fizz";       &nbsp;
} else if (dividesByFive) {   &nbsp;
result = "Buzz";       &nbsp;
} else {   &nbsp;
snprintf(result, length, "%d", n);       &nbsp;
}   &nbsp;
strncpy(buffer, result, length);   &nbsp;
}
</code>
</pre>

#### Pros

1. ???

#### Cons

1. When going down a line, we need to read right-to-left to discern the indentation level, then read the line from left-to-right. This continuous switching is difficult.
2. The curly braces appear to be "inverted". 
3. It's difficult to display text like this in the browser without quirky hacks.
4. It's difficult to scan for the places where a variable is being mutated.

#### Other

1. The curly braces appear to "support" the text (if you pretend gravity is acting rightwards).

Can you see any other pros/cons/observations that I can't? Let me know by tweeting @brandnewbyxor.

## Summary

* Syntactic patterns are good.
* Breaking patterns hasn't shown benefits (in my limited experiment).
* Left-aligned text is good.
* Right-aligned text is bad, but fun.
* You don't have to design your programming languages to look like the rest.
* Think outside the box and you might discover something cool.

What other patterns could you break? Play around and see what you find.
