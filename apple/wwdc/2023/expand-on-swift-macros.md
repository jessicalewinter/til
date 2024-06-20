# Expand on Swift macros

## Why macros?
Derived conformance and result builders, which help users to avoid writing repetitive code.
Ex: When you conform your structure to Codable without providing a custom implementation for its members, the compiler adds the conformance into a set of members that it inserts into the program.
Swift has a lot of feature that provides a boilerplate code implementation, such as:
- Property Wrappers
- Result builders
- Derived CaseIterable, Codable and Equatable
- Autoclosures
- @main attribute

Basically, the user writes a simple syntax and the compiler expands into a more complex piece of code automatically.

### Macros
Before, if the boilerplate code does not attend your needs, you could propose a new feature to the Swift compiler. However, that would take long time, since the open source would need to review individually each proposal. That's why the Swift team introduce macros.

Macros let you to extend Swift features, which you can distribute on a SPM package, without changing the compiler.

Swift macros was design with 4 goals in mind. 

## Design Philosophy
### Distinctive use sites
The first goal is that it should be pretty obvious when you're using a macro. There are two kinds of macros: 
-- **freestanding macros** stands in place of something else in your code. They always starts with a pound(#) sign. 
```swift
return #unwrap(icon, message: "should be in the app bundle")
```
-- **attached macros** are used as attributes on declarations in your code. They always starts with an at(@) sign.
```swift
@AddCompletionHandler func sendRequest() async throws -> Response
```

Swift already uses  `#` and `@` to indicate that you'll have some speciall behaviour, as an example, compiler flag starts with `#` and property wrapper uses `@`. If you don't see any of these symbols, it's guarantee that no macros are involved in that particular existence code.

#### Complete, type-checked, validated
Arguments should be complete expressions. You can't pass a wrong argument type to a macro, because they are type-checked, it will make the compiler also to throw an error.
Basically, a macros's implementation can validate the its input and emit compiler warning or error is something is wrong.

#### Inserted in predictable ways
A macro can only add to the visible code in your program. It can't remove or change existing code. Even though you're not sure what a macro does, you can be sure that will not delete an item from an array or change a method implementation. 

### Macros are note magic
Macros just add code to your program, that is not code magic involved.

You can right-click on a macros's use and ask to see what it expands into. You can add breakpoints in the expansion or step into it with the debugger. If your macro expansion code doesn't compile, you'll be able to see the error message right into the expansion. All this feature is provided by another SPM package called 
You can also add unit tests to make sure that your code is working as expectedd

## Translation model
When the compiler see a macros usage on the code, it extracts that from the code, and sends it to a compiler plugin that contains the implementation of that macro. The plugin run as a separate process in a secure sandbox, it processes the macro use and returns an `expansion`, a new fragment of code created by the macro. The compiler add the expansion to your code and compiles it altogether, which when your code is run, it looks like that you've creaated that macro.

How the compiler know that macro exists? A macro declaration provides the API for a macro. You can write the declaration right into your own module, or you can import it from a 3r party SDK.
it specifies the macro's name and signature, the number of parameters it takes, their labels and types, and the type of the result if the macro has one, just like a function declaration. It also must have one or more attributes that specify the macro role.

## Macro roles
A role ia set of rules for a macro. It governs where and how you apply the macro, what kind of code it expands into, and where that expansion is inserted into your code.

A macro role determines:
- Where it can be used
- What types of code it expands into
- Where the expansions are inserted


There are two types of roles for freestanging macros: Expression and declaration. 
- `@freestanding(expression)`:  Creates a piece of code that returns a value. An expression is a unit of code that executes and produces a result. This macros expands to an expression.
- `@freestanding(declaration)`: Creates one or more declarations. This code expands and creates functions, variables or types.


And there are five roles that create attached macros: Peer, accessor, member attribute, member, and conformance.
Attached macros are, as the name suggests, attached to a specific declaration. That means they have more information to work from. They can also access the declaration they'ree attached to. They cann pull out names, types, and so on.
- `@attached(peer)`: Adds new declarations alongisde the declaration it's applied to. A peer role can be attached to any declaration, not only variables, functions or type, but even things like import and operator declarations. If you create on a function
- `@attached(accessor): Add accessors to a property. These macros will be attached to variables and subscripts, and they can install accessors into them, like ""get", "set", "willSet" or "didSet"
- `@attached(memberAttribute)`: Adds attributes to the declarations in the type/extension it's applied to. The macro gets attached to a type or extension, and it can add attributes to the member of whatever it's attached to.
- `@attached(member)`: Adds new declarations inside the type/extension it's applied to. Like the memberAttribute role, you can add it to types and extensions, but instead od applying to existing members, this role will add new members. Soo you can add properties, initializer, methods and so on.
- `@attached(conformance)`: Adds conformances to the type/extension it's applied to. This macro will already conform to a protocol so we don't need to explicit add it to the struct that we'll adding to.



### Role composition
- A macro may have multiple attached roles
- Swift will expand all roles applicable where the macro was used
- At least one role must be applicable

For example, you create a macro that has the accessor and memberAttribute roles,and you attach it to a propeerty, swift will expand the acessor role. If you attach to a type, swift will expand the memberAttribute role. But if you try to attach to a function, you will get a compiler error.

### Macros can't see each others' expansions
When two different macros roles get applied to the same code, which ones is expandended first? Each macro will see the original version of the declaration without expansions provided by the others.


### Macro implementation
We create the interface using the macro keywork and we need to declare the implementation. Most of the times, we'll create an external macro, which is implemente by a compiler plugin. External macro is what defines this relantionship. It specifies the plugin, or module, that the compiler should launch, and the name of the type inside this plugin. So when a specific is expandend, it will launch a plugin called "MyMacros". 

The macro declaration goes in your normal library alongside your other APIs, bu the macro implementation goes into a separate compiler plugin module. And the "#externalMacro" creates the link between the declaration and the type implementing it.

#### SwiftSyntax
We start importing a library called "SwiftSyntax". That helps you parse, inspect, manipulate and generate Swift source code. This library represents source code as a special tree structure. 
For example, the following code:
```swift
struct Person {
  var name: String
  var height: String
  var birthDate: Date?

}

```
This struct is represented as a `StructDeclSyntax`, which has the following properties:
- `attributesList`: has a list of attributes, 

Some of the syntax nodes in these properties are called tokens. They represent a specific piece of text in the source file. If you take a look at this struct treem you'll see that structKeyword and identifies are TokenSyntax are tokens. However, the attributes and memberBlock are not, the attributes and memberBlock are not, they have children.


#### SwiftSyntaxMacros and SwiftSyntaxBuilder
The SwiftSyntaxMacros provides protocols and types necessaries to work with macros
The SwiftSyntaxBuilder is a library which provides convenience APIs for constructing syntax trees.


#### expansion method
All expansion methods are static, and that's what Swift calls to expand the macro role when this macro is used. They are all static because Swift does not create an instance of this specific macro type, it just uses it as a container for the methods.
Each of the expansion methods returns SwiftSyntax nodes that are inserted into the source code.

It may looks like that we are returning a plai string, but Swift treats it as a fragment of the expanded source code. The DeclSyntax initializer accepts string liteal, And then we ask the Swift parser to turn into a DeclSyntax node. These is one of the conveniences that the SwiftSyntaxBuilder library provides.

We'll take a look at the expansion methods, attribute conforms to AttributeSyntax, which we use to look at the arguments that we pass into our macro. The second argument is called declaration, whic has a type that conforms to DeclGroupSyntax, which is the type/extension the attribute was applied to. The final parameter is the context, and is of type that conforms to MacroExpansionContext, it's when the macro implementation wants to communicate with the compiler. It can do a few different things, like throwing errors and warnings.

You can throw simple errors, but it does not enrich a lot your code. So, we'll create a Diagnostic, which is the instance of an error. A diagnostic contains at least two pieces of information, location of the error, the attribute. The second is a message.

#### Building syntax trees
- SwiftSyntax initializers and methods
- SwiftUI-style syntax builders
- Literals with interpolations

Syntax nodes are immutable, but we have plenty of APIs that either create new nodes or return modified versions of exisiting nodes.


## Writing correct macros
### Name collisions
It generates code which captures the expression's result
- Swift macros don't prevent name conflicts
- Sometimes you want to access names from outside your macro
- Sometimes you even want to introduce new names
-- But you need to declate the names you're adding


Where the macros inside a macro are distinct from the names of outside, so they can't conflict with each other. Swift is not like that because sometimes we need to use names from outside on your macro.

### Name specifiers
- overloaded
- prefixed
- suffixed
- named
- arbitrary

use the other names to make the compiler.

### Don't use outside information
- Only use the information the compiler provides
-- Otherwise, tools won't know when they need to re-expand a macro
- Sandbox can't stop you, but you still shouldn't:
-- Insert API results like currentTime, process ID, or random numbers
- Save information in global variables between expansions.
The compiler assumes that are pure functions.

Now, the macro system is designed to prevent some kinds of behavior that could violate this rule. Compiler plug-ins run in a sandbox that stops macro implementations from reading files on disk or accessing the network.

If you do these things, your macro can be inconsisten. So DON'T

### Testing your macros


## Wrap-up
- Macros let you creta language features that "expand" into complicated code
- Declared in a libary, but implmented in a Swift program run in a sandbox
- Roles express what a macro can do
- Write macro tests with XCTest

## Resources
- [Expand on Swift macros](https://developer.apple.com/videos/play/wwdc2023/10167/)
