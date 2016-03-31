Patterns are not easy to understand, but they reward the effort of study. Over the last 3 years, I’ve been struck by one of the underlying principles that leads to
better designs: remove duplication. It’s also been highlighted by mantras in a couple of recent books: 

  1. the DRY (Don’t Repeat Yourself) principle in the Pragmatic Programmer (A. Hunt, and D. Thomas, Addison Wesley,1999) and 
  2. “Once and Only Once” from Extreme Programming Explained: Embrace Change (K. Beck, Addison Wesley, 1999).
  
The principle is simple: say anything in your program only once. Yet identifying and removing repetition from your code can lead to many interesting consequences. 
If you are determined to remove all repetition, it can lead you a long way toward a good design and can help you
apply and understand the patterns that are common in good designs.

Here is the discussion with key points:

### Name
Avoid Repetition.

### Summary
This improvement applies to the situation where you have spotted and identified duplicate/repeated code in your code base. This technique helps remove duplicate code using **The Rule of Three**


**The Rule of Three**

The first time you do something, you just do it. The second time you do something similar, you wince at the duplication, but you do the duplicate thing
anyway. The third time you do something similar, you refactor.

Tip: Three strikes and you refactor.

### Associated Pattern

[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

### Motivation

**Why**

We can apply this refactoring technique for following reasons:
 1. Remove multiple representations of information.
 2. Remove unintended duplication from our designs.
 3. Remove "copy and paste" code.
 4. Foster code reuse.

**Why NOT**

We should skip refactoring duplicate code in following circumstances:
 1. Removing duplicate code adds more complexity and reduces the readability of existing code.
 2. Removing duplicate code adds [ofuscation](https://en.wikipedia.org/wiki/Obfuscation).
 
### Mechanics

 1. Identify duplicate code blocks in your code base.
 2. Identify what is constant or common and what varies.
 3. Isolate common stuff from variations
 4. Remove the redundancy in the common stuff.
 
### Example
 
 
