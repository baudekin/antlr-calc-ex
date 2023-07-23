# antlr-calc-ex
How to use antlr to create go.
## Setup Project
1. Create app/example1 dir
2. Create app/example2 dir
3. ```go mod init github.com/baudekin/antlr-calc-ex```
4. ```cd parser```
5. Create the grammer: Calc.g4

```
// Calc.g4
grammar Calc;

// Tokens
MUL: '*';
DIV: '/';
ADD: '+';
SUB: '-';
NUMBER: [0-9]+;
WHITESPACE: [ \r\n\t]+ -> skip;

// Rules
start : expression EOF;

expression
   : expression op=('*'|'/') expression # MulDiv
   | expression op=('+'|'-') expression # AddSub
   | NUMBER                             # Number
   ;
```


## Generate Antlr Parser
1. Get jar: ```wget http://www.antlr.org/download/antlr-4.13.0-complete.jar```
2. Generte go parser files: ```java -jar $PWD/antlr-4.13.0-complete.jar -Dlanguage=Go -no-visitor -package parsing *.g4```
3. Fix import statements: ```find . -type f \\n    -name '*.go' \\n    -exec sed -i -e 's,github.com/antlr/antlr4/runtime/Go/antlr,github.com/antlr4-go/antlr/v4,g' {} \;```
4. Remove the -e files: ```rm *.*-e```

## Run Examples
1. Create example1.go in example1 to see lex commands in action run it. You should see:

```
NUMBER ("1")
ADD ("+")
NUMBER ("2")
MUL ("*")
NUMBER ("3")
```

2. Run exampl2.go to parse the input and calculate the value of 7. Remeber to use ```antlr.ParseTreeWalkerDefault.Walk(&listener, p.Start_())```
instead of ```antlr.ParseTreeWalkerDefault.Walk(&listener, p.Start())``` that is show in most examples.
2.1 Make sure to create a compatable struct with calc_base_listerner.go and implement the interface methods you need.
