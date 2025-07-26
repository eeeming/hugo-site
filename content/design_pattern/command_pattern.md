---
date: '2025-07-26T15:08:16+08:00'
draft: true
title: 'Command Pattern'
math: true
cascade:
  type: docs
---

## The Command Pattern

### Example

我们有一个遥控器，上面有两个按钮 `on` 和 `off`, 我们希望这个遥控器可以控制不同电器。

事实上，如果我们把全部具体控制各个电器的代码写在这个控制器上，那么这个遥控器就变得非常复杂了。
如果我们想要添加一个新的电器，我们需要修改遥控器的代码，这样就违反了开闭原则。
为了避免这种情况，我们可以使用命令模式（Command Pattern）来解耦遥控器和电器之间的关系。

```java
public interface Command {
    void execute();
}
```

具体的实现

```java
public class LightOnCommand implements Command {
    Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.on();
    }
}

public class LightOffCommand implements Command {
    Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.off();
    }
}
```

除了开灯和关灯，我们希望遥控器还可以控制播放机（Stereo）的CD播放

```java
public class StereoOnWithCDCommand implements Command {
    Stereo stereo;

    public StereoOnWithCDCommand(Stereo stereo) {
        this.stereo = stereo;
    }

    public void execute() {
        stereo.on();
        stereo.setCD();
        stereo.setVolume(11);
    }
}

public class StereoOffCommand implements Command {
    Stereo stereo;

    public StereoOffCommand(Stereo stereo) {
        this.stereo = stereo;
    }

    public void execute() {
        stereo.off();
    }
}
```

由于遥控器还有很多没有使用的按钮，我们使用NoCommand来处理这些空的按钮。

```java
public class NoCommand implements Command {
    public void execute() {
        // 什么都不做
    }
}
```

{{< callout type="info" >}}
NoCommand 对象是一个空对象。空对象在你没有有意义的对象可以返回时非常有用。可以避免反复判断是否为 null。
{{< /callout >}}

我们可以构建我们的遥控器类，它将 Command 对象存储在数组中，并提供一个方法来设置每个按钮的命令。

```java
public class RemoteControl {
    Command[] onCommands;
    Command[] offCommands;

    public RemoteControl() {
        onCommands = new Command[7];
        offCommands = new Command[7];

        // 初始化所有按钮的命令为 NoCommand
        // 好设计! 避免了空指针异常
        Command noCommand = new NoCommand();
        for (int i = 0; i < 7; i++) {
            onCommands[i] = noCommand;
            offCommands[i] = noCommand;
        }
    }

    public void setCommand(int slot, Command onCommand, Command offCommand) {
        onCommands[slot] = onCommand;
        offCommands[slot] = offCommand;
    }

    public void onButtonWasPushed(int slot) {
        onCommands[slot].execute();
    }

    public void offButtonWasPushed(int slot) {
        offCommands[slot].execute();
    }

    public String toString() {
        StringBuffer stringBuff = new StringBuffer();
        stringBuff.append("\n------ Remote Control ------\n");
        for (int i = 0; i < onCommands.length; i++) {
            stringBuff.append("[slot " + i + "] " + onCommands[i].getClass().getName()
                + "   " + offCommands[i].getClass().getName() + "\n");
        }
        return stringBuff.toString();
    }
}
```

使用示例

```java
public class RemoteLoader{
    public static void main(String[] args) {
        RemoteControl remoteControl = new RemoteControl();

        Light light = new Light();
        LightOnCommand lightOn = new LightOnCommand(light);
        LightOffCommand lightOff = new LightOffCommand(light);

        Stereo stereo = new Stereo();
        StereoOnWithCDCommand stereoOn = new StereoOnWithCDCommand(stereo);
        StereoOffCommand stereoOff = new StereoOffCommand(stereo);

        remoteControl.setCommand(0, lightOn, lightOff);
        remoteControl.setCommand(1, stereoOn, stereoOff);

        System.out.println(remoteControl);

        remoteControl.onButtonWasPushed(0); // 开灯
        remoteControl.offButtonWasPushed(0); // 关灯

        remoteControl.onButtonWasPushed(1); // 开音响
        remoteControl.offButtonWasPushed(1); // 关音响
    }
}
```

如果使用Lambda表达式会更简单

```java
public class RemoteLoader{
    public static void main(String[] args) {
        RemoteControl remoteControl = new RemoteControl();
        Light light = new Light();
        Stereo stereo = new Stereo();

        remoteControl.setCommand(0, () -> light.on(), () -> light.off());
        remoteControl.setCommand(1, () -> {
            stereo.on();
            stereo.setCD();
            stereo.setVolume(11);
        }, () -> stereo.off());
        System.out.println(remoteControl);
        remoteControl.onButtonWasPushed(0); // 开灯
        remoteControl.offButtonWasPushed(0); // 关灯
        remoteControl.onButtonWasPushed(1); // 开音响
        remoteControl.offButtonWasPushed(1); // 关音响
    }
}
```

### The Command Pattern defined

The Command Pattern encapsulates a request as an object, thereby letting you parameterize other objects with different requests, queue or log requests, and support undoable operations.  
命令模式将请求封装为一个对象，从而允许你使用不同的请求来参数化其他对象，对请求进行排队或记录请求，并支持可撤销的操作。

* Client 的责任就是把具体的 Command 进行实现，包括实例化 Command 接口并设置其接收者（Receiver）。
* Receiver 是 Command 中实际的执行者
* Invoker 是调用者，负责调用 Command 的 execute 方法，不关心具体的实现细节。
* 例如在遥控器中，遥控器是 Invoker，里面存储着具体的 Command 对象，每个 Command 对象都负责存储对 Receiver 的具体操作。

{{% details title="为什么需要 Receiver, 而不是在 Command 中直接实现具体的操作？" %}}

因为这样可以让 Command 和 Receiver 解耦，Command 只需要知道如何调用 Receiver，而不需要知道 Receiver 的具体实现细节。这样可以让代码更易于维护和扩展。

{{% /details %}}

{{% details title="如何实现多次撤销操作的历史记录？" %}}

使用一个栈来保存历史记录，每次执行命令时将命令压入栈中，撤销时从栈中弹出命令并执行其撤销操作。

{{% /details %}}

### MacroCommand

有时候我们需要一次执行多个命令，这时可以使用宏命令（MacroCommand）。宏命令是一个命令对象，它包含了多个其他命令对象，并且可以一次性执行所有这些命令。

```java
public class MacroCommand implements Command {
    Command[] commands;
    public MacroCommand(Command[] commands) {
        this.commands = commands;
    }
    public void execute() {
        for (Command command : commands) {
            command.execute();
        }
    }
} 
```

{{% details title="为什么实例化一个ABCDCommand，把需要的操作[A,B,C,D]放在一起？" %}}

没必要。用MacroCommand可以利用参数`Command[] commands`，动态决定哪些Commands要包含在MacroCommand中，因此使用MacroCommand会有更多的灵活性。

{{% /details %}}