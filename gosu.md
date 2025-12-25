Gosu is a Statically Typed, Compiled language.

However, it is designed to behave with the agility of a dynamic scripting language.



Gosu does support a "Program" format (.gsp). These are essentially standalone scripts where you can just start writing code without a class structure:

// A .gsp (Gosu Program) file
uses java.util.Date

var now = new Date()
print("The current time is: " + now)

// This looks like a script, but under the hood, 
// Gosu wraps this in a temporary class and compiles it 
// to bytecode before running it.



Bridging the Gap: Java’s Historical Limitations

In 2002, Java (around version 1.4) was the industry standard for enterprise backends, but it was incredibly verbose and lacked the "syntactic sugar" needed for rapid business logic development.

Lack of Closures/Lambdas: Java didn't get lambdas until version 8 (2014). Gosu had "Blocks" (closures) from day one, allowing us to write clean, functional code for complex insurance calculations and collection filtering.

Null safety check

Type Inference: Gosu introduced var and construct long before Java adopted similar concepts, reducing boilerplate code for developers who needed to focus on insurance rules rather than variable declarations.



Metadata-Driven Architecture

The heart of PolicyCenter is its Data Model. Insurance carriers need to add custom fields (extensions) to entities like Policy, Account, or Coverage constantly.

The Problem: Standard Java requires a "Compile -> Deploy -> Restart" cycle. It doesn't natively "know" about XML-defined metadata extensions without heavy reflection or code generation.

The Gosu Solution: Gosu was designed to be Metadata-aware. It is a statically typed language that compiles directly against the Guidewire metadata. If you add a field to a .etx (extension) file, Gosu sees it immediately with full IDE autocomplete and type safety. This seamless integration is what makes PolicyCenter so configurable.

Gosu’s Open Type System

In Gosu, everything is a type(XML, WSDL, JSON, DB, etc.)

The Traditional Way (Java / C#)

Download the WSDL file.

Run a Tool: Use wsimport or a Maven plugin to generate 50+ Java "POJO" classes.

Compile: Compile those generated classes.

Code: Use the generated classes.

Change: If the service changes, you must regenerate and recompile the st ubs, or your code breaks at runtime.

The Gosu Way (Open Type System)

Drop the WSDL file into the project folder.

Code: Immediately type var service = new soap.external.MyService().

Change: If the WSDL changes, you just save the file. The Gosu compiler instantly sees the new fields. There are no generated files to manage.



uses java.net.URL

var personUrl = new URL( "http://gosu-lang.github.io/data/person.json" )
var person: Dynamic = personUrl.JsonContent
print( person.Name )
print( person.Address.State )

https://gosu-lang.github.io/2016/03/01/new-json-support-in-gosu.html 

1. The Core Interface: ITypeLoader

The entire implementation revolves around a "Strategy" pattern for types. Instead of the compiler looking for a file, it asks a chain of Type Loaders.

When you write var p = new entity.Policy(), the Gosu compiler doesn't look for Policy.class. Instead, it broadcasts a request to the Type Loader Stack: "Does anyone know what 'entity.Policy' is?"

The Java Type Loader says: "No, I don't see that in my JARs."

The Entity Type Loader (which is registered at startup) says: "Yes! I see an XML file named Policy.eti. I will handle this."



The "Enhancement" Pattern

One of the most brilliant features of Gosu is Enhancements. In a standard Java environment, if you want to add a method to a core product class (like PolicyPeriod), you’d have to use inheritance or utility classes.

Open-Closed Principle: Gosu Enhancements allow developers to add methods to any class (even core Guidewire classes or Java's String) without modifying the original source code or using inheritance.

Architectural Benefit: This allows Guidewire to ship a "locked" core product while giving customers a clean, object-oriented way to extend business logic. It prevents the "spaghetti code" that usually arises from heavy customization.



Domain-Specific Logic: Rules and Workflow

Insurance is essentially a massive collection of "If-Then" statements. PolicyCenter uses Gosu Rules to handle everything from underwriting authority to validation logic.

Gosu was built to be readable by "Technical Business Analysts." While it is a full-featured language, its syntax is clean enough that the business logic isn't buried under layers of technical "plumbing" (like Getters/Setters or Exception handling boilerplate).

