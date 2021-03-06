# 第23章 轨道要素在不同坐标中的转换


  在处理某些问题时，可能需要对主行星或慧星的轨道要素进行转换，从一个分点坐标中转到另一个坐标中。当然，转换过程中，半长轴及离心率是不会变化的，因此中有三个要素需要转换：
    i = 轨道倾角
    ω= 近点参数
    Ω= 升交点经度
  设io、ωo、Ω是已知的，它是初始历元的轨道要素，设i、ω、Ω是未知的，它是终止历元的轨道要素。
  在下图中，Eo和λo分别是初始历元的黄道和春风点，E和λ则是终止历元的黄道及分点。这两个黄道的夹角是η，轨道的近点是P。




  同第20章一样，T是J2000.0起算的初历元的儒略世纪数，t则是从初始历元起算的终历元的儒略世纪数。
  接下来计算角度η、П和p，这3个角用20.5式计算，如果初始历元是J2000.0，则可以用20.6式计算。
  令ψ = П+p，则i、Ω（即先求得Ω-ψ，再得到Ω）的值可由下式算出：
  
    cos(i) = cos(io)*cos(η) + sin(io)*sin(η)*cos(Ωo-П) ……23.1式
    sin(i)*sin(Ω-ψ) = sin(io)*sin(Ωo-П) ……23.2式
    sin(i)*cos(Ω-ψ) = -sin(η)*cos(io) + cos(η)*sin(io)*cos(Ωo-П)
  当倾角很小时，23.1式不能使用。
  接下来：ω = ωo + Δω，式中Δω由下式计算：
  
    sin(i)*sin(Δω) = -sin(η)*sin(Ωo-П) ……23.3式
    sin(i)*cos(Δω) = sin(io)*cos(η) - sin(io)*sin(η)*cos(Ωo-П)
  如果io=0，那么Ωo是不确定的，这时有i=0及Ω=ψ+180°。
  要注意的是，用以上描述的方法进行转换，到新历元坐标的i、ω、Ω，它仍然是有效的初始要素。不过，事实上，换个新的历元坐标计算同一条轨道是完全不同的天体力学问题。
  例23.a ——在他们的星库中(巴黎天文台天体物理de Meudon 1952)，De Obaldia 给出了Klinkenberg(1744)慧星的轨道要素,B1744.0平分点坐标：
  
    io = 47°.1220
    ωo=151°.4486
    Ωo= 45°.7481
  请把这些要素转到B1950.0标准分点坐标中。
  解：最后历元是B1950.0，或(JD) = 2433282.4235(详见第20章),而初历元比它早206个太阳年（在为这两个历元都是贝塞尔年首），因此：
    (JD)o = 2433282.4235 - (206*365.2421988) = 2358042.5305。
  则有：
  
    T = -2.559958097
    t = +2.059956002
    η= +97".0341 = +0°.026954
    П= 174°.876384 - 10205".9108 = 172°.041409
    P = +10352".7137 = 2°.875754
    ψ= 174°.917163
  那么，由式23.2得：
  
    sin(i)*sin(Ω-ψ) = -0.59063831 = A
    sin(i)*cos(Ω-ψ) = -0.43408084 = B
  使用以下变换：
  
    sin(i) = sqrt(A*A+B*B) = 0.73299372, i = 47°.1380
    Ω-ψ = ATN2(A,B) = -126°.313473
    Ω = 48°.6037
  由23.3式得：
  
    sin(i)*sin(Δω) = +0.00037917
    sin(i)*cos(Δω) = +0.73299362
  因此：Δω = +0°.0296， ω=151°.4782
  在他的慧星轨道库中(第六版;1989),Marsden给出的值是:
  
    i = 47°.1378,
    ω= 151°.4783
    Ω= 48°.6030
  这与上述计算的Ω存在0°.0007的差异，原因是新的IAU比普通岁差多出一出了一个被Newcomb忽略的黄经的小量(+1".1每世纪)。这造成206年(从1744到1950)积累了0.0006度。
-----------------------
  如果初始历元分点坐标是B1950.0，终止历元坐标是J2000.0，那么公式简化为：
  以下几个式子计为 ……23.4式
  
    S = 0.0001139788 C = 0.9999999935
    W = Ω。- 174°.298782
    A = sin(io)*sin(W)
    B = C*sin(io)*cos(W) - S*cos(io)
    sin(i) = sqrt(A*A+B*B) tan(x) = A/B
    Ω= 174°.997194 + x
    最后 ω = ωo + Δω
    式中tan(Δω) = -S*sin(W) / (C*sin(io) - S*cos(io)*cos(W))
  必须注意角度x与角度Δω的象限。为了安全，可使用ATN2()函数，即x=ATN2(A,B)。此外，当轨道倾角很小时，新的Ω应比初始Ωo大0.7°,Δω应接近0°而不是180°。
-----------------------
  例23.b ——S.Nakano 计算出周期慧星1990返回（小行星通告12577），轨道如下：
  
    历元 = 1990年12月5.0(TD) = JDE 2448200.5
    T = 1990年10月28.54502 TD
    B1950.0要素
    q = 0.3308858 i = 11°.93911
    a = 2.2091404 Ω=334°.04096
    e = 0.8502196 ω=186°.24444
  我们希望将i、Ω、ω转到J2000.0分点坐标中，有以下计算过程：
  
    W = +159°.742178
    A = +0.0716284465
    B = -0.1941873149
    sin(i)= 0.2069767   Ω= 334°.75006
    i = 11°.94524     Δω= -0°.01092
    x = +159°.752866    ω= 186°.23352
  其它的轨道要素(T,q,a,e)保持不变,历元还是1900年12月5.0日
------------------
  然而,23.4式采用的要素io、ωo及Ωo是FK5系统的。把这些要素从B1950.0/FK4转到J2000.0/FK5系统，可以使用Yeomans的如下算法:
  设:
  
    L'= 4.50001688度
    L = 5.19856209度
    J = 0.00651966度
    W = L+Ωo
  然后有：
  
    sin(ω-ωo)*sin(i) = sin(J) *sin(W)
    cos(ω-ωo)*sin(i) = sin(io)*cos(J) + cos(io)*sin(J)*cos(W)
    cos(i) = cos(io)*cos(J) - sin(io)*sin(J)*cos(W)
    sin(L'+Ω) *sin(i) = sin(io)*sin(W)
    cos(L'+Ω) *sin(i) = cos(io)*sin(J) + sin(io)*cos(J)*cos(W)
  从以上算式可算出i、Ω及ω
----------------
  例23.c ——io、Ωo及ωo的初值与例23.b的相同。
  我们得到：
  
    FK5,J2000.0
    i = 11°.94521
    Ω= 334°.75043
    ω= 186°.23327

