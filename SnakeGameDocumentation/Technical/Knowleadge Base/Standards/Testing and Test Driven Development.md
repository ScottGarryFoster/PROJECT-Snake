
#testing #standards #csharp #tdd #testdrivendevelopment

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Copied from FQ|21st May 2023|

**Table of contents**
- [[#Summary|Summary]]
    - [[#Testing Frameworks and software sources/versions|Testing Frameworks and software sources/versions]]
    - [[#Name tests Act_Assert_ArrangeTest|Name tests Act_Assert_ArrangeTest]]
    - [[#Test Exceptions using NUnit|Test Exceptions using NUnit]]
    - [[#Tests are as important as production|Tests are as important as production]]
- [[#Exceptions|Exceptions]]
- [[#Further Work|Further Work]]

# Summary
Defines names and practices for test driven development.

---
## Testing Frameworks and software sources/versions
### Standard
Only use testing frameworks and mocking frameworks below.

### Versions to use
Only use these versions:
| Library | Version |  
|--|--|  
| Unity Testing | 1 .1 .33 |  
| NUnit | 3 .13 .3 |  
| Moq | 4 .18 .4 |

### Reasoning
Sticking to particular versions means upgrading should be easier (do not need to track more software) and the build servers already have this software.

---
## Name tests Act_Assert_ArrangeTest
### Standard
When creating tests, name them with the test subject (or what may be in act), the expectation (or what is asserted) then how the assertion is met (or what is arranged). Suffix tests with 'Test' and use UpperCamelCase.

### Example
``` c#
[Test]
public void AddNumbers_Returns6_When2And4AreGivenTest()
{
    // Arrange
    var calculator = new Calculator();
    int firstNumber = 2;
    int secondNumber = 4;
    int expected = 6;

    // Act
    int actual = calculator.AddNumbers(firstNumber, secondNumber);

    // Assert
    Assert.IsEqual(actual, expected);
}
```

### Reasoning
Placing act at the start means that grouping like acting tests is easier as the acts are the same. Asserts help to identify the difference between two tests by what they assert. Arrange is broad and although helps to understand a test it is the least important (reading the code is generally better).

---
## Test Exceptions using NUnit
### Standard
When testing exceptions do so in-line within the method and not using the test definition.

### Example
``` c#
[Test]  
public void Construction_ArgumentNullException_GivenStringIsNullTests()  
{  
    // Arrange  
    string givenXML = null;  
  
    // Act/Assert  
    Assert.Throws<ArgumentNullException>  
    (  
        () => new ClassName(givenXML)  
    );  
}
```

### Reasoning
Adding an exception to definition is slightly cleaner but less clear. This method ensures that it is clear what is expected and what within the method should be throwing the exception. Adding it to definition means that if mocks are not used other sections of code may be throwing the exception.

---
## Tests are as important as production
When writing tests keep in mind the same principles you would for production code. Create multiple test files if needed (named appropriately).

Techniques you should employ:
1. Avoid duplication
    * Use `[SetUp]` and `[TearDown]` along with private variables to avoid repetitive code
    * Use private methods to make tests easier to understand, especially when it duplicates
2. Stick as much as possible to Arrange, Act, Assert
    * Have these three sections to ensure Assertions are last and clear.
3. In large test classes, split them up or create regions
    * If a class worth of tests is on a single method or property consider splitting it up.
    * Regions and single files are preferred over multiple files but if SetUp and TearDowns need to change, either create methods for the two differences or create two files

---
# Exceptions
*There do not need to be exceptions, but if there are we should point them out.*

---
# Further work
* Outline multithreaded C# tests
* Create a practice for mocks
