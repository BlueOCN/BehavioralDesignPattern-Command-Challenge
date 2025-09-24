## Java Design Patterns: Behavioral

---

# Command Pattern
The Command Design Pattern in Java is a behavioral pattern that encapsulates a request as an object, allowing you to parameterize clients with different requests, queue or log requests, and support undoable operations.

It decouples the sender (invoker) from the receiver (the object that performs the action). Instead of calling a method directly, the sender uses a command object that implements a common interface—typically with an execute() method. This abstraction allows flexible command execution, delayed invocation, and reversibility.

## Key Components
- **Command** Interface: Declares the execute() method.
- **Concrete** Command: Implements the command and defines the binding between the receiver and the action.
- **Receiver**: Knows how to perform the actual work.
- **Invoker**: Triggers the command without knowing its implementation.

## Advantages
- Decouples sender and receiver
- Supports undo/redo functionality
- Enables request queuing and scheduling
- Promotes extensibility
- Improves code organization
- Facilitates macro commands
- Enhances testing and debugging

# Composite Pattern Architecture

![alt text](src/main/resources/CommandPattern.svg)

The GUI class is the application’s entry point. It instantiates a Document (the receiver) and creates two ConcreteCommand objects—PrintCommand and SaveCommand—each holding a reference to that Document. Both ConcreteCommands implement the Command interface by defining execute(), which delegates to document.print() or document.save(). Button acts as the invoker: it stores a Command and calls click(command), which in turn invokes command.execute().

By encapsulating requests as Command objects, this design decouples the invoker (Button) from the receiver (Document). GUI can wire any command to any button without changing their implementations, adhering to the Open/Closed Principle. Each command is a single-focus class, improving testability and maintenance. Moreover, by treating actions as objects, the architecture can support queuing, logging, or undo/redo of commands, while keeping the UI layer agnostic of the actual business logic.

# Command Pattern Usage

Command Pattern usage example for GUI.
```java
Button saveButton = new Button("Save");
Button printButton = new Button("Print");

saveButton.click(new SaveCommand(document));
printButton.click(new PrintCommand(document));
```

# Best Practices for Implementing the Command Pattern
- **Define a clear Command interface**: Keep it minimal—typically just execute(). If you need undo functionality, consider adding undo() or rollback().
- **Use descriptive command names**: Name your concrete commands based on intent, like CreateUserCommand or SendEmailCommand, to improve readability and traceability.
- **Avoid bloated command classes**: Each command should encapsulate a single, focused action. If it starts doing too much, break it down into smaller commands or use a composite.
- **Inject dependencies via constructor**: Pass the receiver and any required parameters through the constructor to keep commands immutable and testable.
- **Support undo/redo with command history**: Store executed commands in a stack or queue to enable undo operations. This is especially useful in UI or transactional systems.
- **Leverage functional interfaces for lightweight commands**: For simple use cases, Java’s Runnable or custom @FunctionalInterface can reduce boilerplate.
- **Use command factories for dynamic creation**: If commands vary by context (e.g. user role, request type), a factory or registry can help instantiate the right command cleanly.
- **Log command execution for auditability**: Especially in microservices, logging command invocations helps with debugging and tracing system behavior.
- **Test commands in isolation**: Since commands encapsulate behavior, they’re perfect for unit testing—mock the receiver and validate outcomes.
