#technical #standards #csharp 

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Copied from FQ|21st May 2023|

**Table of contents**
- [[#Summary|Summary]]
    - [[#Properties|Properties]]
    - [[#Fields|Fields]]
    - [[#Public API property declarations first|Public API property declarations first]]
    - [[#Public Fields are listed second|Public Fields are listed second]]
    - [[#Protected Fields are listed third|Protected Fields are listed third]]
    - [[#Internal Fields are listed Fourth|Internal Fields are listed Fourth]]
    - [[#Private Fields are listed fifth|Private Fields are listed fifth]]
    - [[#Constructors are listed Sixth|Constructors are listed Sixth]]
    - [[#Non-API properties are listed Seventh|Non-API properties are listed Seventh]]
    - [[#Methods are defined Eighth|Methods are defined Eighth]]
    - [[#Prefer a single Return|Prefer a single Return]]
    - [[#Give a name to the standard|Give a name to the standard]]
    - [[#Give a name to the standard|Give a name to the standard]]
- [[#Exceptions|Exceptions]]
- [[#Further work|Further work]]

# Summary
Defines how to structure a given class and how to join classes together with their API.

For the purpose of this document and all the standards these are the two denitions for properties and fields.
## Properties
Properties are variables in interface but when implementating have some hidden logic or private variable influwencing them.

From [Microsoft's documentation](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties):
> A property is a member that provides a flexible mechanism to read, write, or compute the value of a private field.

> Properties enable a class to expose a public way of getting and setting values, while hiding implementation or verification code.

Example
```c#
public class MyClass
{
    public int IAmAProperty
    {
        get => iamaproperty;
        private set => iamaproperty = value;
    }
    private int iAmAProperty;
    
    private int OtherProperty
    {
        get
        {
            return otherProperty;
        }
        private set
        {
            if(iAmAProperty > 0)
            {
                otherProperty = value;
            }
        }
    }
}
```

## Fields
Fields are directly defined within the class or struct. Generally fields are private (in C#) as public properties are preferable to allow API access. Technically static fields are also called Static Members but they should also be considered fields.

From [Microsoft's documentation](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/fields):
> Fields typically store the data that must be accessible to more than one type method and must be stored for longer than the lifetime of any single method.

> Generally, you should use fields only for variables that have private or protected accessibility. Data that your type exposes to client code should be provided through [methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/methods), [properties](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties), and [indexers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/indexers/).

Example
```c#
public class MyClass
{
    public static int MyStaticField;
    public int MyField;
    private bool myOtherField;
}
```

---
## Public API property declarations first
### Standard
Any public property declarations should be first in the implementation. Private variables which directly contribute (are used in the set/get) may be declared near to them if it makes the code clearer.

### Example
Place interface properties first.
``` c#
public interface IMyInterface
{
    bool MyBool { get; }
}

public class MyImplimentation : IMyInterface
{
    public bool MyBool { get; private set; }

    public int SomethingElse;
}
```

Can place privates which contribute to them next to the properties which use them.
``` c#
public interface IMyInterface
{
    bool MyBool { get; }
}

public class MyImplimentation : IMyInterface
{
    public bool MyBool
    { 
        get
        {
            return myBool;
        }
        private set
        {
            if(myBool)
            {
                value = myBool;
            }
        }
    }
    private bool myBool;
}
```

### Reasoning
When moving from API to implementations the first answer one needs is, how is this interface implemented.

---
## Public Fields are listed second
### Standard
All public fields are near the top of the document and come after the interface properties. Constant feilds should be listed at the top of the public fields.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}

    public const int Constant = 2;
    public int Field;
}
```

### Reasoning
Fields do not contain logic but do affect the class from outside. In general these should be avoided as understanding where errors occur is more difficult if the mutable variable may have a hidden outside sets (hidden as in complex not hidden as in references).

Although public fields are to be avoided, if they are used they should be at the top so we are aware of these.

---
## Protected Fields are listed third
### Standard
When defining protected fields do so third in the document. Constant feilds should be listed at the top of the public protected.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}
    public bool PublicField;

    protected const int ProtectedConst;
    protected int SomeThingProtected;
}
```

### Reasoning
Protected variables may as well be public variables for how much they expose the implementation details and are able to be altered by outside parties. To ensure the mutable variables are well known these are displayed after the publics.

---
## Internal Fields are listed Fourth
### Standard
Define internal fields fourth in a class.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}
    public bool PublicField;
    protected int SomeThingProtected;

    internal const int InternalConst;
    internal string internalField;
}
```

### Reasoning
Internal expose implementation and unfortunately they are required for some elements of dependancy injection. If they are used then they should treated as more dangerous privates and therefore they are listed just above private.

---
## Private Fields are listed fifth
### Standard
When defining private variables do so fifth in the document.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}
    public bool PublicField;
    protected int SomeThingProtected;
    internal string internalField;

    private string somePrivateString;
}
```

### Reasoning
Mutable variables are the most common source of bugs and private fields are still mutable whilst being some what hidden. Therefore it makes sense to list them after protected to ensure finding bugs is easier.

---
## Constructors are listed Sixth
### Standard
Constructors are sixth on the list, in the order public, protected, internal, private.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}
    public bool PublicField;
    protected int SomeThingProtected;
    internal string internalField;
    private string somePrivateString;

    public MyClass()
    {
    }
    
    protected MyClass(int protected)
    {
    }
    
    internal MyClass(bool internal)
    {
    }

    private MyClass(string private)
    {
    }
}
```

### Reasoning
Constructors are important and more important than standard methods as the setup can be the route of a bug in the rest of the class.

---
## Non-API properties are listed Seventh
### Standard
API (public) properties are listed near the top but any non-api properties should come seventh in the order public, protected, internal, private.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}
    public bool PublicField;
    protected int SomeThingProtected;
    internal string internalField;
    private string somePrivateString;

    public MyClass(){}
    protected MyClass(int protected){}
    internal MyClass(bool internal){}
    private MyClass(string private){}

    public int NotInApi {get; private set;}
    protected int SomethingElseProtected {get; private set;}
    internal string internalProperty {get; private set;}
    private string privateString {get; private set;}
}
```

### Reasoning
Properties are list methods but not methods so they should have their place and this is above methods due to how much variables may hide functionality.

---
## Methods are defined Eighth
### Standard
Defining class methods should occur eighth in the list and in this order: Public API, public, protected, internal, private.

### Example
``` c#
public class MyClass : IMyInterface
{
    public int ApiProperty {get; private set;}
    public bool PublicField;
    protected int SomeThingProtected;
    internal string internalField;
    private string somePrivateString;

    public MyClass(){}
    protected MyClass(int protected){}
    internal MyClass(bool internal){}
    private MyClass(string private){}

    public int NotInApi {get; private set;}
    protected int SomethingElseProtected {get; private set;}
    internal string internalProperty {get; private set;}
    private string privateString {get; private set;}

    public void InTheAPI(){}
    public void BrandNew(){}
    protected void ProtectedMethod(){}
    internal void InternalMethod(){}
    private void PrivateMethod(){}
}
```

### Reasoning
Implementation needs to be placed somewhere and this is in the body at the bottom.

---
## Prefer a single Return
### Standard
Except in cases which performance is of the utmost importance prefer a single return.

### Example
``` c#
public int MyMethod(bool doSomething)
{
    int answer = -1;

    if(doSomething)
    {
        answer = 11;
    }

    return answer;
}
```
'Early outs' are also prefered.
``` c#
private NumberGetter numberGetter;

public int MyMethod(bool doSomething)
{
    int answer = -1;
    if(numberGetter == null)
    {
        return answer;
    }

    if(doSomething)
    {
        answer = this.numberGetter.GetNumber();
    }

    return answer;
}
```

### Reasoning
Having a single return makes the code more readable. There are cases in which for performance you may want to return several times but methods should be small enough to generally not require such.

---
## Give a name to the standard
### Standard
*Explain the standard here*

### Example
*You may add positive and negative examples*
``` c#
public class name
{
    int number {get;}
}
```

### Reasoning
Explain the reasoning.

---
## Give a name to the standard
### Standard
*Explain the standard here*

### Example
*You may add positive and negative examples*
``` c#
public class name()
{
    int number {get;}
}
```

### Reasoning
Explain the reasoning.

---
# Exceptions
*There do not need to be exceptions, but if there are we should point them out.*

---
# Further work
*Anything further which may affect this.*
