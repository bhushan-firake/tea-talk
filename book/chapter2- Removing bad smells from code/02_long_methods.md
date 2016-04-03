
###Name

[Extract Method](http://refactoring.com/catalog/extractMethod.html), which takes a clump of code and turns it into its own method.

###Summary

This improvement applies to the situation where you have long methods in the code & you are not able to understand the purpose of the method by its name or the body. After applying this improvement, you will be able to understand this method more clearly.

###Associated Design Pattern
[SRP](https://en.wikipedia.org/wiki/Single_responsibility_principle)

###Motivation

**Why**

1. Almost all the time, the problems come from methods that are too long.
2. Long methods are troublesome because they often contain lots of information, which gets buried by the complex logic that usually gets dragged in.
3. It increases the chances that other methods can use a method when the method is finely grained.
4. It allows the higher-level methods to read more like a series of comments.
5. Overriding also is easier when the methods are finely grained.
6. Once the method is broken down, you can understand how it works much better. you may also find that
the algorithm can be improved to make it clearer. You can then use **Substitute Algorithm** to introduce
the clearer algorithm.

**Why NOT**

1. If extracting a method compromises with the quality and clarity of existing code.
2. Sometimes you came across a method body which is as clear as the name of the method in which you want to extract it, then you should not extract it. The procedure is called **Inline Method**, the reverse of Extract method.

###Mechanics

1. Identify the code that you want to extract.
2. Create a new method & name if after the intention of the method(Name it by what is does and NOT by hot it does it).
3. Copy the relevant code from source method to target method.
4. Scan the extracted code for any reference of the local variables in source method. they will be the parameters of the new method.
5. See whether any temporary variables from source method are used in the target method. If so, declare them in the target method as temporary variables.
6. Replace the code in the source method with a function call to new target method.
7. If you have moved any temporary variables, which are declared outside the extracted code, to target method. If so, you can now remove the declaration.

###Example


