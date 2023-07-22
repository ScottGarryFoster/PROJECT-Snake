#randd #research #resources #summaries #talk #logging #nick #chapsas #ndc #oslo 

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Development|29 June 2023|

**Table of contents**
- [Summary](#Summary)
- [Take-Aways](#Take-Aways)
	- [CHANGE ME???](#CHANGE%20ME???)

# Summary
>Add Summary

>Source: [You are doing logging in .NET wrong. Let’s fix it. - Nick Chapsas - NDC Oslo 2023](https://www.youtube.com/watch?v=NlBjVJPkT6M) by [Nick Chapsas](https://www.linkedin.com/in/nick-chapsas/?originalSubdomain=uk)
>Date: NUMBER(ST/ND/RD/TH) MONTH YEAR
>Publication: SITE/PUBLISHER

# Take-Aways
## C# Compiler
* When C# builds there is a step between the CLI and the Complier which interprets the code. This changes from version to version (how and what is translated) and Core is different to the framework versions. 
* This is to protect the compiler from build failures and from mistranslation
* One of the translations is string interpolation:
```c#
string something = $"I am {name} and I am {age} years old";

string becomes = string.Format("I am {0} and I am {1} years old", name, age);
```
* Note In .net version 9 this is not the case.
* The reason this is an issue, is that Format and Concat use arrays (the second half of the above is an object array).
* When casting to an object or hard casting another type (like `return (int)myObject;`) the value will exist on the heap and then need to be cleaned up later by the garbage collector.
* Additionally consider the amount of string in code which are always allocated on the heap slowing down the application.
* String interpolation handlers will not save you in this case but they will help. This is because they will at least reduce the number of strings to a single final string on the heap.

> Key take away, heap fast, stack slow. Strings and boxing always on the heap.

![[Pasted image 20230629220420.png]]

## CHANGE ME???
>Use headings as needed