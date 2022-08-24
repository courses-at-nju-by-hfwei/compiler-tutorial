# Lab1指导文档
## 第一步
首先你需要根据[SysY语言定义](../docs/SysY%E8%AF%AD%E8%A8%80%E5%AE%9A%E4%B9%89.pdf)中的词法规则和语法规则编写两个.g4文件，分别为SysYLexer.g4和SysYParser.g4，然后为其生成词法分析器和语法分析器

## 第二步
你的Main.java应当接收一个参数，即文件路径，并将文件内容传给词法分析器
```java 
    public static void main(String[] args) throws IOException {
        if (args.length < 1) {
            System.err.println("input path is required");
        }
        String source = args[0];
        CharStream input = CharStreams.fromFileName(source);
        SysYLexer sysYLexer = new SysYLexer(input);
    }
```
## 第三步
antlr的词法分析器遇到错误时会向ANTLRErrorListener发送错误信息，自动生成的SysYLexer有一个默认的ErrorListener，你需要自行实现一个继承自BaseErrorListener的ErrorListener并添加给SysYLexer
```java 
    sysYLexer.removeErrorListeners();
    sysYLexer.addErrorListener(myErrorListener);
```

## 第四步
将lexer转换为tokens传给parser，parser在生成语法树时会自动进行词法检查
```java 
    CommonTokenStream tokens = new CommonTokenStream(sysYLexer);
    SysYParser sysYParser = new SysYParser(tokens);
    ParseTree tree = sysYParser.program();
```