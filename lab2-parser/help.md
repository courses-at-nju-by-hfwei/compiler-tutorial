# Lab2指导文档

## 打印语法树的方式
Antlr提供两种遍历语法树的方式，分别为Listener和Visitor，这里仅介绍Visitor的使用，对Listener感兴趣的同学请自行了解
- Visitor的基本使用
```java
    ParseTree tree = sysYParser.program();
    SysYParserBaseVisitor visitor = new SysYParserBaseVisitor();
    visitor.visit(tree);
```
- Visitor默认将对语法树进行深度优先遍历，你可以自己实现Visitor控制对子节点的遍历顺序，可以自行定义遍历节点时以及遍历子节点前后的行为
- Visitor在访问每个节点的子节点前会调用**visitChildren**函数，本次实验中你可以通过重写这个函数来实现语法树的打印
- Visitor在访问每个终结符节点时会调用**visitTerminal**函数，打印终结符及终结符的高亮可以通过重写这个函数来实现
- 对于每一个语法规则都存在一个对应的visit函数，如exp规则对应的函数为**visitExp**，在后续实验中需要访问特定节点并做相应处理，此时需要调用这种函数

## 根据语法规则节点获取规则名称
- 每个语法规则都有其编号，根据在.g4文件中出现的顺序确定
- 通过语法规则节点(node)的上下文(ruleContext)可以获得语法规则的编号
- 通过SysYParser的静态成员ruleNames可以获得所有语法规则的名称，下标与编号对应

## 语法高亮
- 一个一个写太多了，可以通过表驱动实现，一个初步的表可以从SysYLexer的ruleNames获得，只需要在数组对应的位置添加颜色即可

## 选做部分
- 请注意这部分需要另一个.g4文件SysYParser1.g4生成新的语法分析器去处理相同的词法单元的输入