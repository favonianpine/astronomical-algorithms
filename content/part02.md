# 第2章 关于精度

  本章所要讨论的主题是：具体问题所需要的精度；程序设计语言的精度；最后结果发表的精度。

具体给定问题所需的精度

  所需的计算精度，取决于具体问题的目标要求。例如，如果想计算行星的升或降的时刻，计算到0.01度已足够。原因是明显的，天球周日运动，每转过1度对应4分钟，所以0.01度的位置误差，引起的升或降时刻的误差大约为0.04分钟。为了解决这个问题，如果我们考虑数百个周期项，把位置精度控制到了0".01，那是在浪费精力和计算机的时间。

  但是，如果要计算行星引起的恒星食，那么就要求行星位置的计算精度高于1"，因为行星圆面的尺寸很小。

  为某一具体问题编写的程序，不一定适用于另于相似的问题。假设，为了计算恒星的位置，一个程序使用了第20章的“低精方法”得到的岁差。观测者利用望远镜寻找星体，使用这种精度的位置是足够的。但是，在计算“星食”或“会合”时，象这样低精度的位置是没有价值的。

  如果所需的精度已确定，我们必须使用一种适当的算法，满足其精度要求。John Mosley 提到了一个计算行星位置的商用价值的程序，但由于没有考虑摄动，即使在最近的几个角秒，土星、天王星、海王星的位置误差达1度。

  为了取得更高的精度，不是简单地对大约值保留更多的有效数字，常常需要使用其它方法。例如，如果想获得精确到0.01度的火星位置，使用无摄的椭圆轨道(开普勒运动)就已足够。然而，如果对火星位置精度要求达10"或更高，就必须考虑其它行星的摄动，程序将变得比较长。

  对于程序员，必须清楚他的公式及具体问题的精度要求，充分考虑各种可省略的项让程序变得更简洁。例如，涉及Date平分点的太阳几何平黄经：

  L = 280°27'59".244+129602771".380T+1".10915T2

  式中T是从历元2000年1月1.5日起算的儒略世纪数(36525个历书日)。在这一表达式中，如果|T|<0.95，即在1905年到2095年，最后一项(太阳的世纪加速度)小于1"。如果1"精度已足够，那么，在这一时期内，T2项可以忽略。但时，在公元100年，T=-19，最后一项的值是394"，大于0.1度。

计算机的精度

  这是一个很复杂的问题。程序设计语言可工作在足够位数的有效数字。注意：与小数位是不同的！例如，0.0000183有7位小数，但只有3个有效数字。有效数字指，除去左边前导的所有的"0"后，剩下数字的位数。

  一台机器，四舍五入到6位有效数字，结果1000000+2就是1000000。

  计算两个十分接近数的差值时，是很危险的。假设执行了以下减法：6.92736 - 6.92735 = 0.00001.每个数字的有效数字是6位，但是，相减后的结果却只有一位有效数字。另外，两个给定的数可能已做了四舍五入，那么情况将更糟糕。假设两个数的精确值是6.9273649和6.9273451，那么正确的结果是0.0000198，它几乎是刚才的两倍。

  6位或8位有效数字（早期的微机或如今的“单精度”），均不能满足天文计算的要求。

  在很多应用中，要求计算机的有效数字位数比最后结果的有效数字大很多位。例如，让我们考虑任意时刻的月亮平黄经公式，单位是度(参见第45章)：

L' = 218.3164591 + 481267.88134236T - 0.0013268T2 + 0.0000019T3

  式中T是历元2000年1月1.5TD起算的儒略世纪数(36525日)。假设，我们要得到0.001度的月亮平黄经精度。因为经度限制在0——360度，你可能会想到，计算机内部精度只需6位就已足够(小数点之前3位，小数点之后3位)。然而，这个问题不是这样的，因为，在转到0——360度之前，L'的值可能很大。

  例如，让我们计算2040年1月1日12h TD，即T=0.4时，L'的值。我们得到L'=192725.469度，转换后变为125.469度。但是，如果电脑工作在6位有效数字，得到的不再是L'=192725.469度，而192725度(6位！)，转换后变为125度，所以最后的结果精度只有1度，误差0.569度或28'，而且这仅发生在历元后的40年。在这种情况下，不可能用于计算日月食或星食。

  以下BASIC小程序可以找出电脑的内部精度：

10 X=1
20 J=0
30 X=X*2
40 IF X+1<>X THEN 60
50 GOTO 80
60 J=J+1
70 GOTO 30
80 PRINT J,J*0.30103
90 END

  这里，J是浮点数的尾数部分的比特数，而0.30103J是10进制数的有效数字。常数0.30103是log10(2)。例如，HP-85计算机的J=39，因此是11.7位。在HP-UX Technical Basic 5.0，工作在HP-Integral微机，我们得到J=52，因此是15.6位。QUICK-BASIC 4.5中得到的是J=63，因此是19.0位。

  不过，这个精度指简单的算术运算的精度，不是指三角函数。虽然日本东芝公司带GWBASIC电脑，J=55，有16.6位有效数字，但计算正弦时，只有7位是正确的，至少9位完全错误。

  一种迅速检测三角函数计算精度的方法是PRINT 4*ATN(1)。如果计算机工作在弧度制，应得到一个著名的数PI=3.14159265358979...或者，可以计算一个某个角的正弦值，该正弦值精确已知的，例如：

  sin(0.61弧度) = 0.57286746010048...

  计算机的舍入误差不是可避免的。例如，考虑这个值，1/3=0.33333333...因为计算机不能处理无限小数，一个数字必须在某处截断。

  从某次计算到下一次计算，舍入误差可能被积累。多数情况下，这是不要紧的，因为这种误差几乎可以忽略。但是，在某些算术应用中，这种误差积累可能无限制的增加，虽然这已超出本书的范围，不过，我们还要提到两种情况。

  考虑以下程序：

10 X=1/3
20 FOR I=1 TO 30
30 X=(9*X+1)*X-1
40 PRINT I,X
50 NEXT I
60 END

  第30行，实际上是将x赋值给它本身(1/3赋值给x)。可是，大部分计算机将得到无穷大的结果。用刚才提到的HP-UX Technical Basic得到：

4 步之后 0.333 333 333 333 308
14步之后 0.333 326 162 117 054
19步之后 0.215 899 338 763 055
24步之后 286.423...
30步之后,其值为10217

  不同的计算机的计算结果是不相同的，你甚至要以用手持式计算器做个测试：对1.0000001连续平方，27次之后，10位有效数字的结果应是674530.4707。以下是一些电脑或程序语言得到的结果：

674 494.06 在HP-67计算器
674 514.87 在HP-85微机
674 520.61 在TI-58计算器
674 530.4755 在HP-Integral(HP-UX Techn. Basic)
674 530.4755 在QUICKBASIC 4.5

  但是，故事还没结束。计算机用的数值信息有两种不同的基本表示方法。有些计算机，如老式的HP-85，使用BCD(二进制编码的10进制数)方案表示内部数值，而在其它的多数电脑中，使用二进制表示一个数。

  BCD编码方案是：对十进制数的每一位单独编码。这样，对于给定的计算机或程序设计言，所有的数字都可以被准确的表示了。另一方面，二进制编码，是用2的各次方的组合表示一个数。在二进制中，分数也是用2的各次方表示的，所以，不能用2的负次方的组合的的分数不是能用二进制表示的。例如1/10是不能用2的负次方组合得到，因为1/10 = 1/16+1/32+1/128...

  二进制算术运算比相应的BCD运算快得多，但是，有个不便之处就是，有些数(甚至是只有很少小数位的数)不能精确表示。

  其后果是，算术运算的结果可能不正确，哪怕是只有一点点的误差。假设x=4.34，H=INT(100*(x-INT(x)))的正确结果是34，然而，很多计算机语言得到的结果是33。原因是，此时计算机内部表示x的值为4.3399999998，或其它类似的值。

  另一个吃惊的例子：2+0.2+0.2+0.2+0.2+0.2-3

  在很多计算机中，结果不是0！在HP-Integral中，使用HP-UX Technical Basic 5.0，得到的结果是8.88*10-16。而在同样的电脑中，计算0.2+0.2+0.2+0.2+0.2+2-3的结果却是0，可见计算的顺序也是很重要的。让人惊呀的是，在HP-Integral中，使用以下程序计算2+(5*0.2)-3,我们们得到的结果零:

A=0.2+0.2+0.2+0.2+0.2
B=2+A
C=B-3
PRINT C

  考虑以下程序：

10 FOR I=0 TO 100 STEP 0.1
20 U=I
30 NEXT I
40 PRINT U
50 END

  这里，I和U取值依次是0到100，步长为0.1，最后得到的U应是100。在HP-85中的确是100，但在HP-Integral却得到99.9999999999986，这在某些应用中可能造成灾难性的后果。这个误差是由于0.1转为二进制表示，相当于0.0999999...，它与0.1相差很小，但是这里执行了1000步，最后的误差将放大了1000倍。在这种情况下，补救方法是，使用整数步长：

10 FOR J=0 TO 1000
20 I=J/10
30 U=I
40 NEXT J
50 PRINT U
60 END

  我们发现另一个吃惊的结果：

A=3*(1/3)
PRINT INT(A)

  在某些计算机中可能得到正确的结果1，有些却得到0。还可以试试这个例子：A = 0.1，PRINT INT(1000*A0)。

  另一个有趣的测试：

INPUT A
B=A/10
C=10*B
PRINT A-C

  结果应当是0，但不同的A值，结果可能有点差别。

  有一种简单的方法，判断计算机语言是否工作在BCD：通过查找整型变量所表示的最大整数值来判断。如果这个最大整数值是个“漂亮的整数”，那么表明机器工作在BCD编码。例如，在HP-85的最大整数是99999(或10^5-1)。但是，如果这个最大整数值是个陌生值(实际上它的2的n方，再减1)，那么机器不是工作在BCD。在早期的TRS-80，最大整数是32767(或2^15-1)，HP-Integral电脑上的HP-UX Technical Basic 5.0其最大整数是2147483647(或2^31-1)。

  算术运算中的四舍五入，可能导至其它吃惊的结果。在多数微机中，sqrt(25)-5的结果不是0！如果用这个结果作为关键测试值将是有问题的。根号25是个整数值吗？答案是否定的，因为计算机告诉我们sqrt(25)-INT(sqr(25))不等零。

最后结果的舍入

  结果应做舍入，保留正确的有意义的位数。

  “舍入”是取“最近”的近似值。例如，15.88取值为15.9或16，不是15。然而，日期或年的除外。例如，3月15.88日，指“一个时刻”附加在3月15日上：指3月15日0h之后的0.88日。所以，一个事件发生在3月15.88日，是指发生在3月15日，而不是3月16日。类似的，1977.69指1977年的某一时刻，而不是1978年。

  只有“有意义”的数字应当保留。例如，计算木星的可视星等的Muller公式：

m = -8.93 + 5 log(rΔ)

  式中r是木星到太阳的距离，Δ它到地球的距离，二者的单位是天文单位，对数计算是以10为底的。在1992年5月14日0h TD，我们有：
    r = 5.417149,Δ= 5.125382
  因此，m=-1.712514898。但是，如果以电脑算出这样的结果为借口，保留了所有的数字，这是十分可笑的，而且读者可能误解为精度非常的高。因为Muller公式中的常数-8.93的精度在0.01量级，你不能期望结果的精度比它更高。另外，由于木星大气的气象变化也将造成行星的星等的精度不超过0.01(甚至是0.1)。

  另一个例子，John Mosley 提到的用于计算天体升或降时刻的商用程序，精度是0.1秒钟，这也是不可能达到的精度。

  一些“感觉”及相关天文知识是很需要的。例如，计算月亮圆面被照亮部分的比例时，完全没有必要精确到0.000000001。

  执行舍入处理，应在所有的计算完成之后，而不是在程序开始或数据输入时。

  例如，计算1.4+1.4，结果取整。如果我们一开始就执行舍入，我们得到 1+1 = 2。实际上 1.4+1.4 = 2.8，四舍五入后得3。

  这里有另外一个例子。海王星“冲”的日期1996年7月18日，赤纬δ=-20°24'，那么行该星经过南子午圈时，地平纬度hm是多少？设观测站在德国Someberg天文台，纬度是φ=+50°23'，结果精确到度。使用以下公式：

  hm = 90°- φ + δ

  答案是: hm = 90°- 50°23' - 20°24' = 19°13' 因此是19°

  如果在计算之前对φ和δ做舍入计算，将得到不正确的结果：90°-50°-20°=20°.

  再看一个类似的错误：先把距离舍入到英里，再转换为千米。例如，在这种情况下，17km永远没有了,因为：
  10英里 = 16.09km，舍入后得16km
  11英里 = 17.70km，舍入后得18km

  赤经和赤纬。——因为24小时对应360度，1小时对应15度，时间1分钟对应角度15分(注意，是15m而不是15')，1秒钟对应角度1秒(即1s而不是1")，在1秒钟的时间内，地球自转了15"。

  正是由于这个原因，如果给出的天体赤纬精度为1"，赤经的精度则应舍入到十分之一时秒，否则赤纬的精度将高于赤经的精度。下表是赤经(α)和赤纬(δ)精度的大约对应关系。例如，如果δ的精度是1'，那么α应给出0.1m的精度。做为示例，表中还给出了Nova Cygni 1975的不同精度表示。

α	δ	例(Nova Cygni 1975)
1m
0m.1
1s
0s.1	0.1°
1'
0.1'
1"	α=21h10m
   21h09m.9
   21h09m53s
   21h09m52s.89	δ=+47°.9
   +47°57'
   +47°56'.7
   +47°56'41"
  最后注意，让我们讲述一下尾部的“0”，这很重要。例如，18.0与18是不同的。前者意味着真值介于17.95到18.05之间，而后者被舍入为整数，真值介于17.5到18.5之间。由于这个原因，为了表示精度，尾部的0必须给出：恒星的星等为7，它与7.00是不同的。

参考资料

1、John Mosley，《天空和望远镜》，卷78，第300页(1989年9月)
2、F.Gruenberger，《科学美国人》，“电脑娱乐”，卷250，第10页(1984年4月)
3、John Mosley，《天空和望远镜》，卷81，第201页(1991年2月)
