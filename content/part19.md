#第19章 包含三个天体的最小圆


  设A、B、C是三个天体，在天球上，它彼此相邻不远，不超过6度。我们希望计算出包含这三个天体的最小圆的角直径。有以下两种情形：

  类型I：最小圆的直径是三角形ABC的最长的边，这里，有一个顶点在圆内。

  类型II：最小圆经过三个顶点A、B、C。



  最小圆的直径Δ可由以下方法得到。

  利用第16章的方法计算出三角形ABC各边的长度(单位是度)。设a是三角形最大的边长，b和c是另外两边的长度。



--------------------------------

例19.a：计算1981年9月11日0h力学时，水星、木星、土星的最小圆直径。此刻这三个行星的位置是：

水星 α = 12h41m08s.63   δ = -5°37'54".2
木星 α = 12h52m05s.21   δ = -4°22'26".2
土星 α = 12h39m28s.11   δ = -1°50'03".7

  利用公式(16.1)得到三个角度差：

水星——木星 3°.00152
水星——土星 3°.82028
木星——土星 4°.04599 = a

  因为 4.04599 < sqrt( (3.00152)2 + (3.82028)2 ) = 4.85836

  所以利用(19.1)式计算Δ，结果是：

  Δ = 4°.26364 = 4°16'

  这个例子属于类型II。

作为一个练习：请计算 1991年6月20日 0h TD时，金星、火星、木星的最小圆直径，使用以下位置数据：

金星 α = 9h05m41s.44   δ = +18°30'30".0
火星 α = 9h09m29s.00   δ = +17°43'56".7
木星 α = 8h59m47s.14   δ = +17°49'36".8

  这题属于类型I，Δ = 2°19'

---------------------------------

  可以写一个程序，利用插值方法得到行星赤经、赤纬，然后得到a、b、c，最后得到Δ。使用这样的程序，也许可能（试验）计算出三行星的Δ的最小值。的确，Δ是随时间变化的，而本章提供的仅是某一时刻的Δ值。

  重要注意：行星的位置可以使用插方法计算，而Δ则不能。原因是，Δ的变化不能表达为一个多项式，详见“例19.c”中的图。(注：Δ的类型随时间推移会发生跳变)。

---------------------------------

例19.b：在1981年9月，有一组三星：水星、木星、土星。位置如下，不提供赤经赤纬，而提供黄经黄纬。



  我们不再给出详细解答，留给读者作为练习。让我们看一下9月7.00到8.81，三体位置属于类型I，最小圆的直径Δ，从7°01'变化到5°00'几乎是线性的减小的。从9月8.81到12.19，三体位置属于类型II，Δ在9月10.53达到最小值4°14'。从9月12.19开始，三体位置又变为类型I，Δ随时间几乎线性增加。

------------------------

例19.c：现在，让我们考虑以下虚构的情况。在3月12.0日，行星的黄道坐标如下(单位是度)：

黄经	黄纬	黄经每日变化
行星P1	214.23	+0.29	+0.11
行星P2	211.79	+0.48	+0.20
行星P3	208.41	+0.75	+1.08
  我们假设黄纬是常数，黄经匀速增加(详见表中最后一列)。

  我们仍把具体计算留读者。我们用下图表示最小圆直径Δ的变化。注意图中的拐点A和B。除了两个很短的时期，A附近(3月15.87到15.91)和B附近(3月17.93到18.05)是类型II，其它的都是类型I。Δ的最小值是1°55'，在图中的B点，是3月17.94日。



  如果其中的一个星体是恒星，须注意：恒星的坐标应与行星的坐标使用同一参考坐标系。(在第16章同已讲到)
