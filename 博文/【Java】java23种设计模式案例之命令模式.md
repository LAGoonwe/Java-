---
title: 【Java】java23种设计模式案例之命令模式
date: 2019-03-26
tags: java
---
<meta name="referrer" content="no-referrer" />

##  【Java】java23种设计模式案例之命令模式
><a href="http://www.runoob.com/design-pattern/command-pattern.html">命令模式定义参考</a>


>例子:              <a href="https://github.com/LAGoonwe/Commandmode">源代码</a>

3种案例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328202229793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

>1.Command案例
>问题描述：一个指挥官请求（命令）三连偷袭敌人
>代码

```java
package com.lagoon.command;

/**
 * 问题描述：一个指挥官请求（命令）三连偷袭敌人
 * 这个类扮演的是接收者角色
 */
public class CompanyArmy {

    public void sneakAttack(){
        System.out.println("我们知道如何偷袭敌人，保证完成任务！");
    }
}

```

```java
package com.lagoon.command;

/**
 * 这个接口扮演的是命令接口角色
 */
public interface Command {
    public abstract void execute();
}

```

```java
package com.lagoon.command;

/**
 * 这个类扮演的是具体命令的角色
 */
public class ConcreteCommand implements Command{

    CompanyArmy companyArmy;  //含有接收者的引用
    ConcreteCommand(CompanyArmy companyArmy){
        this.companyArmy=companyArmy;
    }

    @Override
    public void execute() {  //封装着指挥官的请求
        companyArmy.sneakAttack();  //偷袭敌人

    }
}

```

```java
package com.lagoon.command;

/**
 * 这个类代表请求者，也就是指挥官的角色
 */
public class ArmySuperior {

    Command command;  //用来存放具体命令的引用
    public void setCommand(Command command){
        this.command=command;
    }
    public void startExecuteCommand(){
        command.execute();
    }
}

```

```java
package com.lagoon.command;

/**
 * 该类为main方法，演示一个指挥官下发命令如何请求三连偷袭敌人
 */
public class Application {
    public static void main(String[] args) {
        //创建接收者
        CompanyArmy 三连=new CompanyArmy();
        //创建具体命令并指定接收者
        Command command=new ConcreteCommand(三连);
        //创建请求者
        ArmySuperior 指挥官=new ArmySuperior();
        //下发命令
        指挥官.setCommand(command);
        //开始执行命令
        指挥官.startExecuteCommand();
        //执行结果：输出语句，我们知道如何偷袭敌人，保证完成任务！
    }
}

```


>2.Dir命令，演示命令模式的可撤销操作
>问题描述：该问题描述的是请求者请求在硬盘上建立目录，还可以撤销请求，这就要求接收者不仅可以在硬盘上建立目录，也可以删除上一次请求所建立的目录
>代码：

```java
package com.lagoon.Dir;

/**
 * 该问题描述的是请求者请求在硬盘上建立目录，还可以撤销请求，这就要求接收者不仅可以在硬盘上建立目录，也可以删除上一次请求所建立的目录
 * 该类扮演的是接收者角色，即既可以新增目录，也可以删除目录
 */
import java.io.*;
public class MakeDir {
    public void createDir(String name){
        File dir=new File(name);
        dir.mkdir();
    }

    public void deleteDir(String name){
        File dir=new File(name);
        dir.delete();
    }
}

```

```java
package com.lagoon.Dir;

/**
 * 该类为命令接口类,代表既可以执行命令，也可以撤销收回命令
 */
public interface Command {
    public abstract void execute(String name);
    public abstract void undo();
}

```

```java
package com.lagoon.Dir;

import java.util.ArrayList;

/**
 *该类为具体命令类
 */
public class ConcreteCommand implements Command{
    ArrayList<String> dirNameList;
    MakeDir makeDir;
    ConcreteCommand(MakeDir makeDir){
        dirNameList=new ArrayList<String>();
        this.makeDir=makeDir;
    }
    @Override
    public void execute(String name) {
        makeDir.createDir(name);
        dirNameList.add(name);

    }

    @Override
    public void undo() {
        if (dirNameList.size()>0){
            int m=dirNameList.size();
            String str=dirNameList.get(m-1);
            makeDir.deleteDir(str);
            dirNameList.remove(m-1);
        }
        else
            System.out.println("没有需要撤销的操作");

    }
}

```

```java
package com.lagoon.Dir;

/**
 * 该类为请求者角色
 */
public class RequestMakeDir {
    Command command;
    public void setCommand(Command command){
        this.command=command;
    }
    public void startExecuteCommand(String name){
        command.execute(name);
    }
    public void undoCommand(){
        command.undo();
    }
}

```

```java
package com.lagoon.Dir;

import java.util.Iterator;

/**
 * 该类为发射类
 */
public class Application {
    public static void main(String[] args) {
        //创建接收者
        MakeDir makeDir=new MakeDir();
        //创建具体命令并指定接收者
        Command command=new ConcreteCommand(makeDir);
        RequestMakeDir requestMakeDir=new RequestMakeDir();
        requestMakeDir.setCommand(command);
        //建立名字是java的目录
        requestMakeDir.startExecuteCommand("java");
        //建立名字是c的目录
        requestMakeDir.startExecuteCommand("c");
        //建立名字是c++的目录
        requestMakeDir.startExecuteCommand("c++");
        //撤销命令，删除名字是c++的目录
        requestMakeDir.undoCommand();
        //撤销命令，删除名字是c的目录
        requestMakeDir.undoCommand();

        //查看当前列表里的目录
        Iterator<String> iterator = ((ConcreteCommand) command).dirNameList.iterator();
        if (iterator.hasNext()){
            System.out.println(iterator.next());
        }

        //运行结果，输出一个列表目录，java，说明其他被撤销成功
    }
}

```


>3.Letter案例
>问题描述：请求者可以请求只输出英文字母表，俄文字母表或1-n之间的偶数
 也可以请求三种都输出
 代码：

```java
package com.lagoon.Letter;

/**
 * 该文件夹演示宏命令
 * 宏命令也是一个具体命令，只不过他包含了其他命令的引用
 * 执行一个宏命令，相当于执行了许多的具体命令
 * 该类为接收者角色
 * 问题描述：请求者可以请求只输出英文字母表，俄文字母表或1-n之间的偶数
 * 也可以请求三种都输出
 */
public class PrintLetter {
    public void printEnglish(){
        for (char c='a';c<='z';c++){
            System.out.println(" "+c);
        }
    }

    public void printRussian(){
        for (char c='а';c<='я';c++){
            System.out.println(" "+c);
        }
    }
}

```

```java
package com.lagoon.Letter;

/**
 * 命令接口类
 */
public interface Command {
    public abstract void execute();
}

```

```java
package com.lagoon.Letter;

//具体命令之输出英文字母表命令
public class PrintEnglishCommand implements Command{
    PrintLetter letter;

    public PrintEnglishCommand(PrintLetter letter) {
        this.letter = letter;
    }
    public void execute(){
        letter.printEnglish();
    }
}

```

```java
package com.lagoon.Letter;

//具体命令之输出俄文字母表
public class PrintRussianCommand implements Command{
    PrintLetter letter;

    public PrintRussianCommand(PrintLetter letter) {
        this.letter = letter;
    }

    @Override
    public void execute() {
        letter.printRussian();
    }

}

```

```java
package com.lagoon.Letter;

import java.util.ArrayList;

//宏命令，执行所有命令
public class MacroCommand implements Command{

    ArrayList<Command> commandArrayList;  //把所有的命令存进数组

    public MacroCommand(ArrayList<Command> commandArrayList) {
        this.commandArrayList = commandArrayList;
    }

    @Override
    public void execute() {
        for (int k=0;k<commandArrayList.size();k++){
            Command command=commandArrayList.get(k);  //循环定位到命令
            command.execute(); //执行命令
        }
    }
}

```

```java
package com.lagoon.Letter;

//该类为请求者角色
public class RequestMakedir {
    Command command;

    public void setCommand(Command command) {
        this.command = command;
    }
    public void startExecuteCommand(){
        command.execute();
    }
}

```

```java
package com.lagoon.Letter;

import java.util.ArrayList;

//发射类，main方法
public class Application {
    public static void main(String[] args) {
        ArrayList<Command> list= new ArrayList<>();
        //创建请求者
        RequestMakedir requestMakedir=new RequestMakedir();
        //创建命令具体接收者
        Command command1=new PrintEnglishCommand(new PrintLetter());
        Command command2=new PrintRussianCommand(new PrintLetter());

        //整合命令
        list.add(command1);
        list.add(command2);


        //创宏命令
        Command macroCommand=new MacroCommand(list);

        System.out.println("单独输出英文字母表:");
        requestMakedir.setCommand(command1);
        requestMakedir.startExecuteCommand();

        System.out.printf("%n用一个宏命令输出所有:%n");
        requestMakedir.setCommand(macroCommand);
        requestMakedir.startExecuteCommand();
    }
}

```

