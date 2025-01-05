# Quality

Simple code is higher quality. Complex code is lower quality. Complexity reduces maintainability, extendability, and understandability.

Code that is seperable tends to be simpler. That means layers and reasons to have those layers. The standard web application layers are:

- Presentation
- API or entry points
- Application or service layer
- Domain or business logic
- Data access or adapters
- Database or persistance

## Diagrams

If I have Java, I can use plantuml.

Sequence diagram:

```uml
@startuml
Alice -> Bob: Hello
Bob -> Alice: Hi
@enduml
```

```bash
plantuml -ttxt input.puml
```

Or, I can embed it like this:

<div hidden>
```
@startuml firstDiagram

Alice -> Bob: Hello
Bob -> Alice: Hi!
		
@enduml
```
</div>

![](firstDiagram.svg)
