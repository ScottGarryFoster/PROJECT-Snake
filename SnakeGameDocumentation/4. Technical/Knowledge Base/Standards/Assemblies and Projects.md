#technical #standards #csharp #project #assembly #library

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Copied from FQ|21st May 2023|

**Table of contents**
- [[#Summary|Summary]]
    - [[#Assemblies are prefixed FQ|Assemblies are prefixed FQ]]
    - [[#Place projects within Scripts folder (Unity)|Place projects within Scripts folder (Unity)]]
    - [[#Interfaces, Enums are placed in an API folder|Interfaces, Enums are placed in an API folder]]
    - [[#API Folders should use their own assembly|API Folders should use their own assembly]]
    - [[#Place test projects next to the implementation|Place test projects next to the implementation]]
    - [[#Separate assembly definitions where code is portable|Separate assembly definitions where code is portable]]
    - [[#Projects with Assemblies have their own namespace|Projects with Assemblies have their own namespace]]
    - [[#Separate files for every new definition|Separate files for every new definition]]
- [[#Further work|Further work]]

# Summary
* Where to place assemblies and projects
* How to name the assemblies and projects

---
## Assemblies are prefixed FQ
### Summary
Name your assemblies with FQ as a prefix.

### Example
* MyClass
    * API
        * ISomeInterface.cs
        * ESomeEnum.cs
        * **FQ.MyClass.API.asmdef**
    * SomethingImplimented.cs
    * **FQ.MyClass.asmdef**
* **MyClassTests**
    * SomethingImplimentedTests.cs
    * **FQ.MyClassTests.asmdef**

### Reasoning
This ensures when any new assemblies from external sources are added that there are no name conflicts.

> 🚫 This is a standard just for Unity Engine. If using other engines this may be ignored.

---
## Place projects within Scripts folder (Unity)
### Standard
Projects should be placed within the Assets/Scripts folder when in Unity.

### Example
* Assets
    * **Scripts**
        * MyClass
            * API
        * MyClassTests

### Reasoning
The assets folder requires some form of organisation, the scripts folder separates technical scripts from other assets such as art assets.

> 🚫 This is a standard just for Unity Engine. If using other engines this may be ignored.

---
## Interfaces, Enums are placed in an API folder
### Standard
Interface and Enums must be placed into a separate folder called API within the implementation folder.

### Example
* FQ.MyClass
    * **API**
        * ISomeInterface.cs
        * ESomeEnum.cs
    * SomethingImplimented.cs

### Reasoning
Understanding a class's API is important to understand what the class does - separating it ensures that returning engineers understand the class's promise to the outside world.

---
## API Folders should use their own assembly
### Standard
API folders should use their own assembly separate from the default implementation.

### Example
**Folder / file layout**
* MyClass
    * API
        * ISomeInterface.cs
        * ESomeEnum.cs
        * **FQ.MyClass.API.asmdef**
    * SomethingImplimented.cs
    * FQ.MyClass.asmdef
* **MyClassTests**
    * SomethingImplimentedTests.cs
    * FQ.MyClassTests.asmdef

### Reasoning
API may be referenced in a project without implementation. In situation where the implementation is not created within a project and they are simply passing around interface, then they do not need to reference everything. Not referencing the implementation means that **only changes to the API will cause dependencies and therefore rebuilds**. API changes are less common and therefore projects which rely just on API should be building less often.

> 🚫 This is a standard just for Unity Engine. If using other engines this may be ignored.

---
## Place test projects next to the implementation
### Standard
Tests are placed next to the given implementations they are testing and **Test** is suffixed to the name of the class and assembly.

### Example
**Folder / file layout**
* MyClass
    * API
        * ISomeInterface.cs
        * ESomeEnum.cs
    * SomethingImplimented.cs
* **MyClassTests**
    * SomethingImplimentedTests.cs

### Reasoning
Tests should be easy to find, to do this they are close to the implementation, named correctly and therefore easy to find without overlap.

---
## Separate assembly definitions where code is portable
### Standard
If a folder contains code and that definition is separate from other code, use an assembly definition. Not every folder necessarily requires a definition, for instance helper folders or sub folders which help some main implementation function. Where functionality is separate make it so with assemblies.

### Example
* Assets
    * **Scripts**
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

### Reasoning
Unity is not the only platform for a lot of the code within this project. Separating the functionality with assemblies means that moving away to another engine or reusing the code for instance in tools is easier.

The more separated the code is in this manner the less likely a small change will cause a chain of building dependencies.

> 🚫 This is a standard just for Unity Engine. If using other engines this may be ignored.

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

> 🚫 This is a standard just for Unity Engine. If using other engines this may be ignored.

---
## Separate files for every new definition
### Standard
Every time a new definition is created it should be within it's own file. The following are examples of definitions, classes, enums, structs, interfaces, abstract classes and xml/layout files. The name of the file should equal the name of the definition it represents (MyClass.cs contains class MyClass).

### Example
* Assets (*do not place code here*)
    * Scripts (*do not place code here*)
        * MyClass
            * API
                * IMyClass.cs
                * MyEnum.cs
            * Helper - FQ.MyClass
            * SomethingElseButCouldBeSeparate
                * API
                    * ISomeInterface
                * SomeImpliementation
            * MyClassStuff.cs
            * SomeOtherClassStuff.cs
        * MyClassTests
            * MyClassTests.cs

### Exceptions
There are two exceptions to this rule.
1. Private definitions defined with a scope inside a class.
2. Classes used purely for testing purposes (test driven development *not prototyping*)

If defining a private enum say within the class of another it is acceptable to not have a separate file.
``` c#
public void MyClass()
{
    private enum SomeEnumToHelp
    {
        Definitions,
    }
    
    private class SomeClass
    {
    
    }
}
```
There is an argument for these types of implementations (hiding the implementation halting the need for tests outside of the parent unit). When these are used this is acceptable.

If a class is untestable without implementation whether as a shortfall of the testing framework (or speed) or due to it being a test for external code, it is acceptable to leave this in the test file for which it is used.
``` c#
public void MyClassTests
{
    [Test]
    public void AddOne_ReturnsOne_WhenOneIsGivenTest()
    {
        // Some tests
    }

    private void TestClass() : SomethingUnmockable
    {
    }
}

public void TestClassTwo : SomethingElseUnmockable
{

}
```
In these cases it is best to add a summary comment to avoid confusion as to these classes being purely for testing. As they are within the testing namespace (and project) the impact is minimized.

### Reasoning
The implementation of types should not be hidden within files. By creating separate files it ensures that upon attempting to find code it is easily navigable.

Grouping types of code together also means that reuse and polymorphism is harder along with the ability to extract code which technically has a different purpose. SOLID principles should be kept in mind and single responsibility also applies to the file not just the code within.

---
# Further work
* Project location for external tools. This will be added if there are external tools.
* Project location for editor tools in Unity.
