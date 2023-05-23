
#standards #csharp #naming #class #name

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Copied from FQ|21st May 2023|

**Table of contents**
- [[#Summary|Summary]]
    - [[#Namespace begin with FQ|Namespace begin with FQ]]
    - [[#Projects with Assemblies have their own namespace|Projects with Assemblies have their own namespace]]
    - [[#API Projects do not contain API in the namespace|API Projects do not contain API in the namespace]]
    - [[#Projects are named UpperCamelCase|Projects are named UpperCamelCase]]
    - [[#Classes are named using UpperCamelCase|Classes are named using UpperCamelCase]]
    - [[#Methods are named using UpperCamelCase|Methods are named using UpperCamelCase]]
    - [[#Public variables are named using UpperCamelCase|Public variables are named using UpperCamelCase]]
    - [[#Protected variables are named using UpperCamelCase|Protected variables are named using UpperCamelCase]]
    - [[#Private variables are named using lowerCamelCase|Private variables are named using lowerCamelCase]]
    - [[#Internal variables are named using lowerCamelCase|Internal variables are named using lowerCamelCase]]
    - [[#Const variables are named using UpperCamelCase|Const variables are named using UpperCamelCase]]
    - [[#Private variables within methods are lowerCamelCase|Private variables within methods are lowerCamelCase]]
    - [[#Always add scope to classes, methods and variables|Always add scope to classes, methods and variables]]
    - [[#Use this. when referring to private member variables|Use this. when referring to private member variables]]
    - [[#Interfaces begin with the letter I|Interfaces begin with the letter I]]
    - [[#Enums and Structs are named UpperCamelCase|Enums and Structs are named UpperCamelCase]]
- [[#Exceptions|Exceptions]]
- [[#Further work|Further work]]

# Summary
Naming rules for all code elements.

---
## Namespace begin with FQ
### Summary
Name spaces must start with **FQ** for **Fated Quest**.

### Example
``` c#
namespace FQ.Something.SomethingElse
{

}
```

### Reasoning
If external libraries are added to the standards then we need to ensure our code is separate. A different prefix means the likely hood is essentially zero.

---
## Projects with Assemblies have their own namespace
### Standard
Projects which contain assemblies should have their own namespace. The namespace begins FQ (as stated in this document) then the location the assembly may be found.

Subfolders do not need their own namespaces as they do not contain their own assembly.

### Example
* Assets
    * Scripts
        * MyClass
            * API
                * FQ.MyClass.API.asmdef
            * Helper
            * SomethingToGatherThingsForMyClass
            * SomethingElseButCouldBeSeparate
                * FQ.MyClass.SomethingElseButCouldBeSeparate.asmdef
            * FQ.MyClass.asmdef
        * MyClassTests
            * FQ.MyClassTests.asmdef

For the above folder structure the following are the namespaces:
* Assets (*do not place code here*)
    * Scripts (*do not place code here*)
        * MyClass - FQ.MyClass
            * API - FQ.MyClass
            * Helper - FQ.MyClass
            * SomethingElseButCouldBeSeparate - FQ.MyClass.SomethingElseButCouldBeSeparate
        * MyClassTests - FQ.MyClassTests

### Reasoning
Namespaces are clutter but they are also required to separate code. If a project is committed to referencing the project, the namespace should remain the same in each until the time that a new **implementation** assembly is added.

---
## API Projects do not contain API in the namespace
### Standard
API projects use a namespace without API - often the same as default implementation.

### Example
**Namespace within API**
``` c#
namespace FQ.Something.SomethingElse
{
    public interface ISomething{}
    public enum ESomethingElse{}
}
```
**Namespace within implementation**
``` c#
namespace FQ.Something.SomethingElse
{
    public class SomethingImplemented : ISomething {}
    public abstract class SomethingAbstract : ISomething {}
}
```

### Reasoning
The using includes should not be filled with API to include the API.

---
## Projects are named UpperCamelCase
### Standard
When naming projects they should be named using UpperCamelCase.

### Example
Projects not following this:
* playerProject
* `_`playerProject
* PLAYERPROJECT

Projects following this:
* PlayerProject
* MapUserInterface

### Reasoning
Projects create namespaces and references, these should have a contained format.

---
## Classes are named using UpperCamelCase
### Standard
All classes must use UpperCamelCase to define their name.

### Example
``` c#
// Does not follow the standard
public class _Classname {}
public class className {}
public class CClassName {}
public class CLASSNAME {}

// Follows the standard
public class ClassName {}
public class PlayerMovement {}
```

### Reasoning
Classes are types which are used by other projects, it should be obvious from the name that you are creating an instance of the type.

---
## Methods are named using UpperCamelCase
### Standard
When naming methods, use UpperCamelCase.

### Example
``` c#
// Does not follow the standard
public void _methodName (){}
public void methodName (){}
public void MMethodName (){}

// Follows the standard
public void MethodName (){}
public void MovePlayer (){}
```

### Reasoning
Methods are important with a given class (as they are the literal code) therefore UpperCamelCase means that when calling methods from other classes it is obvious what type the first letter will be.

---
## Public variables are named using UpperCamelCase
### Standard
Public variables within a class should be named using UpperCamelCase (including properties).

### Example
``` c#
// Does not follow the standard
public int _jumpHeight;
public int jumpHeight;
public int m_jumpHeight;

// Follows the standard
public int JumpHeight;
public int PlayerHealth;
```

### Reasoning
Public variables are important as they are referenced by other projects therefore UpperCamelCase means that when calling methods from other classes it is obvious what type the first letter will be.

---
## Protected variables are named using UpperCamelCase
### Standard
Protected variables within a class should be named using UpperCamelCase (including properties).

### Example
``` c#
// Does not follow the standard
protected int _jumpHeight;
protected int jumpHeight;
protected int m_jumpHeight;

// Follows the standard
protected int JumpHeight;
protected int PlayerHealth;
```

### Reasoning
Protected variables may as well be public because unlike private variables an implementation may expose anything protected.

---
## Private variables are named using lowerCamelCase
### Standard
Private variables within a class should be named using lowerCamelCase (including properties).

### Example
``` c#
// Does not follow the standard
private int _jumpHeight;
private int JumpHeight;
private int m_jumpHeight;

// Follows the standard
private int jumpHeight;
private int playerHealth;
```

### Reasoning
To understand the scope of a variable, switching case removes the confusion - especially in diffing software.

---
## Internal variables are named using lowerCamelCase
### Standard
Internal variables within a class should be named using lowerCamelCase (including properties).

### Example
``` c#
// Does not follow the standard
internal int _jumpHeight;
internal int JumpHeight;
internal int m_jumpHeight;

// Follows the standard
internal int jumpHeight;
internal int playerHealth;
```

### Reasoning
Internal variables are closer to private variables than public. Therefore this is singnalling to the user that the variable is hidden somewhat.

---
## Const variables are named using UpperCamelCase
### Standard
Constant variables within a class should be named using lowerCamelCase (including properties). Readonly should be considered the same as the scope used.

### Example
``` c#
// Does not follow the standard
public const int _jumpHeight = 1;
public const int jumpHeight = 1;
public const int m_jumpHeight = 1;

// Follows the standard
public const int JumpHeight = 1;
private const int PlayerHealth = 1;
```

### Reasoning
Constant variables are important and important names in this system begin with an upper case.

---
## Private variables within methods are lowerCamelCase
### Standard
Private variables within a method should be named using lowerCamelCase.

### Example
``` c#
// Does not follow the standard
private void MethodName()
{
    int JumpHeight = 5;
    int IJumpHeight = 4;
    int _jumpHeight = 3;
}

// Follows the standard
private void MethodName()
{
    int jumpHeight = 5;
}
```

### Reasoning
To understand the scope of a variable, switching case removes the confusion - especially in diffing software. Not adding a this keyword means there is a clear distinction between this and member variables.

---
## Always add scope to classes, methods and variables
### Standard
For interfaces, classes (of all types), methods and variables (including properties) should define a scope. This applies to basically everything other than the private variables within methods.

### Example
``` c#
// Examples Not Following
class ClassName
{
    int privateVariable;

    void privateMethod()
    {
        private int internalInt;
    }
}

// Examples Following
public void ClassName
{
    private privateVariables;
    public PublicVariables;

    public void PublicMethod()
    {
        int somethingUsed;
    }

    private void PrivateMethod()
    {
        int somethingElseUsed;
    }
}
```

### Reasoning
The scope of code should be easily obvious, adding the scope means that nothing is assumed.

---
## Use this. when referring to private member variables
### Standard
When referring to private member variables which are defined in the class and not within the method use the *this* prefix.

### Example
Example not using this
``` c#
public class MyClass()
{
    public string Name;
    private int number;

    public void MyMethod()
    {
        number = 1;
    }

    public void MyOtherMethod()
    {
        this.Name = "Jerry";
    }
}
```
Example using this
``` c#
public class MyClass()
{
    public string Name;
    private int number;

    public void MyMethod()
    {
        this.number = 1;
    }

    public void MyOtherMethod()
    {
        Name = "Jerry";
    }
}
```

### Reasoning
It should be clear without a user interface where a variable is coming from. The this keyword enables even diffing software to understand what is going on.

---
## Interfaces begin with the letter I
### Standard
When adding an interface, place it within the API folder with a capital I at the start, then UpperCamelCase. 

### Example
Names not following this:
* Somefunctionality
* IsomeFunctionality
* isomeInterface

Names following this:
* ISomeInterface
* ISomeFunctionality

### Reasoning
The majority of the interfaces within the project are created with the explicit intention of providing the promises to other projects or classes. The implementation behind them tends to be named the same thing as there is a marrying of implementation and API. Naming things hard enough as it is without prefixing, figuring out interface names, ensuring they are separate and figuring out what to open in a folder tree is going to trend towards another naming scheme. To halt the concept of ever adding Interface when not referring to the user interface, the IPrefix is at least somewhat neat.

---
## Enums and Structs are named UpperCamelCase
### Standard
When adding an enum or struct use UpperCamelCase. Anything with code that warrants a separate file should be named with UpperCamelCase.

### Example
Names not following this:
* someNamesForEnum
* ESomeNamesForEnum
* esomeNamesForEnum

Names following this:
* SomeNamesForEnum
* MapLimits
* SomeStructName

### Reasoning
Files and the names of classes should match. Naming every file this way makes sense because it highlights the words and naming the type itself means there is never ambiguity - if you own it lower case is first, if others may use it or it is external upper case first.

---

# Exceptions
These names only apply to C# and C++ code. They do not necessarily apply to Python or scripting languages.

---
# Further work
Outstanding naming to be decided:
* Events and delegate naming structure
* How Singletons should be named (should they use particular words)
* Use of words like Interface and Display
    * Should they be used, if so when? (For instance is Interface always a user interface?)
* Unit based naming, for instance delayInSeconds.