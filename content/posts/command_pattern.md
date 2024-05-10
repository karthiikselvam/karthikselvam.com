---
title: "Command Pattern"
description: "2024"
date: "2024-05-10"
tags:
- interview
---

1. Implement Transaction Management: Extend the Command Pattern implementation to support transaction management. This exercise will involve implementing methods to start, commit, and rollback transactions, where a transaction consists of multiple commands that are executed atomically.

```java
// Command interface
interface Command {
    void execute();
    void undo();
}

// Concrete command
class ConcreteCommand implements Command {
    private Receiver receiver;

    ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    public void execute() {
        receiver.action();
    }

    public void undo() {
        receiver.undoAction();
    }
}

// Receiver
class Receiver {
    void action() {
        System.out.println("Receiver is executing action...");
    }

    void undoAction() {
        System.out.println("Receiver is undoing action...");
    }
}

// Invoker
class Invoker {
    private List<Command> commands = new ArrayList<>();

    void addCommand(Command command) {
        commands.add(command);
    }

    void executeCommands() {
        for (Command command : commands) {
            command.execute();
        }
    }

    void undoCommands() {
        for (int i = commands.size() - 1; i >= 0; i--) {
            commands.get(i).undo();
        }
    }
}

// Client
public class Client {
    public static void main(String[] args) {
        Receiver receiver = new Receiver();
        Invoker invoker = new Invoker();

        // Start transaction
        System.out.println("Starting transaction...");
        
        // Add commands to the transaction
        invoker.addCommand(new ConcreteCommand(receiver));
        invoker.addCommand(new ConcreteCommand(receiver));
        invoker.addCommand(new ConcreteCommand(receiver));

        // Execute commands
        invoker.executeCommands();

        // Commit transaction
        System.out.println("Committing transaction...");

        // Start another transaction
        System.out.println("Starting another transaction...");

        // Add commands to the new transaction
        invoker.addCommand(new ConcreteCommand(receiver));
        invoker.addCommand(new ConcreteCommand(receiver));

        // Execute commands
        invoker.executeCommands();

        // Rollback transaction
        System.out.println("Rolling back transaction...");
        invoker.undoCommands();
    }
}


```
2. Implement Remote Control for Devices: Create a remote control application that allows users to control various devices (e.g., lights, TV, stereo) using the Command Pattern. Each device action (e.g., turning on/off, changing volume) should be encapsulated as a command object.

```java

// Receiver
class Light {
    void turnOn() {
        System.out.println("Light is on");
    }

    void turnOff() {
        System.out.println("Light is off");
    }
}

class TV {
    void turnOn() {
        System.out.println("TV is on");
    }

    void turnOff() {
        System.out.println("TV is off");
    }

    void changeVolume(int volume) {
        System.out.println("TV volume changed to " + volume);
    }
}

// Command interface
interface Command {
    void execute();
}

// Concrete commands
class LightOnCommand implements Command {
    private Light light;

    LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOn();
    }
}

class LightOffCommand implements Command {
    private Light light;

    LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOff();
    }
}

class TVOnCommand implements Command {
    private TV tv;

    TVOnCommand(TV tv) {
        this.tv = tv;
    }

    public void execute() {
        tv.turnOn();
    }
}

class TVOffCommand implements Command {
    private TV tv;

    TVOffCommand(TV tv) {
        this.tv = tv;
    }

    public void execute() {
        tv.turnOff();
    }
}

class TVVolumeCommand implements Command {
    private TV tv;
    private int volume;

    TVVolumeCommand(TV tv, int volume) {
        this.tv = tv;
        this.volume = volume;
    }

    public void execute() {
        tv.changeVolume(volume);
    }
}

// Invoker
class RemoteControl {
    private Command command;

    void setCommand(Command command) {
        this.command = command;
    }

    void pressButton() {
        command.execute();
    }
}

// Client
public class RemoteControlClient {
    public static void main(String[] args) {
        // Create devices
        Light light = new Light();
        TV tv = new TV();

        // Create commands
        Command lightOn = new LightOnCommand(light);
        Command lightOff = new LightOffCommand(light);
        Command tvOn = new TVOnCommand(tv);
        Command tvOff = new TVOffCommand(tv);
        Command tvVolume = new TVVolumeCommand(tv, 20); // Set initial volume to 20

        // Create remote control
        RemoteControl remoteControl = new RemoteControl();

        // Associate commands with remote control buttons
        remoteControl.setCommand(lightOn); // Button 1 turns on the light
        remoteControl.pressButton();

        remoteControl.setCommand(tvOn); // Button 2 turns on the TV
        remoteControl.pressButton();

        remoteControl.setCommand(tvVolume); // Button 3 changes TV volume
        remoteControl.pressButton();

        remoteControl.setCommand(tvOff); // Button 4 turns off the TV
        remoteControl.pressButton();

        remoteControl.setCommand(lightOff); // Button 5 turns off the light
        remoteControl.pressButton();
    }
}


```

3. Implement Undo/Redo functionality: Enhance the Command Pattern implementation to support undo and redo operations. This exercise will require you to maintain a history of executed commands and implement methods to undo and redo commands.

``` java

import java.util.Stack;

// Receiver
class Light {
    void turnOn() {
        System.out.println("Light is on");
    }

    void turnOff() {
        System.out.println("Light is off");
    }
}

// Command interface
interface Command {
    void execute();
    void undo();
}

// Concrete commands
class LightOnCommand implements Command {
    private Light light;

    LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOn();
    }

    public void undo() {
        light.turnOff();
    }
}

class LightOffCommand implements Command {
    private Light light;

    LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOff();
    }

    public void undo() {
        light.turnOn();
    }
}

// Invoker
class RemoteControl {
    private Stack<Command> undoStack = new Stack<>();
    private Stack<Command> redoStack = new Stack<>();

    void executeCommand(Command command) {
        command.execute();
        undoStack.push(command);
        redoStack.clear(); // Clear redo stack when a new command is executed
    }

    void undo() {
        if (!undoStack.isEmpty()) {
            Command command = undoStack.pop();
            command.undo();
            redoStack.push(command);
        } else {
            System.out.println("Nothing to undo");
        }
    }

    void redo() {
        if (!redoStack.isEmpty()) {
            Command command = redoStack.pop();
            command.execute();
            undoStack.push(command);
        } else {
            System.out.println("Nothing to redo");
        }
    }
}

// Client
public class RemoteControlClient {
    public static void main(String[] args) {
        // Create devices
        Light light = new Light();

        // Create commands
        Command lightOn = new LightOnCommand(light);
        Command lightOff = new LightOffCommand(light);

        // Create remote control
        RemoteControl remoteControl = new RemoteControl();

        // Execute commands
        remoteControl.executeCommand(lightOn); // Turn on the light
        remoteControl.executeCommand(lightOff); // Turn off the light

        // Undo the last command
        remoteControl.undo(); // Should turn on the light

        // Redo the last undone command
        remoteControl.redo(); // Should turn off the light
    }
}


```