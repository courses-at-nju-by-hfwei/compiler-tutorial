# Lab2:Parser 实验要求


## Part1 语法分析与高亮
- 本次实验需要完成语法分析与高亮，要求如下
    - 将SysYParser.g4中exp和cond写成左递归
    - 当输入文件存在语法错误时报告语法错误在哪一行，格式为Error type B at line lineNo:errorMessage 其中lineNo为行号,errorMessage可自行定义
    - 当输入文件不存在语法错误时打印语法树并按照规定格式输出(含高亮)

## Part2 消除左递归（选做）
- 将已经修改为左递归的exp的最后个生成式使用课堂上学过的方式修改为包含exp和exp1的右递归
    - 若存在语法错误则应仅输出一次语法错误
    - 若无语法错误则先输出Part1要求的输出再输出本Part要求的输出

## 改成左递归的形式
``` antlr 
exp : L_PAREN exp R_PAREN
   | lVal
   | number
   | IDENT L_PAREN funcRParams? R_PAREN
   | unaryOp exp
   | exp (MUL | DIV | MOD) exp
   | exp (PLUS | MINUS) exp

cond
   : exp
   | cond (LT | GT | LE | GE) cond
   | cond (EQ | NEQ) cond
   | cond AND cond
   | cond OR cond
   ;
```

## 输出语法树格式
对于输入
```cpp
int main() {
    int a=0;
    return a;
}
```
产生输出
```c
Program
  CompUnit
    FuncDef
      FuncType
        int INT[orange]
      main IDENT[red]
      Block
        BlockItem
          Decl
            VarDecl
              BType
                int INT[orange]
              VarDef
                a IDENT[red]
                = ASSIGN[blue]
                InitVal
                  Exp
                    Number
                      0 INTEGR_CONST[green]
        BlockItem
          Stmt
            return RETURN[orange]
            Exp
              LVal
                a IDENT[red]
```
解释：每个语法节点或词法节点占用一行，在语法树每向下一层缩进两个空格，部分终结符要求输出其终结符的名称以及颜色
- 关于颜色，要求如下
    - 保留字为 orange
    - 运算符为 blue
    - 数字与字符串为 green
    - 标识符为 red
```java
//保留字
CONST[orange],
INT[orange], 
VOID[orange],
IF[orange], 
ELSE[orange], 
WHILE[orange], 
BREAK[orange], 
CONTINUE[orange], 
RETURN[orange],
//运算符
PLUS[blue], 
MINUS[blue], 
MUL[blue], 
DIV[blue], 
MOD[blue], 
ASSIGN[blue], 
EQ[blue], 
NEQ[blue], 
LT[blue], 
GT[blue],
LE[blue], 
GE[blue], 
NOT[blue], 
AND[blue], 
OR[blue],
//标识符 
IDENT[red], 
//数字与字符串
INTEGR_CONST[green],
STRING[green],
```