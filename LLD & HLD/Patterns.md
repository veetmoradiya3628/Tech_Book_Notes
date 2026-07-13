
#### Command pattern
- Behavioral pattern 
- The Command Pattern encapsulates a request as an object. Instead of calling methods directly on an object, we create a Command object representing that request. This decouples the object that invokes the operation from the object that actually performs it. Since commands are objects, they can be stored, queued, logged, scheduled, retried, or undone.
- Four main participants
	- Client
	- Invoker
	- Command
	- Receiver

```
User presses button

↓

Remote

↓

command.execute()

↓

TurnOnTVCommand

↓

tv.turnOn()

↓

TV ON
```

- Commands are objects, objects can be stored, serialized, logged, queued, scheduled etc
- Real-word use cases
	- IDE - copy, paste, undo, redo all are command only
	- Text editor - insert, delete, replace, undo
- Macro Command 
- Undo Redo
- Multiple Undo
- Mature design of Smart Home Automation with Command

```                User
                  │
                  ▼
          Voice / Mobile / Remote
                  │
                  ▼
            Command Factory
                  │
                  ▼
               Command
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
   SimpleCommand      MacroCommand
                              │
                         List<Command>
                  │
                  ▼
          Scheduler / Queue / History
                  │
                  ▼
            SmartHome Hub
                  │
                  ▼
        Receivers (Light, TV, Fan, AC...)
                  │
                  ▼
         Internal State Machines
```

