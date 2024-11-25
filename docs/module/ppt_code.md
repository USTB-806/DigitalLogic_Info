# 课程 PPT 中出现的代码及其注释

> Verilog 基础部分代码注释作者为 **黄灿** 与 **潘涵**。

## Verilog 基础

### P8

```verilog
module Add_half (sum, c_out, a, b);//一位半加器模块
    input   a, b;//输入，两个一位操作数
    output  sum, c_out;//输出，sum:一位的求和计算结果，c_out:进位的结果
    wire    c_out_bar;//中间线路

    xor  (sum, a, b);//异或门，a,b为两输入，sum为输出，即a+b的最低一位
    nand (c_out_bar, a, b);//与非门，a,b为两输入，c_out_bar为输出，即a+b的第二位取反，如果没有则为1
    not  (c_out, c_out_bar);//非门，c_out_bar为输入，c_out为输出
endmodule
```

### P18

```verilog
module multiplexer (x1, x2, s, f);//多路选择器
    input   x1, x2, s;//输入，x1,x2为代选择数据，s为选择开关(switch)
    output  f;//输出
    wire    g, h, k;//中间线路

    not (k, s);//非门，s为输入，k为输出
    and (g, k, x1);//与门，k,x1为输入，g为输出，即~s&x1
    and (h, s, x2);//与门，s,x2为输入，h为输出，即s&x2
    or  (f, g, h);//或门，g,h为输入，f为输出，即(~s&x1)|(s&x2)
endmodule
```

### P19

```verilog
module Add_half ( sum, c_out, a, b );//一位半加器模块
    input  a, b;//输入，两个一位操作数
    output sum, c_out;//输出，sum:一位的求和计算结果，c_out:进位的结果

    assign sum = a ^ b;//assign持续赋值，sum为a异或b的结果
    assign c_out = a & b;//c_out为a按位与上b的结果

endmodule
```

### P21

```verilog
module multiplexer ( x1, x2, s, f);//多路选择器
    input        x1, x2, s;//输入，x1,x2为代选择数据，s为选择开关(switch)
    output wire  f;//输出

    assign f = (~ s & x1) | (s & x2);//s为0则f=x1，s为1则f=x2

endmodule
```

### P22

```verilog
module Add_half ( sum, c_out, a, b );//一位半加器
    input   a, b;//输入，两个一位操作数
    output  sum, c_out;//输出，sum:一位的求和计算结果，c_out:进位的结果

    assign    {c_out, sum} = a+b;//{c_out,sum}将两个一位宽信号拼接成一个两位宽信号，使用a+b的结果对其进行赋值(如果a+b仍是一位，则Verilog自动进行位宽转换，对于无符号数，高位补0)
     
endmodule
```

### P23

```verilog
module multiplexer ( x1, x2, s, f);//多路选择器
    input    x1, x2, s;//输入，x1,x2为代选择数据，s为选择开关(switch)
    output  f;//输出，不表明类型的情况下默认是wire
     reg f;//将f声明为reg类型，这是同一个f，用于在always中赋值

    always @(x1 or x2 or s) //或者@(*)，通配符*表示所有信号
        if (s == 0)//条件判断，实现选择
             f = x1;//选择x1输出
         else
             f = x2;//选择x2输出
endmodule
```

### P26

```verilog
module multiplexer ( x1, x2, s, f);//多路选择器
    input    x1, x2, s;//输入，x1,x2为代选择数据，s为选择开关(switch)
    output  f;//输出，不表明类型的情况下默认是wire
     reg f;//将f声明为reg类型，这是同一个f，用于在always中赋值

     always @(x1 or x2 or s)//或者@(*)，通配符*表示所有信号 
    begin
         if (s == 0)//条件判断，实现选择
             f = x1;//选择x1输出
         else
             f = x2;//选择x2输出
    end
endmodule
```

### P27

```verilog
module Add_half ( sum, c_out, a, b );//一位半加器
    input   a, b;//输入，两个一位操作数
    output  sum, c_out;//输出，sum:一位的求和计算结果，c_out:进位的结果，默认是wire类型
    reg     sum, c_out;//将上面的sum和c_out声明为reg类型，用于在always中赋值
    
    always @ ( a or b ) 
    begin
        if ({a,b}==0) begin c_out=0;sum=0; end//a=0,b=0的情况
        else if ({a,b}==1) begin c_out =0; sum =1; end//a=0,b=1的情况
        else if ({a,b}==2) begin c_out =0; sum =1; end//a=1,b=0的情况
        else begin c_out =1; sum =0; end//其他情况
     end
endmodule
```

### P28

```verilog
module Add_half ( sum, c_out, a, b );//一位半加器
    input   a, b;//输入，两个一位操作数
    output  sum, c_out;//输出，sum:一位的求和计算结果，c_out:进位的结果，默认是wire类型
    reg     sum, c_out;//将上面的sum和c_out声明为reg类型，用于在always中赋值
    always @ ( a or b ) begin
        sum = a ^ b;    //sum为a异或b的结果
        c_out = a & b;  //c_out为a按位与上b的结果
    end
endmodule
```

### P29

```verilog
module Add_half ( sum, c_out, a, b );//一位半加器
    input   a, b;//输入，两个一位操作数
    output  sum, c_out;//输出，sum:一位的求和计算结果，c_out:进位的结果，默认是wire类型
     reg    sum, c_out;//将上面的sum和c_out声明为reg类型，用于在always中赋值
    
     always @ ( a or b ) begin
    {c_out, sum} = a+b;//{c_out,sum}将两个一位宽信号拼接成一个两位宽信号，使用a+b的结果对其进行赋值(如果a+b仍是一位，则Verilog自动进行位宽转换，对于无符号数，高位补0)
      end
endmodule
```

### P32

```verilog
module adder(a,b,s1,s0);//一位半加器
    input a, b;//输入，两个一位操作数
    output s1, s0;//输出，s1:一位的求和计算结果，s0:进位的结果，默认是wire类型

    assign s1 = a & b;//assign持续赋值，s1为a异或b的结果
    assign s0 = a ^ b;//s0为a按位与上b的结果
endmodule
```

### P33

```verilog
module display(s1,s0,a,b,c,d,e,f,g);//七段数码管显示数字模块，只实现十进制数字0,1,2的显示
    input s1, s0;//输入，两个一位信号代表一个两位二进制数
    output a, b, c, d, e, f, g;//数码管段选
 //  -- a --
 //  |     |
 //f |     | b
 //  |     |
 //  -- g --
 //  |     |
 //e |     | c
 //  |     |
 //  -- d --
    assign a = ~s0;
    assign b = 1;
    assign c = ~s1;
    assign d = ~s0;
    assign e = ~s0;
    assign f  =~s1 & ~s0;
    assign g = s1 & ~s0;
    //以上逻辑实现{s1,s0}=2'b00时显示0，{s1,s0}=2'b01时显示1，{s1,s0}=2'b10时显示2
endmodule
```

### P34

```verilog
module display(s1,s0,a,b,c,d,e,f,g);//七段数码管显示数字模块，只实现十进制数字0,1,2的显示
    input s1, s0;//输入，两个一位信号代表一个两位二进制数
    output a, b, c, d, e, f, g;//数码管段选
    reg a, b, c, d, e, f, g;//将变量声明为reg类型，用于在always中赋值
 //  -- a --
 //  |     |
 //f |     | b
 //  |     |
 //  -- g --
 //  |     |
 //e |     | c
 //  |     |
 //  -- d --
    always@(s1, s0)
        if ({s1,s0}==0)//{s1,s0}=2'b00时显示0
    begin
        a=1; b=1;c=1;d=1;e=1;f=1;g=1;
    end
    else if({s1,s0}==1)//{s1,s0}=2'b01时显示1
    ...
endmodule
```

### P35

```verilog
module adder_display(x,y,a,b,c,d,e,f,g);//将一位半加器的结果在数码管上显示
    input x, y;//输入，两个一位加数
    output a,b,c,d,e,f,g;//数码管段选

    adder U1(x,y,w1,w0);//实例化一位半加器
    display U2(w1, w0, a,b,c,d,e,f,g);//实例化七段数码管显示数字模块，对一位半加器的输出进行显示
endmodule
```

### P56

```verilog
`timescale 10ns/1ns//时钟单位为10ns，时间精度为1ns
module top;
 // Data type declaration
    reg x, y;//声明为reg类型用于在initial中赋值
    wire a,b,c,d,e,f,g;////数码管段选
 // instance
    adder_display ad1 (x,y,a,b,c,d,e,f,g);//实例化半加器结果显示模块
 // Apply stimulus
    initial  begin
          x = 0;    y=0;//仿真开始时赋予初值
          #10 x = 0;   y=1;//10个时钟单位后赋值x=0,y=1,半加器的操作数改变，结果显示也随之改变
          #10 x = 1;   y=0;//再过10个时钟单位后赋值（从仿真开始到此时已经过去了20个时钟单位）
          #10 x = 1;   y=1;//再过10个时钟单位后赋值
          #50 $finish;//再过50个时钟单位后结束仿真
    end
endmodule
```

### P59

```verilog
`timescale 10ns/1ns //时间单位/时间精度
module top; //顶层模块
 // Data type declaration
    reg x;  //寄存器型变量 x
initial  begin  //initial 块，从0时刻开始执行
          x = 0;    //x初始为0
end
always begin //无敏感列表always块
          #5 x=~x; //持续执行对x的反转操作，时间间隔为5时间单位。
end
endmodule
```

### P73

```verilog
module test(out2,out1,in); //顶层模块，包含三个参数out2，out1和in
  output out2,out1; //out2和out1为输出
  input in; //in为输入
  … //定义的其他变量
  wire a=b&c;  //declare and assign

  assign out2 =  ~ in ; //in取反连接到out2
  assign out1 =   sel? i1:i0; //三目运算符，sel为真（高电平）时将i1连接到out1，否则将i0连接到out1
endmodule
```

### P75

```verilog
module multiplexer ( x1, x2, s, f);//多路复用器
    input    x1, x2, s;//输入线网型变量 x1,x2,s
    output  reg     f;//输出寄存器型变量 f



     always @(x1 or x2 or s) //always块，敏感列表为x1、x2、s，当其中一个变量发生改变时执行always块中所有操作
         if (s == 0) //if分支，条件为s==0
             f = x1; //条件为真时执行阻塞赋值语句 f=x1
         else
             f = x2; //条件为假时执行阻塞赋值语句 f=x2
endmodule
```

```verilog
module multiplexer ( x1, x2, s, f);//多路复用器
    input    x1, x2, s;//输入线网型变量 x1,x2,s
    output  f; //输出线网型变量 f
    wire    g; //线网型中间变量 g



    assign g = ~ s & x1; //s取反和x1取与后物理连接g
    assign f = g | (s & x2); //s与x2再或上g之后物理连接f
endmodule
```

### P99

```verilog
`define not_delay   #1 //宏定义，编译时将代码中的字符串`not_delay替换成 #1
`define and_delay  #2 //宏定义，编译时将代码中的字符串`and_delay替换成 #2
`define or_delay     #1//宏定义，编译时将代码中的字符串`or_delay替换成 #1
module MUX2_1 (out, a, b, sel); //二选一多路选择器 out=(a&~sel)|(b&sel)
output out; //输出线网型变量 out
input a, b, sel;//输入线网型变量 a,b,sel
    not `not_delay   not1( sel_, sel); //实例化非门模块，延迟1时间单位传递到输出，sel_=~sel
    and `and_delay and1( a1, a, sel_); //实例化与门模块，延迟2时间单位传递到输出，a1=a&sel_
    and `and_delay and2( b1, b, sel); //实例化与门模块，延迟2时间单位传递到输出，b1=b&sel
    or   `or_delay    or1( out, a1, b1); //实例化或门模块，延迟1时间单位传递到输出，out=a1|b1
endmodule
```

### P101

```verilog
`timescale 1 ns / 10 ps//时间单位/时间精度
    // All time units are in multiples of 1 nanosecond
    module MUX2_1 (out, a, b, sel);//二选一多路选择器 out=(a&~sel)|(b&sel)
    output out;//输出线网型变量out
    input a, b, sel;//输入线网型变量 a,b,sel
        not #1 not1( sel_, sel);//实例化非门模块，延迟1时间单位传递到输出，sel_=~sel
        and #2 and1( a1, a, sel_);//实例化与门模块，延迟2时间单位传递到输出，a1=a&sel_
        and #2 and2( b1, b, sel);//实例化与门模块，延迟2时间单位传递到输出，b1=b&sel
        or #1 or1( out, a1, b1);//实例化或门模块，延迟1时间单位传递到输出，out=a1|b1
    endmodule
```

> 组合逻辑电路部分代码注释作者为 **潘涵** 与 **孙明炜**。

## 组合逻辑电路

### P17

```verilog
module mux4to1 (w0, w1, w2, w3, S, f);//四选一多路选择器
    input w0, w1, w2, w3; //输入线网型变量 w0,w1,w2,w3，将要选择的四个分支
    input [1:0] S; //输入二位线网型变量 S，用于决定输出为哪一个分支
    output f; //输出线网型变量f
    assign f = S[1] ? (S[0] ? w3 : w2) : (S[0] ? w1 : w0); //使用多个三目运算符实现S四种状态00、01、10、11依次对应到w0、w1、w2、w3
endmodule 
```

### P18

```verilog
module mux4to1 (W, S, f); //四选一多路选择器
    input [0:3] W;//输入四位线网型变量 W[0:3]，将要选择的四个分支
    input [1:0] S;//输入二位线网型变量 S，用于决定输出为哪一个分支
    output reg f;//输出寄存器型变量f（寄存器型变量可以在always块中被改变）
        
    always @(*) //always块，敏感列表自动检测，当其中一个变量发生改变时执行always块中所有操作
        if (S == 0) //使用if else 语句实现多分支
            f = W[0]; //S==0时阻塞赋值f为W[0]
        else if (S == 1) 
            f = W[1]; //S==1时阻塞赋值f为W[1]
        else if (S == 2)
            f = W[2]; //S==2时阻塞赋值f为W[2]
        else if (S == 3)
            f = W[3]; //S==3时阻塞赋值f为W[3]
endmodule
```

```verilog
module mux4to1 (W, S, f); //四选一多路选择器
    input [0:3] W; //输入四位线网型变量 W[0:3]，将要选择的四个分支
    input [1:0] S; //输入二位线网型变量 S，用于决定输出为哪一个分支
    output reg f; //输出寄存器型变量f（寄存器型变量可以在always块中被改变）
        
    always @(W, S) //always块，敏感列表为W和S，当其中一个变量发生改变时执行always块中所有操作
        if (S == 2'b00) //使用if else 语句实现多分支
            f = W[0]; //S==2'b00时阻塞赋值f为W[0]
        else if (S == 2'b01)
            f = W[1]; //S==2'b01时阻塞赋值f为W[1]
        else if (S == 2'b10)
            f = W[2]; //S==2'b10时阻塞赋值f为W[2]
        else if (S == 2'b11)
            f = W[3]; //S==2'b11时阻塞赋值f为W[3]
endmodule
```

### P19

```verilog
module mux4to1 (W, S, f); //四选一多路选择器
    input [0:3] W; //输入四位线网型变量 W[0:3]，将要选择的四个分支
    input [1:0] S; //输入二位线网型变量 S，用于决定输出为哪一个分支
    output reg f; //输出寄存器型变量f（寄存器型变量可以在always块中被改变）
        
    always @(W, S) //always块，敏感列表为W和S，当其中一个变量发生改变时执行always块中所有操作
        case (S) //使用case语句实现多分支
            0: f = W[0]; //S==0时阻塞赋值f为W[0]
            1: f = W[1]; //S==1时阻塞赋值f为W[1]
            2: f = W[2]; //S==2时阻塞赋值f为W[2]
            3: f = W[3]; //S==3时阻塞赋值f为W[3]
            default: f = 1’b0; //default语句与实际效果无关，用于防止生成额外的电路，从而产生错误时序或降低效率。
        endcase //结束case语句
endmodule  
```

### P21

```verilog
module mux4to1 (w0, w1, w2, w3, S, f); //八位四选一多路选择器
    input [7:0] w0, w1, w2, w3; //输入四个八位线网型变量 w0,w1,w2,w3，将要选择的四个分支
    input [1:0] S; //输入二位线网型变量 S，用于决定输出为哪一个分支
    output reg[7:0] f; //输出八位寄存器型变量f（寄存器型变量可以在always块中被改变）
        
    always @(*) //always块，敏感列表自动检测，当其中一个变量发生改变时执行always块中所有操作
        case (S)//使用case语句实现多分支
            0: f = w0; //S==0时阻塞赋值f为W[0]
            1: f = w1; //S==1时阻塞赋值f为W[1]
            2: f = w2; //S==2时阻塞赋值f为W[2]
            3: f = w3; //S==3时阻塞赋值f为W[3]
            default: f = 8’b0; //default语句与实际效果无关，用于防止生成额外的电路，从而产生错误时序或降低效率。
        endcase //结束case语句
endmodule  
```

### P23

```verilog
module mux16to1 (W, S, f); //十六选一多路选择器
    input [0:15] W; //输入一个十六位线网型变量 W，将要选择的十六个分支
    input [3:0] S; //输入四位线网型变量 S，用于决定输出为哪一个分支
    output f; //输出线网型变量f
    wire [0:3] M; //线网型四位中间变量M
 
    mux4to1 Mux1 (W[0:3], S[1:0], M[0]); //按照S较低两位取出W[0:3]分段中对应的一个分支，存入M[0]
    mux4to1 Mux2 (W[4:7], S[1:0], M[1]); //按照S较低两位取出W[4:7]分段中对应的一个分支，存入M[1]
    mux4to1 Mux3 (W[8:11], S[1:0], M[2]); //按照S较低两位取出W[8:11]分段中对应的一个分支，存入M[2]
    mux4to1 Mux4 (W[12:15], S[1:0], M[3]); //按照S较低两位取出W[12:15]分段中对应的一个分支，存入M[3]
    mux4to1 Mux5 (M[0:3], S[3:2], f);   //按照S较高两位选出对应的W四个分段中的一个，即M[0:3]中对应的一个分支 
endmodule 
```

### P31

```verilog
module dec2to4 (W,  Y); //二线-四线译码器（decoder）
    input [1:0] W; //输入一个二位线网型变量W
    output reg [0:3] Y; //输出一个四位寄存器型变量Y
    always @(W) //always块，敏感列表为W，当其中一个变量发生改变时执行always块中所有操作
        case (W) //使用case语句实现多分支
            2'b00: Y = 4'b1000; //W==2'b00阻塞赋值Y为4'b1000
            2'b01: Y = 4'b0100; //W==2'b01阻塞赋值Y为4'b0100
            2'b10: Y = 4'b0010; //W==2'b10阻塞赋值Y为4'b0010
            2'b11: Y = 4'b0001; //W==2'b11阻塞赋值Y为4'b0001
            default: Y = 4'b0000; //default语句与实际效果无关，用于防止生成额外的电路，从而产生错误时序或降低效率。
        endcase //结束case语句
endmodule
```

### P33

```verilog
module dec2to4 (W, En, Y); //二线-四线译码器（decoder）（带有使能（En））
    input [1:0] W; //输入一个二位线网型变量W
       input En; //输入一个一位线网型变量En，表示使能
    output reg [0:3] Y;  //输出一个四位寄存器型变量Y
    always @(W, En) //always块，敏感列表为W，当其中一个变量发生改变时执行always块中所有操作
        case ({En,W}) //使用case语句实现多分支，其中大括号即为拼接两个二进制数
            3’b100: Y = 4'b1000; //En==1且W==2'b00阻塞赋值Y为4'b1000
            3’b101: Y = 4'b0100; //En==1且W==2'b01阻塞赋值Y为4'b0100
            3’b110: Y = 4'b0010; //En==1且W==2'b10阻塞赋值Y为4'b0010
            3’b111: Y = 4'b0001; //En==1且W==2'b11阻塞赋值Y为4'b0001
            default: Y = 4'b0000; //En==0时Y阻塞赋值为4'b0000
        endcase //结束case语句
endmodule
```

### P34

```verilog
module dec2to4 (W, En, Y);  //带使能端2-4译码器，模块定义为dec2to4，包含三个端口：W,En,Y
    input [1:0] W;          //输入端口W，是一个2位宽的向量
    input En;               //输入端口En，使能端，是一个单位宽的标量
    output reg [0:3] Y;     //输出端口Y，是一个4位宽reg类型的向量
        
    always @(W, En)         //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
    begin
        if (En == 0)        //if语句实现选择器，当En==0时，执行相应语句
            Y = 4'b0000;    //此时Y赋值为0
        else                //else语句，当En!=0时，执行相应语句
            case (W)        //case语句同样实现选择器，根据W的值对Y进行赋值
                0: Y = 4'b1000;         //当W==0时，Y赋值为二进制数1000
                1: Y = 4'b0100;         //当W==1时，Y赋值为二进制数0100
                2: Y = 4'b0010;         //当W==2时，Y赋值为二进制数0010
                default: Y = 4'b0001;   //对于W的其他值，Y赋值为二进制数0001
            endcase         //case的结束语句
    end
endmodule  
```

### P35

```verilog
module dec4to16 (W, En, Y);  //结构化描述实现4-16译码器，模块定义为dec4to16，包含三个端口：W,En,Y
    input [3:0] W;           //输入端口W，是一个4位宽的向量
    input En;                //输入端口En，使能端，是一个单位宽的标量
    output [0:15] Y;         //输出端口Y，是一个16位宽的向量
    wire [0:3] M;            //内部连线M，是一个4位宽的向量
    
    dec2to4 Dec1 (W[3:2], En, M[0:3]);      //实例化第一个dec2to4模块Dec1，输入W的高两位与En，输出M
    dec2to4 Dec2 (W[1:0], M[0],Y[0:3] );    //实例化第二个dec2to4模块Dec2，输入W的低两位与M[0]，输出Y[0:3]
    dec2to4 Dec3 (W[1:0], M[1],Y[4:7] );    //实例化第三个dec2to4模块Dec3，输入W的低两位与M[1]，输出Y[4:7]
    dec2to4 Dec4 (W[1:0], M[2], Y[8:11]);   //实例化第四个dec2to4模块Dec4，输入W的低两位与M[2]，输出Y[8:11]
    dec2to4 Dec5 (W[1:0], M[3],Y[12:15] );  //实例化第五个dec2to4模块Dec5，输入W的低两位与M[3]，输出Y[12:15]
endmodule
```

### P36

```verilog
module seg7 (hex, leds);    //代码转换器（显示译码器）：用七段数码管输出数字和字母，模块定义为seg7，包含两个端口：hex,leds

    input [3:0] hex;        //输入端口hex，是一个4位宽的向量
    output reg [1:7] leds;  //输出端口leds，是一个7位宽的向量

    always@(hex)            //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
        case(hex) // abcdefg       //case语句，根据hex的值对leds进行赋值
            0: leds = 7'b1111110;  //当hex为0时，对leds赋值，使数码管上显示数字0
            1: leds = 7'b0110000;  //当hex为1时，对leds赋值，使数码管上显示数字1
            2: leds = 7'b1101101;  //当hex为2时，对leds赋值，使数码管上显示数字2
            3: leds = 7'b1111001;  //当hex为3时，对leds赋值，使数码管上显示数字3
            4: leds = 7'b0110011;  //当hex为4时，对leds赋值，使数码管上显示数字4
            5: leds = 7'b1011011;  //当hex为5时，对leds赋值，使数码管上显示数字5
            6: leds = 7'b1011111;  //当hex为6时，对leds赋值，使数码管上显示数字6
            7: leds = 7'b1110000;  //当hex为7时，对leds赋值，使数码管上显示数字7
            8: leds = 7'b1111111;  //当hex为8时，对leds赋值，使数码管上显示数字8
            9: leds = 7'b1111011;  //当hex为9时，对leds赋值，使数码管上显示数字9
            10: leds = 7'b1110111; //当hex为10时，对leds赋值，使数码管上显示字母A
            11: leds = 7'b0011111; //当hex为11时，对leds赋值，使数码管上显示字母B
            12: leds = 7'b1001110; //当hex为12时，对leds赋值，使数码管上显示字母C
            13: leds = 7'b0111101; //当hex为13时，对leds赋值，使数码管上显示字母D
            14: leds = 7'b1001111; //当hex为14时，对leds赋值，使数码管上显示字母E
            15: leds = 7'b1000111; //当hex为15时，对leds赋值，使数码管上显示字母F
    endcase
endmodule
```

### P37

```verilog     //编码器
always @(W)    //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
    case (W)   //case语句，根据W的值对Y进行赋值
        4'b0001: Y = 2'b00;  //当W的值为二进制数0001时，Y赋值为二进制数00
        4'b0010: Y = 2'b01;  //当W的值为二进制数0010时，Y赋值为二进制数01
        4'b0100: Y = 2'b10;  //当W的值为二进制数0100时，Y赋值为二进制数10
        4'b1000: Y = 2'b11;  //当W的值为二进制数1000时，Y赋值为二进制数11
        default: Y = 2'b00;  //当W的值为其他数时，Y赋值为二进制数00
    endcase
```

### P38

```verilog     //优先级编码器，根据W中最高位的1的位置对Y进行赋值
always @ (W)   //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
begin
    if (W[3]) Y=2'b11;        //若最高位的1位于W[3]，则Y赋值为二进制数11
    else if (W[2]) Y=2'b10;   //若最高位的1位于W[2]，则Y赋值为二进制数10
    else if (W[1]) Y=2'b01;   //若最高位的1位于W[1]，则Y赋值为二进制数01
    else Y=2'b00;             //若最高位的1位于W[0]或W为0，则Y赋值为二进制数00
end
assign z=W[3]||W[2]||W[1]||W[0]   //使用逻辑或操作符||，如果W的任何一位为1，则z为1；否则，z为0。
```

### P41

```verilog
module alu(F, S, A, B); // 74381 ALU  //算术逻辑单元，模块定义为alu，包含四个端口：F,S,A,B
    output reg [3:0] F; //输出端口F，是一个4位宽reg类型的向量
    input [2:0] S;      //输入端口S，表示需要进行的运算
    input [3:0] A, B;   //输入端口A和B，是进行运算的数值
    always @(S, A, B)   //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
        case (S)        //case语句，根据S的值，判断对A和B进行的运算，从而对F进行赋值
            0: F = 4'b0000;   //当S的值为二进制数0时，Y赋值为0
            1: F = B - A;     //当S的值为二进制数1时，Y赋值为B-A
            2: F = A - B;     //当S的值为二进制数2时，Y赋值为A-B
            3: F = A + B;     //当S的值为二进制数3时，Y赋值为A+B
            4: F = A ^ B;     //当S的值为二进制数4时，Y赋值为A^B（按位异或）
            5: F = A | B;     //当S的值为二进制数5时，Y赋值为A|B（按位或）
            6: F = A & B;     //当S的值为二进制数6时，Y赋值为A&B（按位与）
            default: F = 4'b1111;    //当S的值为其他值时，Y赋值为二进制数1111
        endcase
endmodule
```

### P42

```verilog
module  M(W,  Y);          //译码器，模块定义为M，包含两个端口：W,Y
    input [1:0] W;         //输入端口W，是一个2位宽的向量
    output reg [0:3] Y;    //输出端口Y，是一个4位宽的向量
        
    always @(*)    //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
        case (W)   //case语句，根据W的值对Y进行赋值
            2'b00: Y = 4'b1000;    //当W的值为二进制数00时，Y赋值为二进制数1000
            2'b01: Y = 4'b0100;    //当W的值为二进制数01时，Y赋值为二进制数0100
            2'b10: Y = 4'b0010;    //当W的值为二进制数10时，Y赋值为二进制数0010
            2'b11: Y = 4'b0001;    //当W的值为二进制数11时，Y赋值为二进制数0001
            default: Y = 4'b0000;  //当W的值为其他数时，Y赋值为0
        endcase
endmodule
```

### P44

```verilog
module  FullAdder (A, B, Ci, S, Co);  //一位全加器，模块定义为FullAdder，包含五个端口：A,B,Ci,S,Co
input     A, B, Ci;   //输入端口A、B、Ci，A和B为全加器的两个加数，Ci是进位
output   S, Co;       //输出端口S和Co，S为和的输出，Co为进位输出

assign   S  = A ^ B ^ Ci;   //assign赋值语句计算并输出S，S为A,B,Ci三者异或的结果，代表不考虑进位时的和
assign   Co = (A & B) | (A & Ci) | (B & Ci);   //assign赋值语句计算并输出Co，代表由A,B,Ci产生的进位（A,B,Ci中任意两个为1便产生进位）
endmodule 
```

### P49

```verilog
module ADDER4BIT ( Ai, Bi, S, OVF);  //用一位全加器组成四位全加器，模块定义为ADDER4BIT，包含四个端口：Ai,Bi,S,OVF
input       [3:0] Ai, Bi;   //输入端口A和B，A和B为四位全加器的两个加数，是4位宽的向量
output     [3:0] S;         //输出端口S，为各位和的输出
output     OVF;             //输出端口OVF，为进位输出
wire        [2:0] CY;       //内部连线CY，储存加数相加时产生的进位
FullAdder   U0 (Ai[0], Bi[0], 0, S[0], CY[0]);          //实例化FullAdder模块，将Ai[0]和Bi[0]相加，此时无进位，和赋值给S[0]，进位赋值给CY[0]
    FullAdder   U1 (Ai[1], Bi[1], CY[0], S[1], CY[1]);  //实例化FullAdder模块，将Ai[1]和Bi[1]相加，进位为CY[0]，和赋值给S[1]，进位赋值给CY[1]
    FullAdder   U2 (Ai[2], Bi[2], CY[1], S[2], CY[2]);  //实例化FullAdder模块，将Ai[2]和Bi[2]相加，进位为CY[1]，和赋值给S[2]，进位赋值给CY[2]
    FullAdder   U3 (Ai[3], Bi[3], CY[2], S[3], OVF);    //实例化FullAdder模块，将Ai[3]和Bi[3]相加，进位为CY[2]，和赋值给S[3]，进位赋值给OVF
endmodule
```

## 触发器寄存器计数器

> 触发器寄存器计数器部分代码注释作者为 **孙明炜**、**冯子贻**、**夏榆峰** 与 **刘春旭**。

### P14

```verilog
module SR_latch(R, S, Q, Q_bar);  //SR锁存器，模块定义为FullAdder，包含五个端口：A,B,Ci,S,Co
    input R,S;             //输入端口R和S，R为复位端，S为置位端
    output wire Q,Q_bar;   //输出端口Q和Q_bar，Q为初态，Q_bar为次态
    nor n1(Q, R, Q_bar);   //或非门n1，以R、Q_bar作为输入，并输出到Q（此处或非门的实例化中第一个为输出）
    nor n2(Q_bar, S, Q);   //或非门n2，以S、Q作为输入，并输出到Q_bar
endmodule
```

### P22

```verilog
module D_latch (D, Clk, Q);  //D锁存器，模块定义为D_latch，包含三个端口：D，Clk，Q
    input D, Clk;     //输入端口D和Clk，D为输入的内容，Clk为时钟
    output reg Q;     //输出端口Q，为reg类型
    
    always @(D, Clk)  //always块，用来描述时序逻辑，与assign连续赋值实现的功能一致
        if (Clk)      //if语句，若Clk为高电平，执行相应语句
            Q <= D;   //非阻塞赋值，将D的值赋给Q
endmodule             //没有else，表示敏感事件不发生时，状态保持
```

### P23

```verilog         //D锁存器
always @(D, Clk)   //always块，用来描述时序逻辑，与assign连续赋值实现的功能一致
    if (Clk)       //if语句，若Clk为高电平，执行相应语句
        Q <= D;    //非阻塞赋值，将D的值赋给Q
    else           //Clk为低电平时执行相应语句
        Q <= ~D;   //非阻塞赋值，将~D的值赋给Q
```

### P25

```verilog
always @(a or b or gate ) begin  //always块，用来描述组合逻辑，与assign连续赋值实现的功能一致
  if (gate)          //if语句，若gate为1，执行相应语句
     out = a | b;    //将out赋值为a|b
end
```

### P30

```verilog
module flipflop (D, Clock, Q);  //D触发器
    input D, Clock;             //输入端口D和Clock，D为输入的内容，Clock为时钟
    output reg Q;               //输出端口Q，为reg类型
    
    always @(posedge Clock)     //always块，posedge上升沿触发，negedge下降沿触发
        Q <= D;                 //非阻塞赋值，在Clk上升沿时将D的值赋给Q
endmodule
```

### P31

```verilog //简单的触发器（flip-flop）模块,在时钟信号的下降沿捕获输入D的值，并将其存储在输出Q中
module flipflop (D, Clock, Q);//定义flipflop模块，定义变量D，Clock，Q
    input D, Clock;//输入wire类型 D，Clock
    output reg Q;//输出寄存器类型Q
    
    always @(negedge Clock)//敏感信号下降沿触发Clock信号
        Q <= D; //将D赋值给Q
endmodule
```

### P33

```verilog       //一个带有使能端的寄存器模块,在使能信号E为高时，会在每个时钟上升沿将D的值同步到Q,如果E为低，Q的值将保持不变
module rege (D, Clock, E, Q);//定义rege模块，定义变量D，Clock，E，Q
    input D, Clock, E;//输入wire类型 D，Clock
    output reg Q;//输出寄存器类型Q
 
    always @(posedge Clock)//敏感信号上升沿触发信号Clock
        if (E)//if语句，如果E==1
            Q <= D;//实现赋值语句D赋给Q
endmodule
```

### P36

```verilog    //带有复位功能的D触发器模块,时钟信号的上升沿根据复位信号Resetn的状态来更新输出Q的值
module flipflop (D, Clock, Resetn, Q);//定义flipflop模块，定义变量D，Resetn，Q
    input D, Clock, Resetn;//输入wire类型变量D，Clock，Resetn
    output reg Q;//输出寄存器类型变量Q
    
    always @(posedge Clock)//敏感信号上升沿触发信号Clock
        if (!Resetn)//if语句，如果Restn==0
            Q <= 0;//实现赋值语句0赋给Q
        else //否则
            Q <= D;//实现赋值语句D赋给Q
    
endmodule 
```

### P37

```verilog      //带有复位功能的D触发器模块,在时钟信号的上升沿或者复位信号Resetn的下降沿更新输出Q的值
module flipflop (D, Clock, Resetn, Q);//定义flipflop模块，定义变量D，Resetn，Q
    input D, Clock, Resetn;//输入wire类型变量D，Clock，Resetn
    output reg Q;///输出寄存器类型变量Q
    
    always @(negedge Resetn or posedge Clock)//敏感信号下降沿信号Resetn和上升沿信号Clock
        if (!Resetn)//if语句，如果Restn==0
            Q <= 0;//实现赋值语句0赋给Q
        else //否则
            Q <= D;//实现赋值语句D赋给Q
    
endmodule
```

### P54

```verilog      //两级触发器链,在时钟信号的上升沿将输入D的值传递到Q1，然后将Q1的值传递到Q2
module example5_4 (D, Clock, Q1, Q2);//定义example5_4模块，定义变量D，Clock,Q1,Q2
    input D, Clock;//输入wire类型变量D，Clock
    output reg Q1, Q2;///输出寄存器类型变量Q1,Q2
    
    always @(posedge Clock)//敏感信号上升沿信号Clock
    begin
        Q1 <= D;//实现赋值语句D赋给Q1
        Q2 <= Q1;//实现赋值语句Q1赋给Q2
    end
    
endmodule
```

```verilog
always @(posedge Clock)//敏感信号上升沿信号Clock
begin
    Q2 <= Q1;//实现赋值语句Q1赋给Q2
    Q1 <= D;//实现赋值语句D赋给Q1
end

```

```verilog
always @(posedge Clock)//敏感信号上升沿信号Clock
    Q2 <= Q1;//实现赋值语句Q2赋给Q1
always @(posedge Clock)//敏感信号上升沿信号Clock
    Q1 <= D;//实现赋值语句Q1赋给D
```

### P55

```verilog
module example5_4 (D, Clock, Q1, Q2);//定义example5_4模块，定义变量D，Clock,Q1,Q2
    input D, Clock;//输入wire类型变量D，Clock
    output reg Q1, Q2;///输出寄存器类型变量Q1,Q2
    
    always @(posedge Clock)//敏感信号上升沿信号Clock
    begin
        Q1 = D;//实现赋值语句Q1赋给D
        Q2 = Q1;//实现赋值语句Q2赋给Q1
    end
    
endmodule

```

### P56

```verilog
module example5_4 (D, Clock, Q1, Q2);//定义example5_4模块，定义变量D，Clock,Q1,Q2
    input D, Clock;//输入wire类型变量D，Clock
    output reg Q1, Q2;///输出寄存器类型变量Q1,Q2
    
    always @(posedge Clock)//敏感信号上升沿信号Clock
    begin
        Q1 = D;//实现赋值语句Q1赋给D
        Q2 = Q1;//实现赋值语句Q2赋给Q1
    end
    
endmodule
```

### P57

```verilog
module example5_5 (x1, x2, x3, Clock, f, g);//定义example5_5模块，定义变量x1,x2,x3,Clock,f, g
    input x1, x2, x3, Clock;//输入wire类型变量x1,x2,x3,Clock
    output reg f, g;//输出寄存器类型变量f,g

    always @(posedge Clock)//敏感信号上升沿信号Clock同步逻辑
    begin//非阻塞赋值语句块，上升沿捕获输入，并在时钟周期结束时更新输出
        f <= x1 & x2;//在时钟的上升沿，x1 和 x2 的逻辑与（AND）结果将被赋值给 f
        g <= f | x3;//在时钟的上升沿，f 和 x3 的逻辑或（OR）结果将被赋值给 g
    end//在当前时钟周期的末尾执行
endmodule
```

### P58

```verilog
module example5_5 (x1, x2, x3, Clock, f, g);//定义example5_5模块，定义变量x1,x2,x3,Clock,f, g
    input x1, x2, x3, Clock;//输入wire类型变量x1,x2,x3,Clock
    output reg f, g;//输出寄存器类型变量f,g

    always @(posedge Clock)//敏感信号上升沿信号Clock同步逻辑
    begin//阻塞赋值语句块
        f = x1 & x2;//x1 和 x2 的逻辑与（AND）结果将被赋值给 f
        g = f | x3;//f 和 x3 的逻辑或（OR）结果将被赋值给 g
    end
endmodule
```

### P59

```verilog
always @(posedge Clock)//敏感信号上升沿信号Clock同步逻辑
begin
    f <= x1 & x2;//在时钟的上升沿，x1 和 x2 的逻辑与（AND）结果将被赋值给 f
    g <= x1 & x2 | x3;//在时钟信号Clock的上升沿，x1 和 x2 的逻辑与（AND）结果与 x3 的逻辑或（OR）结果将被赋值给 g
end//在当前时钟周期的末尾执行
```

```verilog
assign  f1 = x1 & x2; //逻辑与门（AND），它将 x1 和 x2 作为输入，并把结果连续赋值给 f1，只要 x1 或 x2 的值发生变化，f1 的值就会立即更新
assign  g1 = f1 | x3; //assign连续赋值，创建一个逻辑或门，它将 f1 和 x3 作为输入，并把结果连续赋值给 g1，只要 f1 或 x3 的值发生变化，g1 的值就会立即更新
always @(posedge Clock)//敏感信号上升沿信号Clock同步逻辑
begin
    f <= f1;//将f1赋值给f
    g <= g1;//将g1赋值给g
end//在当前时钟周期的末尾执行
```

### P61

```verilog
module test_1 (x1, x2, Clock, f);//定义test_1模块，定义变量x1,x2,x3,Clock,f
    input x1, x2, Clock;//输入wire类型变量x1,x2,Clock
    output reg f;//输出寄存器类型变量f
    always @(posedge Clock)//敏感信号上升沿信号Clock同步逻辑
    begin
        f <= x1 & x2;//在当前时钟周期结束时，x1 和 x2 的逻辑与结果将被赋值给 f
    end
endmodule
```

### P62

```verilog
module test_3 (x1, x2, Clock, f);//定义test_3模块，定义变量x1,x2,Clock,f
    input x1, x2, Clock;//输入wire类型变量x1,x2,Clock
    output reg f;//输出寄存器类型变量f
    always @(x1, x2, Clk)//敏感信号x1，x2,Clk
        if (Clk)//if语句，如果Clk==1
            f <= x1 & x2;//在当前时钟周期结束时，x1 和 x2 的逻辑与结果将被赋值给 f
    end
endmodule
```

### P63

```verilog
module test_2 (x1, x2, Clock, f);//定义test_2模块，定义变量x1,x2,Clock,f
    input x1, x2, Clock;//输入wire类型变量x1,x2,Clock
    output reg f;//输出寄存器类型变量f
    always @(x1, x2, Clk)//敏感信号x1，x2,Clk
        if (Clk)//if语句，如果Clk==1
            f = x1 & x2;//x1 和 x2 的逻辑与结果赋值给 f
        else//否则
           f = 0;  //f为0
    end
endmodule
```

### P67

```verilog
//定义模块regn，带有异步清零的n位寄存器
module regn (D, Clock, Resetn, Q);
    parameter n = 16;//parameter定义声明一个常量n=16
    input [n-1:0] D;//n位输入信号D
    input Clock, Resetn;//时钟信号Clock，复位信号Resetn
    output reg [n-1:0] Q;//n位输出寄存器Q

    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)// 如果Resetn为低电平（复位）
            Q <= 0;// 将输出Q置为0
        else 
            Q <= D;// 否则将输入D的值赋给输出Q
endmodule//模块结束
```

### P71

```verilog
//定义模块shift4，实现并行存取移位寄存器
module shift4 (R, L, w, Clock, Q);
    input [3:0] R;// 定义一个4位输入信号R
    input L, w, Clock;//时钟信号Clock，输入信号L、w
    output reg [3:0] Q;//n位输出寄存器Q
    always @(posedge Clock)// 在时钟信号Clock的上升沿触发
        if (L)// 如果L为高电平
            Q <= R;// 将输入R的值赋给输出Q
        else  begin// 否则
            Q[0] <= Q[1];// 将Q的第1位赋值给Q的第0位
            Q[1] <= Q[2];// 将Q的第2位赋值给Q的第1位
            Q[2] <= Q[3];// 将Q的第3位赋值给Q的第2位
            Q[3] <= w;// 将输入w的值赋给Q的第3位
        end 
endmodule//模块结束
```

### P72

```verilog
//定义模块shift4，实现并行存取移位寄存器
module shiftn (R, L, w, Clock, Q);
    parameter n = 4;//parameter定义声明一个常量n=4
    input [n-1:0] R;// 定义一个4位输入信号R
    input L, w, Clock;//时钟信号Clock，输入信号L、w
    output reg [n-1:0] Q;//n位输出寄存器Q
    integer k;// 定义一个整数变量k（虽然在代码中未使用）

    always @(posedge Clock) // 在时钟信号Clock的上升沿触发
        if (L) // 如果L为高电平
            Q <= R;// 将输入R的值赋给输出Q
        else// 否则
        begin
            Q[n-2:0] <= Q[n-1:1];// 将Q的第n-1位到第1位的值分别赋给Q的第n-2位到第0位，也可写作Q<=Q>>1
            Q[n-1] <= w;// 将输入w的值赋给Q的第n-1位
        end 
endmodule//模块结束
```

### P74

```verilog
//定义模块shift4，实现带有使能信号的并行存取移位寄存器
module shiftrne (R, L, E, w, Clock, Q);
    parameter n = 4;//parameter定义声明一个常量n=4
    input [n-1:0] R;// 定义一个4位输入信号R
    input L, E, w, Clock;//时钟信号Clock，输入信号L、w，使能信号E
    output reg [n-1:0] Q;//n位输出寄存器Q
    integer k;// 定义一个整数变量k（虽然在代码中未使用）
    always @(posedge Clock)// 在时钟信号Clock的上升沿触发
    begin
        if (L) // 如果L为高电平
            Q <= R;// 将输入R的值赋给输出Q
        else if (E)// 否则如果E为高电平
        begin
            Q[n-1] <= w;// 将输入w的值赋给Q的第n-1位
            Q[n-2:0] <= Q[n-1:1];// 将Q的第n-1位到第1位的值分别赋给Q的第n-2位到第0位
        end
    end
endmodule//模块结束
```

### P77

```verilog
//定义模块counter，实现具有异步复位的同步递增计数器
module counter (Resetn, Clock, E, Q);
    input Resetn, Clock, E;//时钟信号Clock，复位信号Resetn，使能信号E
    output reg [3:0] Q;// 定义一个4位寄存器输出信号Q
    
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)// 如果Resetn为低电平（复位）
            Q <= 0;// 将输出Q置为0
        else if (E)// 否则如果E为高电平
            Q <= Q + 1; // 将输出Q的值加1
            
endmodule//模块结束
```

### P78

```verilog
//定义模块counter，具有并行载入的同步递增计数器
module counter (R, Resetn, Clock, E, L, Q);
    input [3:0] R; // 定义一个4位输入信号R
    input Resetn, Clock, E, L;//时钟信号Clock，复位信号Resetn，使能信号E，输入信号L
    output reg [3:0] Q;// 定义一个4位宽的寄存器输出信号Q
    
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)// 如果Resetn为低电平（复位）
            Q <= 0;// 将输出Q置为0
        else if (L)// 否则如果L为高电平
            Q <= R;// 将输入R的值赋给输出Q
        else if (E)// 否则如果E为高电平
            Q <= Q + 1;// 将输出Q的值加1
endmodule//模块结束
```

### P79

```verilog
//定义模块counter，具有并行载入和计满标志的同步递增计数器
module counter (R, Resetn, Clock, E, L, Q, Rc);
    input [3:0] R;// 定义一个4位输入信号R
    input Resetn, Clock, E, L;//时钟信号Clock，复位信号Resetn，使能信号E，输入信号L
    output reg [3:0] Q;// 定义一个4位宽的寄存器输出信号Q
    output Rc;//声明寄存器信号Rc,即计满标志
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)    Q <=4’b0;// 如果Resetn为低电平（复位），将输出Q置为0
        else if (L) Q <= R;// 否则如果L为高电平，将输入R的值赋给输出Q
        else if (E) Q <= Q + 1;//否则如果E为高电平，将输出Q的值加1
    always @(Q)// 当Q的值发生变化时触发
        if (Q==4’d15)   Rc=1;// 如果Q等于15（4'b1111），将输出计满标志Rc置为1
        else    Rc=0;// 否则将输出计满标志Rc置为0
endmodule//模块结束
```

### P80

```verilog
//定义模块counter，具有并行载入和计满标志和片选的同步递增计数器
module counter (R, Resetn, Clock, CE, E, L, Q, Rc);
    input [3:0] R;// 定义一个4位输入信号R
    input Resetn, Clock, CE, E, L;//时钟信号Clock，复位信号Resetn，使能信号E，输入信号L，时钟功能信号CE
    output reg [3:0] Q;// 定义一个4位宽的寄存器输出信号Q
    output Rc;//声明寄存器信号Rc,即计满标志
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)    Q <=4’b0;// 如果Resetn为低电平（复位），将输出Q置为0
        else if (CE & L)    Q <= R;// 否则如果L、CE为高电平，将输入R的值赋给输出Q
        else if (CE & E)    Q <= Q + 1;//否则如果E、CE为高电平，将输出Q的值加1
    always @(*)// 当任何输入信号发生变化时触发
        if (CE && (Q==4’d15))   Rc=1;// 如果CE为高电平且Q等于15（4'b1111），将输出计满标志Rc置为1
        else    Rc=0;// 否则将输出计满标志Rc置为0
endmodule//模块结束
```

### P81

```verilog
//定义模块counter，具有并行载入和计满标志和片选的同步递增计数器(十进制)
module counter (R, Resetn, Clock, CE, E, L, Q, Rc);
    input [3:0] R;// 定义一个4位输入信号R
    input Resetn, Clock, CE, E, L;//时钟信号Clock，复位信号Resetn，使能信号E，输入信号L，时钟功能信号CE
    output reg [3:0] Q;// 定义一个4位宽的寄存器输出信号Q
    output Rc;//声明寄存器信号Rc,即计满标志
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)    Q <=4’b0;// 如果Resetn为低电平（复位），将输出Q置为0
        else if (L) Q <= R;// 否则如果L为高电平，将输入R的值赋给输出Q
        else if (CE && E && (Q==4’d9))  Q <= 4’d0;//否则如果E、CE为高电平且Q等于9，将输出Q置为0
        else if (CE & E)    Q <= Q + 1;//否则如果E、CE为高电平，将输出Q的值加1

    always @(*)// 当任何输入信号发生变化时触发
        if (CE && (Q==4’d9))    Rc= 4’d1;// 如果CE为高电平且Q等于9，将输出计满标志Rc置为1
        else    Rc=0;// 否则将输出计满标志Rc置为0
endmodule//模块结束
```

### P82

```verilog
//定义模块counter，具有并行载入和计满标志和片选的同步递增、递减计数器
module counter (R, Resetn, Clock, E, UP, L, Q, Rc);
    input [3:0] R;// 定义一个4位输入信号R
    input Resetn, Clock, E, L, UP;//时钟信号Clock，复位信号Resetn，使能信号E，输入信号L，增减信号标志UP
    output reg [3:0] Q;// 定义一个4位宽的寄存器输出信号Q
    output Rc;//声明寄存器信号Rc,即计满标志
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)    Q <=4’b0;// 如果Resetn为低电平（复位），将输出Q置为0
        else if (L) Q <= R;// 否则如果L为高电平，将输入R的值赋给输出Q
        else if (E && UP)   Q<=Q+1;//否则如果E、L为高电平，将输出Q的值加1（L为高电平表示递增）
        else if (E && !UP)  Q<=Q-1;//否则如果E为高电平、L为低电平，将输出Q的值减1（L为低电平表示递减）


    always @(*)// 当任何输入信号发生变化时触发
        if (UP && (Q==4’d15)) || (!UP && (Q==4’d0)) Rc=1;// L为高电平且Q等于15（递增最大值）或L为低电平且Q等于0（递减最小值），将输出计满标志Rc置为1
        else    Rc=0;// 否则将输出计满标志Rc置为0
endmodule//模块结束
```

### P83

```verilog
//定义模块counter,实现递增计数器
module counter (Resetn, Clock, Q);
    input Resetn, Clock;//时钟信号Clock，复位信号Resetn
    output reg Q;// 定义一个寄存器输出信号Q
    
    always @(negedge Resetn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
        if (!Resetn)// 如果Resetn为低电平（复位）
            Q <= 0;//将输出Q置为0
        else
            Q <= Q + 1;// 否则输出Q加1
endmodule//模块结束
```

### P84

```verilog
//定义模块counter,实现递增计数器
module counter (Rstn, Clock, Q);
    input Rstn, Clock;//时钟信号Clock，复位信号Resetn
    output reg [1:0] Q;// 定义一个2位寄存器输出信号Q
always @(negedge Rstn or posedge Clock)// 在Resetn的下降沿或Clock的上升沿触发
    if (!Rstn)// 如果Resetn为低电平（复位）
                Q <= 0;//将输出Q置为0
    else
        Q <= Q + 1;// 否则输出Q加1

endmodule//模块结束
```

### P85

```verilog
module clk_div_phase ( //时钟分频模块，输入为时钟信号Clock和复位信号Reset，输出为c0~c9
Clock, Reset, c0, c1, c2, c3,c4, c5, c6, c7, c8, c9);  
input Clock, Reset;  
output c0, c1, c2, c3,c4, c5, c6, c7, c8, c9 ;
wire c0, c1, c2, c3,c4, c5, c6, c7, c8, c9 ; //声明输出为wire型
reg [9:0] cnt; //声明一个10位宽寄存器cnt
  always @ (posedge Clock) //Clock升沿，执行always模块内的操作
    if (Reset) //Reset复位信号高电平，将计数器cnt清零
       cnt<=10'b0000000000;
    else       cnt <= cnt + 1; //Reset复位信号低电平，计数器cnt加1
  // 将cnt的每一位进行取反操作，并赋值给相应的输出信号
  // 这样做可以生成具有不同相位的时钟信号
  assign c0 = ~cnt [0];  assign c1 = ~cnt [1];  assign c2 = ~cnt [2];
  assign c3 = ~cnt [3];  assign c4 = ~cnt [4];  assign c5 = ~cnt [5];
  assign c6 = ~cnt [6];  assign c7 = ~cnt [7];  assign c8 = ~cnt [8];
  assign c9 = ~cnt [9];
  //c0~c9是Clock的不同分频的结果，例如c0每计数1翻转一次，c9计数到1023才会翻转一次
endmodule 
```

### P123

```verilog

module BCDcount (Clock, Clear, E, BCD1, BCD0); //双位二进制编码的十进制计数器模块
    input Clock, Clear, E; //时钟信号、清零、使能
    output reg [3:0] BCD1, BCD0; //BCD0为输出的个位，BCD1为输出的十位
    
    always @(posedge Clock) //Clock上升沿执行always模块内的操作
    begin
        if (Clear) //若Clear为高电平，则将两个BCD码清零
        begin
            BCD1 <= 0; 
            BCD0 <= 0;
       end
       else if (E) //使能E为高电平时，进行计数
            if (BCD0 == 4'b1001) //BCD0为9，由于进位清零
            begin
            BCD0 <= 0;
            if (BCD1 == 4'b1001) //BCD1为9，由于进位清零
                 BCD1 <= 0;
            else //否则BCD1加1
                BCD1 <= BCD1 + 1;
            end
            else //BCD0不为9，加1
                BCD0 <= BCD0 + 1;
    end
endmodule
```

### P124

```verilog
module seg7 (bcd, leds); //7位数码管显示模块

    input [3:0] bcd; // 输入：一个4位的BCD码，表示十进制数字0-9
    output reg [1:7] leds; // 输出：一个7位的LED控制信号，用于控制七段数码管的点亮情况

    always@(bcd)
        case(bcd) // abcdefg，bcd为0~9时，分别在数码管上显示数字0~9
            0: leds = 7'b1111110;
            1: leds = 7'b0110000;
            2: leds = 7'b1101101;
            3: leds = 7'b1111001;
            4: leds = 7'b0110011;
            5: leds = 7'b1011011;
            6: leds = 7'b1011111;
            7: leds = 7'b1110000;
            8: leds = 7'b1111111;
            9: leds = 7'b1111011;
            defaults: leds=7'b1111110; //bcd为其余值，显示数字0
            //同一个数字，管脚绑定不同，led的值也可能不同
    endcase
endmodule
```

### P126

```verilog
module Control (Clock, Reset, w, Pushn, CEN, LEDn); //控制使能信号生成模块
    input Clock, Reset, w, Pushn; // 输入信号：时钟Clock，复位信号Reset，工作信号w，按钮信号Pushn（低电平有效）
    output reg CEN; // 输出信号：控制使能信号CEN
    output LEDn; //输出信号：LED指示灯信号LEDn
    
    always @(posedge Clock) //Clock上升沿执行always模块内的操作
    begin
        if (Reset || ! Pushn) //如果复位信号Reset为高电平，或者按钮信号Pushn为低电平
           CEN<=0; //则将控制使能信号CEN置为0
        else if (w) //否则，如果工作信号w为高电平
           CEN<=1; //则将控制使能信号CEN置为1
    end
     assign LEDn=~CEN; //使用连续赋值语句将LEDn设置为CEN的反相
endmodule
```

### P127

```verilog
module Reaction(Clock, Reset, w, Pushn, LEDn, Digit1, Digit0);//Reaction模块，一个反应时间测量系统
    input Clock, Reset, w, Pushn; //时钟信号、复位信号、工作信号、按钮信号
    output wire LEDn; //输出信号，LED指示灯
    output wire [1:7] Digit1, Digit0; //十位和个位数码管的LED控制信号
    //内部信号
    wire CEN, c0, c1, c2, c3, c4, c5, c6,  c7, c8, c9; //使能控制信号即时钟分频信号
    wire [3:0] BCD1, BCD0; //十位和个位的BCD码，由BCDcount模块生成
    //子模块实例化
    Control    controller(Clock, Reset, w, Pushn, CEN, LEDn); // 控制使能信号生成模块
    clk_div_phase   divider(Clock, c0, c1, c2, c3, c4, c5, c6,  c7, c8, c9); // 时钟分频模块
    BCDcount    counter(c9, Reset, CEN, BCD1, BCD0 ); // BCD计数器模块，用于计算反应时间并转换为BCD码
    seg7    seg1(BCD1, Digit1); //十位数字数码管显示模块
    seg7    seg0(BCD0, Digit0); //个位数字数码管显示模块
endmodule
```

### P129

```verilog
module Reaction(Clock, Reset, c9,  w, Pushn, LEDn, Digit1, Digit0);
    input Clock, Reset, c9, w, Pushn;//c9为BCDcount模块的时钟信号
    output wire LEDn;
    output wire [1:7] Digit1, Digit0;
    reg LED; // 用于存储LED状态的寄存器
    wire [3:0] BCD1, BCD0;
      
    always @(posedge Clock) // 时钟上升沿触发的always块
    begin
        // 如果按钮被按下（Pushn为低）或复位信号有效（Reset为高），则LED清零
        if(!Pushn || Reset)
            LED <= 0;
        else if(w) // 否则，如果反应信号w有效（为高），则LED置1
            LED <= 1;
    end

    assign LEDn = ~LED; // 将LED的否逻辑赋值给LEDn输出信号
    BCDcount counter(c9, Reset, LED, BCD1, BCD0); // BCD计数器模块，用于计算反应时间并转换为BCD码
    seg7 seg1(BCD1, Digit1); //十位数字数码管显示模块
    seg7 seg0(BCD0, Digit0); //个位数字数码管显示模块
endmodule
```

## 有限状态机

> 触发器寄存器计数器部分代码注释作者为 **刘春旭** 与 **张溪延**。

### P58

```verilog
module simple (Clock, Resetn, w, z);//摩尔机simple模块
    input Clock, Resetn, w; //时钟信号，复位信号，输入信号
    output z; //输出信号
    reg [2:1] y, Y;//寄存器类型y储存当前状态，Y储存下一状态
    parameter [2:1] A = 2'b00, B = 2'b01, C = 2'b10; //A B C三种状态

    //组合逻辑电路
    //根据当前状态y和输入w确定下一状态Y
    always @(w, y)
        case (y)
            A: if (w)   Y = B;//当前状态y为A,且w为1时，下一状态Y为B
               else    Y = A;//当前状态y为A,且w为0时，下一状态Y为A.以下类似
            B: if (w)   Y = C;
               else    Y = A;
            C: if (w)   Y = C;
               else    Y = A;
            default:    Y = A;
        endcase
        
    // 更新当前状态，Clock上升沿或Resetn下降沿触发
    always @(negedge Resetn, posedge Clock)
        if (Resetn = = 0)   y <= A; //Resetn低电平，将状态重置为A
        else     y <= Y; //否则更新为下一状态Y
    
    // 当前状态为C时输出1，输出仅与当前状态y有关
    assign z = (y = = C);
 endmodule
```

### P59

```verilog
module simple (Clock, Resetn, w, z); //摩尔机simple模块
    input Clock, Resetn, w; //时钟信号，复位信号，输入信号
    output reg z;  //输出信号
    reg [2:1] y, Y; //寄存器类型y储存当前状态，Y储存下一状态
    parameter [2:1] A = 2'b00, B = 2'b01, C = 2'b10; //A B C三种状态
 
    //定义下一状态Y的组合逻辑电路，并在内部对输出z进行赋值
    always @(w, y)
    begin
        case (y) //根据当前状态y和输入w确定下一状态Y
            A: if (w)   Y = B;
                 else   Y = A;
            B: if (w)   Y = C;
                else    Y = A;
            C: if (w)   Y = C;
                else    Y = A;
            default:    Y = A;
        endcase
        z = (y = = C);  //根据当前状态确定输出
    end
 
    // 更新当前状态，Clock上升沿或Resetn下降沿触发
    always @(negedge Resetn, posedge Clock)
        if (Resetn = = 0)   y <= A; //Resetn低电平，将状态重置为A
        else     y <= Y; //否则更新为下一状态Y
    
endmodule
```

### P60

```verilog
module simple (Clock, Resetn, w, z);//摩尔机simple模块
    input Clock, Resetn, w; //时钟信号，复位信号，输入信号
    output z; //输出信号
    reg [2:1] y; // 状态寄存器y，2位宽，用于存储当前状态
    parameter [2:1] A = 2'b00, B = 2'b01, C = 2'b10; //A B C三种状态
 
    // 在时钟上升沿或复位信号下降沿触发
    always @(negedge Resetn, posedge Clock)
        if (Resetn = = 0)   y <= A; //Resetn低电平，将状态重置为A
        else  //否则根据当前状态y和输入w确定下一状态Y
            case (y)
                A: if (w)   y <= B;
                    else    y <= A;
                B: if (w)   y <= C;
                    else    y <= A;
                C: if (w)   y <= C;
                    else    y <= A;
                default:    y <= A;
            endcase
 
    // 当前状态为C时输出1，输出仅与当前状态y有关
    assign z = (y = = C);
 
endmodule
```

### P62

```verilog
module mealy (Clock, Resetn, w, z); // 定义模块名 mealy
    input Clock, Resetn, w; // Clock 为时钟信号，Resetn 为复位信号，w 为输入信号
    output reg z; // z 为输出信号，使用 reg 数据类型以便在 always 块中赋值
    reg y, Y; // 声明寄存器变量 y 和 Y，用于存储当前状态和下一状态

    
    parameter A = 1'b0, B = 1'b1; // A 和 B 是状态机的两个状态

    always @(w, y) // 每当输入信号 w 或当前状态 y 发生变化时，触发 always 块
        case (y) // 根据当前状态 y 选择不同的逻辑
            A: // 当前状态为 A 时
                if (w)  begin // 如果输入信号 w 为 1
                    z = 0; // 输出信号 z 设置为 0
                    Y = B; // 下一状态 Y 变为 B
                end
                else        begin // 如果输入信号 w 为 0
                    z = 0; // 输出信号 z 设置为 0
                    Y = A; // 下一状态 Y 保持为 A
                end
            B: // 当前状态为 B 时
                if (w)  begin // 如果输入信号 w 为 1
                    z = 1; // 输出信号 z 设置为 1
                    Y = B; // 下一状态 Y 保持为 B
                end
                else        begin // 如果输入信号 w 为 0
                    z = 0; // 输出信号 z 设置为 0
                    Y = A; // 下一状态 Y 变为 A
                end
        endcase // 结束 case 语句
endmodule // 结束模块定义
```

### P71

```verilog
module simple (Clock, Resetn, w, z); // 定义模块 simple
    input Clock, Resetn, w; // Clock 为时钟信号，Resetn 为复位信号，w 为输入信号
    output z; // z 为输出信号
    reg [2:1] y, Y; // 定义2 位宽寄存器 y 和 Y存储当前状态和下一状态
    parameter [2:1] A = 2'b00, B = 2'b01, C = 2'b10; // 定义 3 个状态 A, B, C

    // 定义状态转换的组合逻辑
    always @(w, y) // 每当输入信号 w 或当前状态 y 发生变化时，触发 always 块
        case (y) // 根据当前状态 y 选择不同的转换逻辑
            A: if (w)   Y = B; // 如果当前状态是 A，且输入信号 w 为 1，下一状态变为 B
               else    Y = A; // 如果当前状态是 A，且输入信号 w 为 0，下一状态保持为 A
            B: if (w)   Y = C; // 如果当前状态是 B，且输入信号 w 为 1，下一状态变为 C
               else    Y = A; // 如果当前状态是 B，且输入信号 w 为 0，下一状态变为 A
            C: if (w)   Y = C; // 如果当前状态是 C，且输入信号 w 为 1，下一状态保持为 C
               else    Y = A; // 如果当前状态是 C，且输入信号 w 为 0，下一状态变为 A
            default:    Y = A; // 如果当前状态不符合以上任何情况，则下一状态设为 A
        endcase // 结束 case 语句

   
    always @(negedge Resetn, posedge Clock) // 当复位信号 Resetn 下降沿或时钟信号 Clock 上升沿时，触发此 always 块
        if (Resetn == 0)   y <= A; // 如果Resetn 为 0，当前状态 y 设为 A
        else     y <= Y; // 否则，当前状态 y 更新为下一状态 Y

    // 定义输出逻辑
    assign z = (y == C); // 当前状态 y 为 C 时，输出信号 z 为 1，否则为 0
endmodule // 结束模块定义
```

### P72

```verilog
module mealy (Clock, Resetn, w, z); // 定义模块 mealy
    input Clock, Resetn, w; // Clock 为时钟信号，Resetn 为复位信号，w 为输入信号
    output reg z; // z 为输出信号，因为 z 的值在 always 块内赋值,所以使用 reg 类型
    reg y, Y; // 定义寄存器 y 和 Y，用来存储当前状态和下一状态
    parameter A = 1'b0, B = 1'b1; // 定义 2 个状态 A 和 B

    // 定义状态转换和输出的组合逻辑
    always @(w, y) // 每当输入信号 w 或当前状态 y 改变时，触发 always 块
        case (y) // 根据当前状态 y 选择不同的行为
            A:  if (w)  begin // 如果当前状态是 A，并且 w 为 1
                    z = 0; // 输出 z 为 0
                    Y = B; // 下一状态设为 B
                end
                else        begin // 如果当前状态是 A，并且 w 为 0
                    z = 0; // 输出 z 为 0
                    Y = A; // 下一状态保持为 A
                end
            B:  if (w)  begin // 如果当前状态是 B，并且 w 为 1
                    z = 1; // 输出 z 为 1
                    Y = B; // 下一状态保持为 B
                end
                else        begin // 如果当前状态是 B，并且 w 为 0
                    z = 0; // 输出 z 为 0
                    Y = A; // 下一状态变为 A
                end
        endcase // 结束 case 语句
endmodule // 结束模块定义
```

### P82

```verilog
module arbiter (r, Resetn, Clock, g); // 定义模块 arbiter
    input [1:3] r; //三位信号r表示三个请求信号 r[1], r[2], r[3]
    input Resetn, Clock; // Resetn 是复位信号，Clock 是时钟信号
    output wire [1:3] g; // 三位信号g，，对应三个输出 g[1], g[2], g[3]
    reg [2:1] y, Y; // 定义当前状态寄存器 y 和下一状态寄存器 Y
    parameter Idle = 2'b00, gnt1 = 2'b01, gnt2 = 2'b10, gnt3 = 2'b11; // 定义四个状态，Idle ，gnt1、gnt2、gnt3 
    // 定义下一状态的组合逻辑
    always @(r, y) // 每当输入信号 r 或当前状态 y 发生变化时，触发 always 块
        case (y) // 根据当前状态 y 执行不同的状态转换逻辑
            Idle:   // 当前状态是 Idle 
                if (r[1]) Y = gnt1; // 如果信号为 r[1]，下一状态变为 gnt1
                else if (r[2]) Y = gnt2; // 如果信号为 r[2]，下一状态变为 gnt2
                else if (r[3]) Y = gnt3; // 如果信号为 r[3]，下一状态变为 gnt3
                else Y = Idle; // 如果没有任何信号有效，保持 Idle 
            gnt1: // 当前状态是 gnt1 
                if (r[1])  Y = gnt1; // 如果r[1] 仍然有效，保持 gnt1 状态
                else  Y = Idle; // 否则下一状态变为 Idle
            gnt2: // 当前状态是 gnt2 
                if (r[2])  Y = gnt2; // 如果 r[2] 仍然有效，保持 gnt2 状态
                else  Y = Idle; // 否则下一状态变为 Idle
            gnt3: // 当前状态是 gnt3
                if (r[3])  Y = gnt3; // 如果 r[3] 仍然有效，保持 gnt3 状态
                else  Y = Idle; // 否则下一状态变为 Idle
            default: Y = Idle; // 默认情况下，下一状态为 Idle
        endcase // 结束 case 语句
        
    // 定义状态寄存器的时序逻辑
    always @(posedge Clock) // 在时钟上升沿触发 
        if (Resetn == 0)        y <= Idle; // 如果 Resetn 信号为 0，复位状态机
        else     y <= Y; // 否则，将下一状态 Y 更新为当前状态 y

    // 定义输出信号的组合逻辑
    assign g[1] = (y == gnt1); // 当前状态为 gnt1 时，输出信号 g[1] 
    assign g[2] = (y == gnt2); // 当前状态为 gnt2 时，输出信号 g[2]
    assign g[3] = (y == gnt3); // 当前状态为 gnt3 时，输出信号 g[3] 

endmodule // 模块定义结束
```

### P88

```verilog
module ex1 (Clock, A, B, Z); // 定义模块 ex1
    input Clock, A, B; // Clock 是时钟信号，A 和 B 是控制信号
    output reg Z; // 一个寄存器类型的信号Z，用于输出状态
    reg [2:0] c_s, n_s; // 定义两个 3 位寄存器 c_s 和 n_s，分别表示当前状态和下一状态

    parameter [2:0] INIT = 3'b000, // 定义状态 INIT (初始化状态) 的编码为 000
                    A0    = 3'b001, // 定义状态 A0 的编码为 001
                    A1    = 3'b010, // 定义状态 A1 的编码为 010
                    OK0   = 3'b011, // 定义状态 OK0 的编码为 011
                    OK1   = 3'b100; // 定义状态 OK1 的编码为 100
    
    // 状态寄存器的时序逻辑
    always @(posedge Clock) // 在时钟信号的上升沿触发
        c_s <= n_s; // 将下一状态 n_s 的值赋给当前状态 c_s，完成状态更新

    always @(c_s) // 当前状态 c_s 发生变化时触发
        case (c_s) // 根据当前状态 c_s 的值决定输出 Z
            INIT, A0, A1: Z = 0; // 在状态 INIT、A0 和 A1 中，输出 Z 为 0
            OK0, OK1:     Z = 1; // 在状态 OK0 和 OK1 中，输出 Z 为 1
            default:      Z = 0; // 默认情况下，输出 Z 为 0
        endcase // 结束 case 语句

    always @(A, B, c_s) begin // 当输入 A、B 或当前状态 c_s 变化时触发
        case (c_s) // 根据当前状态 c_s 和输入信号 A、B 的值决定下一状态 n_s
            INIT:  // 当前状态是 INIT
                if (A == 0)   n_s = A0; // 如果 A 为 0，下一状态为 A0
                else          n_s = A1; // 如果 A 为 1，下一状态为 A1
            A0:    // 当前状态是 A0
                if (A == 0)   n_s = OK0; // 如果 A 为 0，下一状态为 OK0
                else          n_s = A1; // 如果 A 为 1，下一状态为 A1
            A1:    // 当前状态是 A1
                if (A == 0)   n_s = A0; // 如果 A 为 0，下一状态为 A0
                else          n_s = OK1; // 如果 A 为 1，下一状态为 OK1
            OK0:   // 当前状态是 OK0
                if (A == 0)   n_s = OK0; // 如果 A 为 0，保持状态为 OK0
                else if ((A == 1) && (B == 0)) n_s = A1; // 如果 A 为 1 且 B 为 0，下一状态为 A1
                else          n_s = OK1; // 否则，下一状态为 OK1
            OK1:   // 当前状态是 OK1
                if ((A == 0) && (B == 0)) n_s = A0; // 如果 A 为 0 且 B 为 0，下一状态为 A0
                else if ((A == 0) && (B == 1)) n_s = OK0; // 如果 A 为 0 且 B 为 1，下一状态为 OK0
                else          n_s = OK1; // 否则，保持状态为 OK1
            default: n_s = INIT; // 默认情况下，下一状态为 INIT
        endcase // 结束 case 语句
    end // 结束 always 块     
endmodule // 模块定义结束
```

### P89

```verilog
module ex1 (Clock, A, B, Z); // 定义模块 ex1
    input Clock, A, B; // Clock 是时钟信号，A 和 B 是输入控制信号
    output wire Z; // Z是一个组合逻辑的信号
    reg LASTA; // 寄存器 LASTA用于存储 A 的上一时刻的值
    reg [1:0] c_s, n_s; // 定义两个 2 位寄存器 c_s 和 n_s，分别表示当前状态和下一状态

    parameter [1:0] INIT = 2'b00, // 定义状态 INIT 的编码为 00
                    Looking = 2'b01, // 定义状态 Looking 的编码为 01
                    OK      = 2'b11; // 定义状态 OK 的编码为 11


    always @(posedge Clock) begin // 在时钟信号的上升沿触发
        if (rst)                  // 如果复位信号 rst 有效
            c_s <= INIT;          // 当前状态 c_s 设为 INIT
        else                      // 如果复位信号无效
            c_s <= n_s;           // 将下一状态 n_s 的值赋给当前状态 c_s，完成状态更新
        LASTA <= A;               // 将 A 的值存储到 LASTA 中
    end

    assign Z = (c_s == OK) ? 1 : 0; // 定义组合逻辑输出 Z，若当前状态是 OK 时，Z 为 1；否则 Z 为 0

    // 状态转移的组合逻辑
    always @(A, B, LASTA, c_s) begin // 当输入 A、B、LASTA 或当前状态 c_s 变化时触发
        case (c_s)                   // 根据当前状态 c_s 和输入信号的值决定下一状态 n_s
            INIT:     n_s = Looking; // 当前状态是 INIT 时，直接转移到 Looking 状态
            Looking:                 // 当前状态是 Looking 时
                if (A == LASTA)       // 如果输入信号 A 和 LASTA 相等
                    n_s = OK;         // 下一状态转移到 OK
                else                  // 如果输入信号 A 和 LASTA 不相等
                    n_s = Looking;    // 保持在 Looking 状态
            OK: // 当前状态是 OK 时
                if ((A == LASTA) || (B == 1)) // 如果 A 和 LASTA 相等，或者 B 信号为 1
                    n_s = OK;         // 保持在 OK 状态
                else                  // 否则
                    n_s = Looking;    // 转移到 Looking 状态
            default:                  // 默认情况下
                n_s = INIT;           // 转移到 INIT 状态
        endcase // 结束 case 语句
    end // 结束 always 块
endmodule // 模块定义结束
```

## 数字系统设计

> 触发器寄存器计数器部分代码注释作者为 **张溪延** 与 **范泽夏**。

### P4

```verilog
module regn (bus, in_en, out_en, Clock); // 定义模块 regn
    inout [7:0] bus; // 定义一个 8 位的bus用于数据输入和输出
    input in_en, Clock; //  in_en 表示写入使能信号，Clock 为时钟信号
    reg [7:0] Q; // 定义 8 位寄存器 Q用于存储 bus 上的数据

    always @(posedge Clock) // 在时钟上升沿触发
        if (in_en)          // 如果写入使能信号  有效
            Q <= bus;       // 将 bus 上的数据存储到寄存器 Q 中

    assign bus = (out_en) ? Q : 8'hzz; // 如果输出使能信号 out_en 有效，将 Q 的值赋给 bus；否则 bus 处于高阻态 
endmodule // 模块定义结束
```


### P6

```verilog
module regn (R, L, Clock, Q);
    // 输入端口定义
    input [7:0] R; // 8位输入R
    input L, Clock; // L和Clock输入
    output reg [7:0] Q; // 8位输出Q，为寄存器类型
    
    // 时钟上升沿触发，如果L为高，则Q更新为R的值
    always @(posedge Clock)
        if(L)   Q <= R;
    
endmodule

module trin (Y, E, F);
    // 输入端口定义
    input [7:0] Y; // 8位输入Y
    input E; // E输入
    output wire [7:0] F; // 8位输出F，为线网类型
    
    // 根据E的值，F被赋值为Y或高阻态
    assign F = E ? Y : 8'bz;
    
endmodule
```

### P10

```verilog
// 状态机的case语句，根据y的值更新Y
always @(w,y)
    case(y)
        A: if(w) Y=B; // 如果w为高，则Y更新为B
            else Y = A; // 否则Y保持A
        B: Y=C; // Y更新为C
        C: Y=D; // Y更新为D
        D: Y=A; // Y更新为A
    endcase

// 时钟下降沿或复位信号触发，更新y的状态
always @(negedge Resetn, posedge Clock)
    if(Resetn == 0) y <= A; // 如果复位信号为低，则y更新为A
    else y <= Y; // 否则y更新为Y的值

// 根据y的值，输出不同的信号
assign R2out = (y==B); // 如果y为B，R2out为高
assign R3in = (y==B); // 如果y为B，R3in为高
assign R1out = (y==C); // 如果y为C，R1out为高
assign R2in = (y==C); // 如果y为C，R2in为高
assign R3out = (y==D); // 如果y为D，R3out为高
assign R1in = (y==D); // 如果y为D，R1in为高
assign Done = (y==D); // 如果y为D，Done为高
```

### P23

```verilog
// 总线和寄存器定义
trin tri ext (Data, Extern, BusWires); // 三态缓冲器
regn reg 0 (BusWires, Rin[0], Clock, R0); // 寄存器0
regn reg 1 (BusWires, Rin[1], Clock, R1); // 寄存器1
regn reg 2 (BusWires, Rin[2], Clock, R2); // 寄存器2
regn reg 3 (BusWires, Rin[3], Clock, R3); // 寄存器3

trin tri 0 (R0, Rout[0], BusWires); // 三态缓冲器0
trin tri 1 (R1, Rout[1], BusWires); // 三态缓冲器1
trin tri 2 (R2, Rout[2], BusWires); // 三态缓冲器2
trin tri 3 (R3, Rout[3], BusWires); // 三态缓冲器3
regn reg A (BusWires, Ain, Clock, A); // 寄存器A

// ALU单元定义
always @(AddSub, A, BusWires)
    if (!AddSub) // 如果AddSub为低，则执行加法
        Sum = A + BusWires; // A和BusWires相加
    else // 否则执行减法
        Sum = A - BusWires; // A减去BusWires

regn reg G (Sum, Gin, Clock, G); // 寄存器G
trin tri G (G, Gout, BusWires); // 三态缓冲器G
```

### P79

```verilog
module adder_pipe2(cout, sum, ina, inb, cin, clk);
// 输入端口定义
input[7:0] ina, inb; // 两个8位加数输入
input cin, clk; // 进位输入和时钟信号
// 输出端口定义
output reg[7:0] sum; // 8位求和结果输出
output reg cout; // 进位输出
// 内部信号定义
reg[3:0] tempa, tempb, firsts; // 用于存储加法中间结果的4位寄存器
reg firstc; // 用于存储低位加法的进位结果

// 第一个加法器，处理低位
always @(posedge clk)
begin  
    {firstc,firsts}<=ina[3:0]+inb[3:0]+cin; // 低位相加并计算进位
    tempa<=ina[7:4];  // 输入同步
    tempb<=inb[7:4]; // 输入同步
end

// 第二个加法器，处理高位
always @(posedge clk)
begin  
    {cout,sum[7:4]}<=tempa+tempb+firstc; // 高位相加并计算进位
    sum[3:0]<=firsts; //输出同步
end
endmodule
```

### P99

```verilog
// 时钟上升沿或复位信号下降沿触发，更新data_in
always @(posedge clk or negedge resetn) begin
    if(!resetn) // 如果复位信号为低
        data_in <= 0; // data_in清零
    else if(strobe) // 如果strobe为高
        data_in <= tx_data; // data_in更新为tx_data的值
end    if(!resetn)
        data_in <= 0;
    else if(strobe)
        data_in <= tx_data;
end
```

### P100

```verilog
// 时钟上升沿或复位信号下降沿触发，更新state_c
always @(posedge clk or negedge resetn) begin
    if(!resetn) // 如果复位信号为低
        state_c <= 4'b0000; // state_c清零
    else if(strobe) // 如果strobe为高
        state_c <= state_n; // state_c更新为state_n的值
end
```

```verilog
// 根据state_c的值，更新dout
always @(*) begin
    case(state_c)
        4'b0000: dout = 1'b1; // 如果state_c为0000，dout为1
        4'b0001: dout = 1'b0; // 如果state_c为0001，dout为0
        4'b0010: dout = data_in[0]; // 如果state_c为0010，dout为data_in的第0位
        4'b0011: dout = data_in[1]; // 如果state_c为0011，dout为data_in的第1位
        4'b0100: dout = data_in[2]; // 如果state_c为0100，dout为data_in的第2位
        4'b0101: dout = data_in[3]; // 如果state_c为0101，dout为data_in的第3位
        4'b0110: dout = data_in[4]; // 如果state_c为0110，dout为data_in的第4位
        4'b0111: dout = data_in[5]; // 如果state_c为0111，dout为data_in的第5位
        4'b1000: dout = data_in[6]; // 如果state_c为1000，dout为data_in的第6位
        4'b1001: dout = data_in[7]; // 如果state_c为1001，dout为data_in的第7位
        4'b1010: dout = ~(^data_in); // 如果state_c为1010，dout为data_in所有位的异或非
        4'b1011: dout = 1'b1; // 如果state_c为1011，dout为1
        default: dout = 1'b1; // 默认dout为1
    endcase
end
```

```verilog
// 根据state_c的值，更新state_n
always @(*) begin
    case(state_c)
        4'b0000: state_n = if(tx_strobe==1'b1) // 如果state_c为0000且tx_strobe为高
                            state_n = 4'b0001; // state_n更新为0001
                        else state_n = 4'b0000; // 否则保持不变
        4'b0001: state_n = 4'b0010; // 如果state_c为0001，state_n更新为0010
        4'b0010: state_n = 4'b0011; // 如果state_c为0010，state_n更新为0011
        4'b0011: state_n = 4'b0100; // 如果state_c为0011，state_n更新为0100
        4'b0100: state_n = 4'b0101; // 如果state_c为0100，state_n更新为0101
        4'b0101: state_n = 4'b0110; // 如果state_c为0101，state_n更新为0110
        4'b0110: state_n = 4'b0111; // 如果state_c为0110，state_n更新为0111
        4'b0111: state_n = 4'b1000; // 如果state_c为0111，state_n更新为1000
        4'b1000: state_n = 4'b1001; // 如果state_c为1000，state_n更新为1001
        4'b1001: state_n = 4'b1010; // 如果state_c为1001，state_n更新为1010
        4'b1010: state_n = 4'b1011; // 如果state_c为1010，state_n更新为1011
        default: state_n = 4'b0000; // 默认state_n更新为0000
    endcase
end
```

### P102

```verilog
// 时钟上升沿或复位信号下降沿触发，更新data_in
always @(posedge clk or negedge resetn) begin
    if(!resetn) // 如果复位信号为低
        data_in <= 0; // data_in清零
    else if(strobe) // 如果strobe为高
        data_in <= tx_data; // data_in更新为tx_data的值
end

// 时钟上升沿或复位信号下降沿触发，更新tx_flag
always @(posedge clk or negedge resetn) begin
    if(!resetn) // 如果复位信号为低
        tx_flag <= 1'b0; // tx_flag清零
    else if(tx_strobe) // 如果tx_strobe为高
        tx_flag <= 1'b1; // tx_flag置为1
    else if(cnt == 8) // 如果cnt等于8
        tx_flag <= 1'b0; // tx_flag清零
end
```

### P103

```verilog
// 时钟上升沿或复位信号下降沿触发，更新cnt
always @(posedge clk or negedge resetn) begin
    if(!resetn) // 如果复位信号为低
        cnt <= 4'b0000; // cnt清零
    else if(cnt == 8) // 如果cnt等于8
        cnt <= 4'd0; // cnt清零
    else if(tx_flag) // 如果tx_flag为高
        cnt <= cnt + 1'b1; // cnt加1
end
```

```verilog
// 根据tx_flag和cnt的值，更新dout
always @(posedge clk or negedge resetn) begin
    if(!resetn) dout <= 1'b1; // 如果复位信号为低，dout置为1
    else if(tx_flag) // 如果tx_flag为高
        case(cnt) // 根据cnt的值
            0: dout <= 1'b0; // 如果cnt为0，dout置为0
            1: dout <= data_in[0]; // 如果cnt为1，dout为data_in的第0位
            2: dout <= data_in[1]; // 如果cnt为2，dout为data_in的第1位
            3: dout <= data_in[2]; // 如果cnt为3，dout为data_in的第2位
            4: dout <= data_in[3]; // 如果cnt为4，dout为data_in的第3位
            5: dout <= data_in[4]; // 如果cnt为5，dout为data_in的第4位
            6: dout <= data_in[5]; // 如果cnt为6，dout为data_in的第5位
            7: dout <= data_in[6]; // 如果cnt为7，dout为data_in的第6位
            8: dout <= data_in[7]; // 如果cnt为8，dout为data_in的第7位
            default: dout <= 1'b1; // 默认dout置为1
        endcase
    else // 否则
        dout <= 1'b1; // dout置为1
end
```

### P106

```verilog
// 时钟上升沿或复位信号下降沿触发，更新state
always @(posedge clk or negedge resetn)begin
    if(!resetn) state <= 3'b000; // 如果复位信号为低，state清零
    else
        case(state) // 根据state的值
            3'b000: if(!din) state <= 3'b001; // 如果state为000且din为低，state更新为001
            3'b001: state <= 3'b010; // 如果state为001，state更新为010
            3'b010: state <= 3'b011; // 如果state为010，state更新为011
            3'b011: state <= 3'b100; // 如果state为011，state更新为100
            3'b100: state <= 3'b101; // 如果state为100，state更新为101
            default: state <= 3'b000; // 默认state清零
        endcase
end
```

### P107

```verilog
// 时钟上升沿或复位信号下降沿触发，更新dbuf
always @(posedge clk or negedge resetn)begin
    if(!resetn) dbuf <= 4'b0000; // 如果复位信号为低，dbuf清零
    else
        case(state) // 根据state的值
            1: dbuf[0] <= din; // 如果state为1，dbuf的第0位更新为din
            2: dbuf[1] <= din; // 如果state为2，dbuf的第1位更新为din
            3: dbuf[2] <= din; // 如果state为3，dbuf的第2位更新为din
            4: dbuf[3] <= din; // 如果state为4，dbuf的第3位更新为din
        endcase
end
```

```verilog
// 时钟上升沿或复位信号下降沿触发，更新strobe和dout
always @(posedge clk or negedge resetn)begin
    if(!resetn) begin
        strobe <= 1'b0; // 如果复位信号为低，strobe清零
        dout <= 4'b0; // dout清零
    end
    else
        if( state==3'b101 && ^dbuf==~din) begin
            strobe <= 1'b1; // 如果state为101且dbuf的异或非等于din的反相，strobe置为1
            dout <= dbuf; // dout更新为dbuf的值
        end
        else begin
            strobe <= 1'b0; // 否则，strobe清零
            dout <= 4'b0; // dout清零
        end
end
```
