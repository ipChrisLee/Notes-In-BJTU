# 第一章



**不同进制的简写（P3）**

**十进制小数转二进制数怎么转**

**二进制与十六进制、八进制怎么快速转换（P5）**

**2421D编码是什么样的**：[*](从0000~0100再从1011~1111)

**余3码是怎么制作的**：[*](就是8421D码每个+3)

**常用的几个码制里哪些是恒权码哪些是无权码**：[*](只有余3码是无权码)

**十进制数转BCD码怎么转，能忽略前置0或者末尾0吗**：[*](就是把每个数字都变成BCD码之后再组合在一起，注意不能忽略前置/末尾0)

**格雷码怎么生成的**

**逻辑电路中正逻辑和负逻辑的概念**：[*](我们一般用的是正逻辑，高电平取0的是负逻辑)



# 第二章



**三极管在逻辑电路中一般工作在哪两个状态？**：[*](饱和和截止)

**从制造工艺上来说可以把目前的数字集成电路分成哪三种？（P40中）**：[*](双极型、单极型、混合型)

**TTL相比CMOS最大的缺点是，因而导致两者在制作集成电路时的区别是？**：[*](功耗，TTL的功耗大于CMOS的，因此TTL只适合做小规模、中规模的，而CMOS可以用于大规模的集成电路)



## 逻辑电路的特性



**门电路的输入输出电压值**$V_{IL},V_{IH},V_{OL},V_{OH}$**的含义？高/低电平的噪声容限**$V_{NH},V_{NL}$**怎么用左边的变量表示？（P41）**:[*](前四个变量分别表示输入低电平时的最高电压、输入高电平时的最低电压，输出低电平时的最高电压，输出低电平时的最低电压，后面那个问题见书P42)

**器件的噪声容限越大，什么能力越强？**：[*](抗干扰能力)



**传输延迟时间的定义？**：[*](在输入脉冲波形的作用下输出脉冲波形相对输入的延迟了多长时间)

$t_{pHL},t_{pLH}$**的在某一个波形图上的定义应该是？**：[*](输出沿的中点和输入沿的中点的时间差，LH、HL表示输出波形上升沿、下降沿（书上可能有误）)



## CMOS逻辑门电路



**MOS管的开关状态怎么看？**：[*](如果中间部分电平下降方向与箭头方向相反，表示MOS管导通)

**CMOS电路指什么？**：[*](由N沟道/P沟道两种场效应管组成的电路)

**CMOS的噪声容限与什么有关？是什么样的关系？**：[*](与电源电压有关，电源电压越高其噪声容限越高)

**为什么需要线与？（P48下）**：[*](为了输出电平变换，吸收大负载电流以及实现与逻辑的功能)

**漏极开路是指什么？使用时需要附加什么？有什么符号？**：[*](CMOS门输出电路只有N MOS管，并且漏极开路；需要加上上拉电阻和电源；画横线的菱形)

**OD门电路功能有？**：[*](提高输出电流，实现逻辑电平转换)

**三态门有哪三态？主要用于什么？**：[*](高低电平和高阻态；总线传输)

**CMOS器件的优点？**：[*](功耗低，扇出数大，噪声容限大)



# 第三~五章



**逻辑电路分为？**：[*](组合逻辑电路和时序逻辑电路)

**组合电路的特点？（区分于时序逻辑电路）**：[*](任何时刻输出只和输入有关；输入和输出之间没有反馈电路、电路中不含记忆元件且由逻辑门电路组成)

**触发器指什么？**：[*](能够存储1位二值信号的基本电路统称为触发器)

**时序逻辑电路分为什么？**：[*](缪尔型和米里型，前者只与触发器现态有关与输入无关；后者与现态和输入都有关)

**同步时序和异步时序的区别**：[*](同步中所有触发器时钟端和同一个时钟脉冲源连接，异步没有统一的时钟脉冲)



# 第六章



**大规模集成组合逻辑电路包括？**：[*](半导体存储器和可编程逻辑器件)

**半导体存储器是？**：[*](存储大量二值数据信息代码的半导体器件)

**半导体存储器的特点**：[*](集成度高、体积小、存储信息容量大、工作速度快)

**可编程逻辑器件有什么特点？**：[*](可以由用户定义和设置逻辑功能，取代小规模标准逻辑器件；结构灵活、集成度高、处理速度快和可靠性高)

**半导体存储器件的分类？**：[*](只读存储器ROM，随机存取存储器RAM)

**RAM和ROM由哪三个部分组成**：[*](地址译码器、存储矩阵、输入输出控制)

**PLD的全称是什么？**：[*](可编程逻辑器件)

**PLD基本结构有什么？**：[*](与逻辑阵列 和 或逻辑阵列)

**PLD的分类是什么样的？**：[*](低密度PLD包括：只读存储器PROM、可编程逻辑阵列PLA（我们用的）、可编程阵列逻辑PAL、通用阵列逻辑GAL；高密度的包括复杂可编程器件CPLD、现场可编程门阵列FPGA)



# 第八章

**模数转换的步骤**：[*](取样、保持、量化、编码)











