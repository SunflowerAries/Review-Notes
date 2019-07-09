# <center>Computer Organization</center>

[TOC]

## 运算方法和运算部件

### 定点数加减运算

#### 溢出标志与进位/借位标志

溢出标志的两种判别方式
$$
OF = C_n \oplus C_{n - 1}
$$

$$
OF = X_{n - 1}Y_{n - 1}\overline F_{n - 1} + \overline X_{n - 1} \overline Y_{n - 1} F_{n - 1}
$$

CF针对无符号数，OF针对有符号数。判断时需要转换成对应形式运算查看是否溢出。以四位为例，$CF = (result<0 \ | \ result> 15)$ , $OF = (result < -8 \ | \ result > 7)$

<div>
    <img src="Pic/OF-CF.png">
</div>

#### 串行进位加法器

<div>
    <img src="Pic/Carryout and Output.png">
</div>

进位按串行方式传递，速度慢

#### 并行进位加法器

<div>
    <img src="Pic/CLA.png">
</div>

#### 局部（单级）先行进位加法器

组内并行、组间串行

#### 多级先行进位加法器

组内并行、组间并行

### 定点数乘除法运算

#### 原码乘法

##### 一位原码乘法

符号位由两个乘数的符号异或得到。

<div>
    <img src="Pic/1 true form mul.png">
</div>

##### 二位原码乘法

需要三位符号位，因为有“+2X”，所以可能存在加法溢出使数值部分占用两位符号位。

<div>
    <img src="Pic/2 true form mul.png">
</div>

#### 补码乘法运算

##### 一位补码乘法

<div>
    <img src="Pic/Booth derivation.png">
</div>

<div>
    <img src="Pic/1 complement mul.png">
</div>

##### 两位补码乘法

<div>
    <img src="Pic/2 complement mul.png">
</div>

#### Hint

判断溢出

- unsigned类型时，如果乘积的高n位全为0，则不溢出
- signed类型时，乘积的高n+1位位全0或全1，则不溢出

#### 恢复余数除法

假定被除数X为2n位，除数Y和商为n位

- 第一步判断溢出，对于无符号整数除法，如果试商为1，则发生溢出；对于原码定点小数，如果试商为1，则可以通过右规消除
- 中间余数$R_{i + 1}$ = 2$R_i$ - Y。如果$R_{i + 1}$ < 0，则上商$q_{n  - i} = 0$，同时恢复余数$R_{i + 1} = R_{i + 1} + Y$；若$R_{i + 1}$ >= 0，则上商$q_{n  - i} = 1$
- 循环执行第二步n次

##### Hint

- 余数阶码与移位次数相关
- 原码在后面补n位0，无符号在前面补n位0

<div>
    <img src="Pic/恢复余数除法.png">
</div>
## 指令系统

