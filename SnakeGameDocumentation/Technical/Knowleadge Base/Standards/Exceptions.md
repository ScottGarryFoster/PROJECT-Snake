
#standards #csharp #exceptions #error #errorhandling

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Copied from FQ|21st May 2023|

**Table of contents**
- [[#Summary|Summary]]
    - [[#Use typeof in exceptions.|Use typeof in exceptions.]]
    - [[#Use nameof in exceptions.|Use nameof in exceptions.]]
    - [[#Throw exceptions when you would not want to continue|Throw exceptions when you would not want to continue]]
    - [[#Prefer error messages to exceptions|Prefer error messages to exceptions]]
- [[#Further work|Further work]]

# Summary
Describes how to throw exceptions and when to use them.

---
## Use typeof in exceptions.
### Standard
When creating an exception the origin should be defined within the exception, using typeof(**ClassName**).

### Example
``` c#
public class ClassName()
{
    public ClassName(string something)
    {
        if (string.IsNullOrWhiteSpace(something))  
        {  
            throw new ArgumentNullException(  
                $"{typeof(ClassName)}: {nameof(something)} may not be null or empty.");  
        }
    }
}
```

### Reasoning
When viewing logs or telemetry on crashes the exceptions are thrown with just the description and not the full stack. For this reason we should be more explicit in our exception message and include the type.

TypeOf shows us the entire path to the class which threw the exception, for instance:
`FQ.UpperClassName.ClassName: something may not be null or empty.`

---
## Use nameof in exceptions.
### Standard
When creating an exception anything which may have causes the exception should not be placed into the code verbatim with the name burned into the message. Instead use nameof to ensure that upon the name altering all the instances rename.

### Example
``` c#
public class ClassName()
{
    public ClassName(string something)
    {
        if (string.IsNullOrWhiteSpace(something))  
        {  
            throw new ArgumentNullException(  
                $"{typeof(ClassName)}: {nameof(something)} may not be null or empty.");  
        }
    }
}
```

### Reasoning
NameOf ensures that upon renaming that all the exception messages are up dated. There may be occasions where the core reason for an exception is many classes/scopes away from the thrown exception and therefore not obvious upon renaming that it to must be renamed.

---
## Throw exceptions when you would not want to continue
### Standard
In the cases in which the promise made by the class are not met an exception should be thrown. This exception in theory should never be called however upon the situation when it is called, we should be ones writing the exceptions to ensure a clear error is given.

### Example
``` c#
public class ClassName()
{
    public ClassName(string something)
    {
        if (string.IsNullOrWhiteSpace(something))  
        {  
            throw new ArgumentNullException(  
                $"{typeof(ClassName)}: {nameof(something)} may not be null or empty.");  
        }
    }
}
```

### Reasoning
When setting up a type or reaching the point of no return throwing an exception is ideal over allowing the code to fail. Fail loudly is a key principle and doing so early (for instance in a constructor) allows us to better understand an area and bug.

---
## Prefer error messages to exceptions
### Standard
Exceptions are rather nice for returning information from code which may not be able to return any indication it has failed. Exceptions should not be the norm within the code base as they are slower than simply displaying error messages. They also cause the application to crash when unhandled which means that with features that are unfinished work or testing may be lost.

### Example
What might be done if aiming for more exceptions:
``` c#
public class ClassName()
{
    public void MethodName(string something)
    {
        if (string.IsNullOrWhiteSpace(something))  
        {  
            throw new ArgumentNullException(  
                $"{typeof(ClassName)}: {nameof(something)} may not be null or empty.");  
        }
    }
}

public class OtherClass()
{
    public void CreateAndUseMethod()
    {
        var className = new ClassName();
        try
        {
            className.MethodName(" ");
        }
        catch(Exception e)
        {
            MessageBox.Show(e.Message);
        }
    }
}
```

What should be done over exceptions:
``` c#
public class ClassName()
{
    public bool MethodName(string something, out List<string> errors)
    {
        if (string.IsNullOrWhiteSpace(something))  
        {  
            errors.Add($"{nameof(something)} may not be null or empty.");
        }

        return !errors.Count;
    }
}

public class OtherClass()
{
    public void CreateAndUseMethod()
    {
        var className = new ClassName();
        if(!className.MethodName(" ", out List<string> errors))
        {
            foreach(string message in errors)
            {
                MessageBox.Show(message);
            }
        }
    }
}
```


### Reasoning
Exceptions are slower than simply creating a logic structure to alter around it. Error messages allow for multi-line messages with the intent of displaying them to the user. The code to produce an error message output is much more performant and clearer. Error messages also may be gathered into logs to debug the problem later.

---
# Further work
* Possibly an Error format
* Create a unified logger for errors
