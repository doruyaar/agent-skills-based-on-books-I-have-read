## Code Smells to Avoid (Based on Martin Fowler's Refactoring)

### Naming & Clarity
- **Mysterious Name**: Never use vague names. Functions, variables, and classes must clearly reveal their purpose. Refactor by renaming to descriptive names that explain intent. Example: `calc()` → `calculateMonthlySubscriptionRevenue()`

### Duplication & Bloat
- **Duplicated Code**: Extract identical or similar code into shared functions. Use the DRY (Don't Repeat Yourself) principle. Create reusable utilities instead of copying logic.
- **Long Function**: Break functions longer than ~20 lines into smaller, single-purpose functions. If you need comments to explain sections, extract those sections into named functions.
- **Long Parameter List**: Pass no more than 3-4 parameters. Use configuration objects or create parameter objects to group related data.
- **Large Class**: Split classes doing too much (indicated by many instance variables or methods) into smaller, focused classes with single responsibilities.

### Data Issues
- **Global Data**: Eliminate global variables. Encapsulate data in classes or pass it explicitly through function parameters. Global state causes unpredictable bugs.
- **Mutable Data**: Prefer immutable data structures. Use const/readonly/final modifiers. Return new objects instead of modifying existing ones.
- **Data Clumps**: When 3+ variables always appear together (e.g., startDate, endDate, timezone), create a dedicated class/object to hold them (e.g., DateRange).
- **Primitive Obsession**: Replace primitive types representing domain concepts with dedicated classes. Use Money class instead of float, PhoneNumber instead of string, Coordinates instead of x/y numbers.
- **Temporary Field**: Avoid instance variables used only in certain cases. Either pass them as parameters or create a separate class for that specific context.

### Control Flow Complexity
- **Repeated Switches**: Replace repeated switch/if-else on type codes with polymorphism. Create subclasses or use strategy pattern instead of type checking.
- **Loops**: Prefer functional pipelines (map, filter, reduce) over manual loops when transforming collections. They're more declarative and readable.
- **Message Chains**: Break long chains like `obj.getA().getB().getC().doSomething()`. Use the Law of Demeter: only talk to immediate friends. Create intermediate methods.

### Poor Abstraction
- **Lazy Element**: Remove classes or functions that don't justify their existence. If a class just wraps one function or delegates everything, inline it.
- **Middle Man**: If a class only delegates to another class without adding value, remove it and call the destination directly.
- **Speculative Generality**: Don't build features "we might need someday". Delete unused hooks, abstractions, and parameters. Build for today's requirements using YAGNI (You Aren't Gonna Need It).
- **Data Class**: Classes with only fields and getters/setters but no behavior should have business logic moved into them or be converted to simple data structures.

### Coupling Problems
- **Divergent Change**: If one module changes for many different reasons, split it into separate modules where each handles one type of change.
- **Shotgun Surgery**: If every change requires edits across many classes, consolidate related behavior into a single module.
- **Feature Envy**: Move methods that use another class's data extensively into that class where the data lives.
- **Insider Trading**: Reduce coupling between classes that access each other's private data. Use public interfaces or merge the classes.
- **Inappropriate Intimacy**: Classes that access each other's internals too much should either be merged or have their interface redesigned with proper encapsulation.

### Inheritance Issues
- **Refused Bequest**: If a subclass doesn't use inherited methods, use composition over inheritance. Extract shared behavior into a separate class used by both.
- **Parallel Inheritance Hierarchies**: When creating a subclass of one class forces creating a subclass of another, collapse or merge the hierarchies.
- **Alternative Classes with Different Interfaces**: If two classes do the same thing with different method names, unify their interfaces or create a common abstraction.

### Design Patterns Gone Wrong
- **Oddball Solution**: Use consistent approaches for similar problems across the codebase. If you solved authentication in module A, solve it the same way in module B.
- **Incomplete Library Class**: When a library doesn't provide needed functionality, create wrapper classes or extension methods rather than scattering workarounds throughout code.

### When Comments Smell
- **Comments as Deodorant**: Comments explaining complex code indicate the code should be refactored. Extract methods with descriptive names instead. Comments should explain "why" not "what".

### Global State & Dependencies
- **Global Data**: Eliminate global variables. Encapsulate data in classes or pass it explicitly through function parameters. Global state causes unpredictable bugs.
- **Hidden Dependencies**: Never instantiate concrete objects inside constructors. Accept dependencies as parameters (Dependency Injection) to enable testing and flexibility.

### Function Design Issues
- **Flag Arguments**: Replace boolean parameters that control behavior (e.g., `process(data, true)`) with separate, explicitly named functions. Example: `processWithValidation(data)` and `processWithoutValidation(data)`.
- **Output Parameters**: Avoid passing variables to modify them inside functions. Return new values instead. Refactor `updateUser(user)` to `updatedUser = createUpdatedUser(user)`.

### Logic Implementation Problems
- **Nested Conditionals**: Refactor "arrow anti-pattern" (if within if within if) using early returns, guard clauses, or extracting nested logic into separate functions. Keep indentation shallow.
- **Magic Numbers**: Replace literal numbers with named constants. Change `timeout = 86400` to `timeout = SECONDS_IN_A_DAY`. This documents meaning and enables reuse.
- **Type Code as Strings**: Replace string-based type codes ("Manager", "Engineer") with enums, dedicated classes, or polymorphism for type-safe, maintainable code.
- **Null Checks Everywhere**: Excessive null checking indicates missing abstractions. Use Null Object pattern, Optional types, or redesign to eliminate null as a valid state.
- **Temporary Variables**: Reduce temp variables that obscure logic flow. Either extract meaningful intermediate steps with descriptive names or eliminate unnecessary assignments.
- **Dead Code**: Delete unused functions, variables, or parameters immediately. Version control preserves history. Dead code confuses readers and clutters the codebase.
- **Complex Expressions**: Break dense one-liners into multiple statements with descriptive variable names. If an expression requires careful study, it should be decomposed.

### Object-Oriented Design Problems
- **Large Class**: Split classes doing too much (indicated by many instance variables or methods) into smaller, focused classes with single responsibilities.
- **Static Everything**: Overusing static methods prevents polymorphism, testing, and dependency injection. Use instance methods unless the function is truly stateless and generic.
- **Interface Bloat**: Split large interfaces using Interface Segregation Principle. Clients shouldn't implement methods they don't use. Create focused, role-specific interfaces.
- **Base Class Dependence**: Parent classes shouldn't know about or depend on their subclasses. If a base class calls subclass-specific code, restructure using Template Method or Strategy pattern.
- **Tightly Coupled Tests**: Tests should verify behavior, not implementation. If refactoring internal code breaks tests despite unchanged output, tests are over-specified.

### Architectural & Organizational Issues
- **Commented-Out Code**: Delete commented code immediately. It creates confusion about what's active. Use version control to preserve history; don't clutter source files.
- **Misplaced Responsibility**: Move methods to classes where they logically belong. If a method uses another class's data extensively, it probably belongs in that class.
- **Excessive Indirection**: Balance abstraction with clarity. If understanding simple logic requires jumping through 10 files, consolidate or reduce layers. Avoid abstraction for its own sake.
- **Useless Abstract Classes**: Remove abstractions created for a single implementation. Create abstractions when you have 2+ concrete implementations with shared behavior.
- **Over-engineered Patterns**: Use the simplest solution that works. Don't apply complex design patterns (Factory, Strategy, Singleton) when straightforward code suffices. YAGNI principle applies.
- **Missing Domain Language**: Code should speak the business domain language (Invoice, ShoppingCart) not computer science terms (HashMap, ArrayList). Domain concepts should be explicit types.
- **Feature Creep in Classes**: Resist adding "just one more thing" to existing classes. Each addition dilutes focus. Create new classes for new responsibilities.
- **Poorly Defined Boundaries**: Establish clear architectural layers (Data, Domain, Presentation). Each layer should have distinct responsibilities and controlled dependencies.

### Naming Consistency
- **Inconsistent Naming**: Use consistent terminology across the codebase. Don't mix `fetch`, `get`, `retrieve` for the same operation. Establish and follow naming conventions.

## Refactoring Techniques (Martin Fowler's Catalog)

### Basic Refactorings - Extraction & Inline
- **Extract Function**: When code fragment can be grouped together, turn it into a function with a name explaining its purpose. Apply when function is too long or needs comment to explain intent.
- **Inline Function**: When function body is as clear as its name, remove the function and inline its body. Use when indirection no longer provides value.
- **Extract Variable**: When expression is complex or appears multiple times, put result into variable with descriptive name. Makes code self-documenting.
- **Inline Variable**: When variable name doesn't communicate more than expression itself, remove it and use expression directly. Avoid over-extraction that obscures logic.
- **Replace Inline Code with Function Call**: When duplicated logic already exists as a function elsewhere, replace inline code with call to that function.

### Moving and Organizing Code
- **Move Function**: When function references elements in other contexts more than its own, move it to where it's most needed. Follow "put things together that change together".
- **Move Field**: When field is used more by another class than its own, move it to that class. Data should live close to code that uses it.
- **Move Statements into Function**: When same code appears before every call to a function, move it into the function itself. Reduces duplication.
- **Move Statements to Callers**: When function does too much and some behavior needs to vary between callers, move varying parts back to callers. Opposite of above.
- **Slide Statements**: Move related statements next to each other to improve code locality. Group related code before extracting.

### Conditionals & Control Flow
- **Decompose Conditional**: When complex conditional logic is hard to read, extract condition and both branches into separate functions with descriptive names.
- **Consolidate Conditional Expression**: When multiple conditionals check different things but result in same action, combine into single expression that captures intent.
- **Replace Nested Conditional with Guard Clauses**: When nested conditionals obscure normal flow, use guard clauses (early returns) for special cases. Put happy path at lowest indentation.
- **Replace Conditional with Polymorphism**: When behavior varies by type and you have switch/if-else checking types, create subclasses and let polymorphism handle behavior selection.
- **Introduce Special Case**: When special-case behavior (like null checks) appears everywhere, create special-case object (Null Object pattern) that provides default behavior.

### Loops & Pipelines
- **Replace Loop with Pipeline**: When processing collections with loops, replace with pipeline operations (map, filter, reduce). Makes transformations explicit and composable.
- **Split Loop**: When loop does multiple things, split into separate loops each doing one thing. Enables easier extraction and optimization.

### Function Signatures & Parameters
- **Change Function Declaration**: Rename functions to better reveal intent. Modify parameters to make function easier to understand and use.
- **Rename Variable**: When variable name doesn't clearly express purpose, rename it to something more descriptive. Good names are fundamental to readability.
- **Rename Field**: When field name is unclear or misleading, rename it. Data should be as self-explanatory as behavior.
- **Introduce Parameter Object**: When functions share 3+ parameters appearing together, group them into object. Creates semantic unity.
- **Preserve Whole Object**: When passing multiple values from an object as parameters, pass whole object instead. Reduces parameter count and makes dependencies explicit.
- **Replace Parameter with Query**: When function parameter can be obtained by calling method on another parameter, remove it and make recipient call the method.
- **Replace Query with Parameter**: When function depends on global state or external context, pass needed value as parameter. Makes dependencies explicit for testing.
- **Remove Flag Argument**: When boolean parameter controls function behavior, split into separate functions. Makes call sites self-documenting.
- **Parameterize Function**: When multiple functions differ only in literal values, create single function with parameter for varying value.

### Variables & Encapsulation
- **Encapsulate Variable**: When data is accessed directly, hide it behind getter/setter functions. Enables monitoring changes and adding logic later.
- **Encapsulate Collection**: When getter returns collection directly, provide methods to add/remove elements instead. Prevents external modification of internal state.
- **Encapsulate Record**: When using record structures (dicts, plain objects), wrap in class. Enables evolution and encapsulation.
- **Replace Temp with Query**: When temporary variable holds result of expression, extract expression into function and replace references. Enables reuse.
- **Split Variable**: When variable is assigned multiple times for different purposes, create separate variable for each purpose. Each variable should have single responsibility.
- **Replace Derived Variable with Query**: When variable holds calculated value that can become stale, calculate it on-demand instead. Eliminates synchronization issues.

### Class Organization
- **Extract Class**: When class does work of two or more, create new class and move related fields/methods. Each class should have single, well-defined responsibility.
- **Inline Class**: When class isn't doing enough to justify existence, move all features into another class and delete it. Removes unnecessary indirection.
- **Combine Functions into Class**: When group of functions operate on same data, create class and make functions methods. Encapsulates shared state.
- **Combine Functions into Transform**: When group of functions derive information from source data, create transform function that produces enriched version. Alternative to class for immutable data.
- **Split Phase**: When code does multiple distinct things, separate into sequential phases each with clear input and output. Makes dependencies explicit.

### Hierarchy Refactorings
- **Extract Superclass**: When two classes share similar features, create superclass and move common features up. Captures shared behavior.
- **Pull Up Method**: When methods in subclasses do same thing, move identical methods to superclass. Eliminates duplication in hierarchy.
- **Pull Up Field**: When subclasses have same field, move it to superclass. Declares shared data at appropriate level.
- **Pull Up Constructor Body**: When subclass constructors do similar work, extract common code into superclass constructor and call it.
- **Push Down Method**: When superclass behavior is relevant only for some subclasses, move it to those subclasses. Removes irrelevant behavior from parent.
- **Push Down Field**: When field used only by some subclasses, move it to those subclasses. Clarifies where data is actually needed.
- **Replace Subclass with Delegate**: When inheritance used only for behavioral variation, use composition with strategy object instead. Favor composition over inheritance.
- **Replace Superclass with Delegate**: When subclass uses only part of superclass interface, replace inheritance with delegation. Use when "is-a" relationship doesn't hold.
- **Collapse Hierarchy**: When subclass isn't different enough to justify existence, merge it with superclass. Reduces unnecessary complexity.
- **Remove Subclass**: When subclass provides too little value, remove it and replace with field in superclass. Simplifies class structure.
- **Replace Type Code with Subclasses**: When type code affects class behavior, replace with subclasses for each type. Enables polymorphism.

### Data & Primitives
- **Replace Primitive with Object**: When primitive value has associated behavior or validation, wrap it in class. Creates proper abstraction for domain concept.
- **Change Value to Reference**: When many identical instances of class should be single shared instance, make it reference object. Useful for lookup data.
- **Change Reference to Value**: When reference object is small and immutable, make it value object. Simplifies identity and sharing concerns.

### Commands & Assertions
- **Replace Function with Command**: When function needs complex configuration, undo, or lifecycle management, turn it into command object. Provides more control.
- **Replace Command with Function**: When command object wraps single method without complex needs, replace with simple function. Removes unnecessary object.
- **Introduce Assertion**: When code assumes something is always true, state assumption explicitly with assertion. Documents assumptions and fails fast.
- **Separate Query from Modifier**: When method returns value AND changes state, split into two methods. Queries shouldn't have side effects (Command-Query Separation).

### Dealing with Inheritance Problems
- **Replace Constructor with Factory Function**: When constructor has limitations (naming, return type), replace with factory function. Provides more flexibility.
- **Remove Setting Method**: When field should be set only at creation, remove setter. Communicates immutability.
- **Hide Delegate**: When client calls method on object received from another object, create wrapper method that hides delegation. Reduces coupling.
- **Remove Middle Man**: When class does too much simple delegation, let client call delegate directly. Removes unnecessary forwarding.

### Algorithmic Changes
- **Substitute Algorithm**: When clearer way to do something exists, replace entire algorithm body. Sometimes wholesale replacement beats incremental change.
- **Remove Dead Code**: When code is never used, delete it. Dead code imposes mental burden without benefit. Version control preserves history.
