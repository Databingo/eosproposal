
# Dear EOS BPs:
I am a EOS user, I have a report about lost private key. I need you help to frozen then move back my EOS coin. The Situation, Proof and Request as below.
# 1. Situation
My EOS account “gyytsnigenes” was created at EOS launched time. I have about 33620 EOS in it. At Aug 16, 2019, a user “jiawenwallet” created a similar account “gyygsnigenes”(tx1), three minutes later, someone moved my 33620 EOS in to it (tx2). I found this action at Oct 2019, I’m sure someone got my private key because I never do such transfer. I immediately changed the private key of “gyytsnigenes”, then collected other EOS in sub-account “chairmanship”(tx3) and “aiwobabamama”(tx4) to “gyytsnigenes”. Now “gyytsnigenes” is safe with some EOS in it(plus CUP/NET/RAM).
Picture of hacker's action  
(2019.8.16-2019.8.17)  
                                           jiawenwallet  
                                                |  
                                                |Step 1: Create account(Tx1)  
                    Step 2: Move coin(Tx2)      V  
       gyytsnigenes ---------------------> gyygsnigenes  
            /\  
           /  \
aiwobabamama  chairmanship  




# 2. Proofs
- I have the original key pair of “gyytsnigenes”, both owner and active keys.
- I have the key pair of sub-accounts “aiwobabamama” and ”chairmanship” of “gyytsnigenes”. Those sub-accounts were created by myself before “gyytsnigenes" was attacked(tx5, tx6).
- I have done transactions with memo “9da6b5ea943863ce898cd1faf38d46b4” the hash of sentence “I am the owner of gyytsnigenes and aiwobabamama and chairmanship.” from “gyytsnigenes” to “aiwobabamama” to “chairmanship” then to “gyytsnigenes” as proofs that I am the owner(tx7, tx8, tx9).
# 3. Request
I invite you BPs to check this situations, proofs and requests. For EOS holder’s rights, please vote the proposal “anewidentity” to frozen fake account “gyygsnigenes” immediately then change gyygsnigenes’s pub keys to gyytsnigenes’s. The coin belongs to two old men, they work half life’s money.
# 4. Transctions
tx1: 6e72676101976af81abcb8e4801b03ca50c1199e0db36a40f879e9cbe949dbf9  
tx2: 54d8fff2f02257d0b4872394d8673d0a6f8b38669d834b6b7bce281513142804  
tx3: fb78ff4d355cc436226519bb7687df3ca508549f111d9d4c0c9228eaa6a92b8a  
tx4: 3d0a4eed41e1076254e05f1c2f04fd5e5714796c525d6c754c8c3b7ee48eb73b  
tx5: 7d1fdd11f1ebb53c2ed7a86b8af548b2c307c045ad2e652207322cc7742bf0fa  
tx6: 1877ed94dafdeee92a6a912bbc4ad404a0ad2da6ff91ad447a4c7679faf4ba71  
tx7: a1f7489af1a84e4251b88b1cb81a36159b46c90515ca8661d37d5a79549a8337  
tx8: 757e5460eb471d73b67a917c3e55608a576d68d1ad3201230c2345e55ab92348  
tx9: 86c59dd37dbdf90689d7dda24188e565ab286a0111d8f194e8e12aa7c4aeb834  

Best Wishes.  
Ashah Chen   
11/6/2019  


## <p align="center"> ECC 原理推导与代码实现 </p>
#### <p align="center">————如何彻底理解 ECC 加密原理并部署代码</p>
&nbsp;  
&nbsp;  
### <p align="center">摘要</p>
&nbsp;  
&emsp;&emsp;随着比特币的日渐崛起，金融领域掀起了一场思想和技术革命，哈耶克的自由货币思想<sup>1</sup>、去中心化金融（Decentralized Finance)思想、对等商业联盟等思想，伴随着计算机节点共识机制、分布式网络、不对称加密、零知识证明等技术革新组合，衍生出了一个蔚为壮观的变革生产关系的社会实验，这场运动以共识为原则，以去中心化为目的，以分布式技术为工具，宣称一个全新的社会协作时代即将来临，强力解构着传统的金融、商业、组织形式，其中一些大型跨国企业或央行等已经开始或者计划发行自己的数字货币<sup>2</sup>，商业银行也在积极研发自己的区块链技术体系<sup>3</sup>，前瞻的组织都在为这个即将到来的巨大转变做准备。本文试图在数学原理和代码实践的层面上，对比特币等加密货币的价值基础之一：椭圆曲线加密算法（Elliptic Curve Cryptography），进行连续的推导和实现，以确保这个运动的数学基础是坚实可靠并易于理解的。

&emsp;&emsp;【关键词】 ECC、 椭圆曲线、 不对称加密、 数学原理、 编程实现  
&emsp;&emsp;【作者】 陈钢， 中国政法大学硕士 2007 级研究生 
### 1. 导论
&emsp;&emsp;传统上加密算法分为对称加密算法和不对称加密算法，对称算法包括 DES、AES 等，不对称算法包括 RSA、ECC 等, 对称算法需要加解密双方使用同一套秘钥，因此不可避免的存在在秘钥传输过程中泄露的风险，不对称算法使用一对公私钥对，私钥加密，公钥解密，私钥不需要传输，因此不存在传输泄露的风险。不对称算法的基本原理是用私钥推导出对应公钥容易，反推则困难，其中用私钥生成公钥是一个 P 问题<sup>4</sup>， 公私钥解密是一个 NP 问题<sup>5</sup>， 然而逆推，即用公钥逆推出私钥则是一个指数级的难题，即利用现有计算机资源无法在多项式时间内完成的问题，强力计算所需时间通常以亿年为单位，因此被认为是无法破解的。不对称加密算法利用这一特性，保证了密码第一不需要传输，第二极不可能被破解。ECC 椭圆曲线加密算法是其中的一种基于椭圆曲线离散对数难题的不对称加密算法，椭圆曲线在密码学中的使用，是 1985 年由 Neal Koblitz 和 Victor Miller 分别独立提出的。2009 年中本聪设计比特币时，选择了 secp256k1<sup>s</sup> 这条椭圆曲线。
### 2.数学原理
&emsp;&emsp;基于椭圆曲线构造的难题叫做椭圆曲线离散对数难题，即基于椭圆曲线构造一个映射函数，使得在给定一个起点 K 时, 随机次数 k 与终点 P 之间有不可逆推的映射关系。应用在不对称加密中就是使得私钥 k 推导公钥 P 容易，而公钥 P 推导私钥 k 困难。要理解这个不可逆的映射关系，需要借助魏尔斯特拉斯椭圆函数、射影坐标、群论的知识，一步一步构建起这个可去不可回的陷门函数（trapdoor function）。
  
<p align="center"><img src="http://latex.codecogs.com/png.latex?\\secp256k1: y^{2}=x^{3}+x+7" /></p> 


#### 2.1 魏尔斯特拉斯椭圆函数（Weierstrass‘s elliptic function）



众所周知，椭圆是平面上到两个固定点的距离之和为常数的点之轨迹，椭圆方程的标准形式是：
<p align="center"><img src="http://latex.codecogs.com/png.latex?\frac{x^2 }{a^2} + \frac{y^2}{b^2} = 1 (a > 0, b > 0)" /></p>  
<p>推导过程如下： 椭圆的定义是在一个平面内一个动点到两个定点的距离的和等于定长，那么这个动点的轨迹叫做椭圆。设动点为 P(x, y), 两个定点为 F<sub>1</sub>(-c, 0) 和 F<sub>2</sub>(c, 0), 定长为 2a, 根据定义，动点 P 的轨迹方程满足：</p>    
<p align="center"><img src="http://latex.codecogs.com/png.latex?|PF_1|+|PF_2|=2a(a>0)" /></p>   
将两点的距离公式：
<p align="center"><img src="http://latex.codecogs.com/png.latex?|PF_1|=\sqrt{(x+c)^2+y^2},\&space;|PF_2|=\sqrt{(x-c)^2+y^2}" /></p>   
带入其中可得：
<p align="center"><img src="http://latex.codecogs.com/png.latex?\sqrt{(x+c)^2+y^2}=2a-\sqrt{(x-c)^2+y^2}" /></p>  
整理化简可得：   
<p align="center"><img src="http://latex.codecogs.com/png.latex?(a^2-c^2)x^2+a^2y^2=a^2(a^2-c^2)\&space;\textcircled{\scriptsize{1}}" /></p>  
<p>设 a 为长轴长度， b 为短轴长度，则椭圆与 x 轴的右交点到两焦点的距离为：</p>  
<p align="center"><img src="http://latex.codecogs.com/png.latex?(a-c)+(a+c)=2a" /></p>  
<p>由点 <img src="http://latex.codecogs.com/png.latex?(a,0)" />与点<img src="http://latex.codecogs.com/png.latex?(0,b)" /> 到两焦点的距离相等可得：<p>   
<p align="center"><img src="http://latex.codecogs.com/png.latex?(a-c)+(a+c)=2\sqrt{b^2+c^2}" /></p>  
化简可得：   
<p align="center"><img src="http://latex.codecogs.com/png.latex?a^2-c^2=b^2" /></p>  
<p>带入公式<img src="http://latex.codecogs.com/png.latex?\&space;\textcircled{\scriptsize{1}}" />得：</p>  
<p align="center"><img src="http://latex.codecogs.com/png.latex?b^2x^2+a^2y^2=a^2b^2" /></p>  
<p>因为 <img src="http://latex.codecogs.com/png.latex?a^2b^2>0" />, 两边同除以 <img src="http://latex.codecogs.com/png.latex?a^2b^2" /> 得：<p>     
<p align="center"><img src="http://latex.codecogs.com/png.latex?\frac{x^2}{a^2}+\frac{y^2}{b^2}=1" /></p>  
<!-- <p align="center"><img src="ecc.gif" width="20%"/> 甲</p>  -->







在计算椭圆弧长的过程中，数学家们发现无法用显示计算，只能通过数值计算，因此发展出椭圆积分，现代数学将椭圆积分定义为可以表达为如下形式的任何函数 f 的积分：  
<p align="center"><img src="http://latex.codecogs.com/png.latex?f(x)=\int_{c}^{x}R\left [ t,\sqrt{P(t)} \right ]d_{t}" /></p>   
其中 R 是两个参数的有理函数，P 是一个无重根的 3 或 4 阶多项式，而 c 是一个常数。椭圆函数是作为椭圆积分的逆函数被发现的，与 P(t) 表现形式比较相像，通常在求解椭圆周长积分的过程中，通过一定的技巧，总能够得到具有：  
<p align="center"><img src="http://latex.codecogs.com/png.latex?f(x)=\int_{\alpha}^{\beta} {\frac{dx}{\sqrt{x^3+ax+b}}}" /></p>  
<p>的形式的积分项，因此习惯上把具有 <img vertical-align="middle" src="http://latex.codecogs.com/png.latex?\\E:y^2=x^3+ax+b, 4a^3+27b^2 \neq 0" /> 这种方程形式的曲线，叫做椭圆曲线，把这类函数叫做椭圆函数，因为卡尔·魏尔斯特拉斯(Kark Weierstrass)首先研究了这些函数, 因此由魏尔斯特拉斯椭圆积分反演得到的椭圆函数也叫做魏尔斯特拉斯椭圆函数。</p>     


<!--
实轴
连续
不可微（不可导）--微积分
病态函数
光滑函数是指函数各点都有切线，且切线随着切点的移动连续转动（同济《高等数学》）-->
#### 2.2射影平面
#### 2.3阿尔贝群
### 3.代码实现
### 注释
————————————————   
<sup>s</sup> secp256k1 参数定义（Certicom Research, http://www.secg.org/sec2-v2.pdf)   
（原理篇+代码篇）  
$d \mid a$</br>
##基于椭圆曲线离散对数难题的不对称加密算法  
射影平面？  
威尔斯特拉斯方程？!!  
非奇异？  
齐次方程？ 
&emsp;&emsp;椭圆曲线的初等定义是，由维尔斯特拉斯方程（Weierstrass function）确定的平面射影曲线 E，要求曲线上的每个点都是非奇异（或光滑）的，指定无穷远点（0:1:0) ∈ P<sup>2</sup> 为其"零点"。     
Weierstrass 方程仿射部分：<img src="http://latex.codecogs.com/png.latex?\\y^2+a_1xy+a_3y=x^3+a_2x^2+a_4x+a_6" />   
Weierstrass 方程射影版本：<img src="http://latex.codecogs.com/png.latex?\\y^2Z^3+a_1xyZ^3+a_3yZ^3=x^3Z^3+a_2x^2Z^3+a_4xZ^3+a_6Z^3" />    
Let K be a field.  
An elliptic curve E over K is defined by the Weierstrass equation:  
<p align="center"><img src="http://latex.codecogs.com/png.latex?E:y^2+a_1xy+a_3y=x^3+a_2x^2+a_4x+a_6,a_i&space;\in&space;K" /><p align="center">
Special forms:  
<p align="center"><img src="http://latex.codecogs.com/png.latex?K \neq 2,3: y^2=x^3+ax+b,a,b \in&space;K" /><p align="center">
<p align="center"><img src="http://latex.codecogs.com/png.latex?E:y^2+a_1xy+a_3y=x^3+a_2x^2+a_4x+a_6,a_i&space;\in&space;K" /><p align="center">

一条椭圆曲线是在射影平面上满足威尔斯特拉斯方程(Weierstrass)所有点的集合,且曲线上的每个点都是非奇异（或光滑）的。  
<img src="http://latex.codecogs.com/png.latex?\\Y^2Z+a_1XYZ+a_3YZ^2=X^3+a_2X^2Z+a_4XZ^2+a_6Z^3" />--[t1]</br>

- 1.这是一个齐次方程。
- 2.椭圆曲线的形状，并不是椭圆的，只是因为这个方程类似于计算一个椭圆周长的方程，故取名“椭圆曲线”。
标准椭圆曲线方程： 
<img src="http://latex.codecogs.com/png.latex?\200dpi \frac{x^2 }{a^2} + \frac{y^2}{b^2} = 1" />
(a > b, 焦点在 X 轴， a < b, 焦点在 Y 轴)
- 3.非奇异(偏导数)
- 4.椭圆曲线上有一个无穷远点O∞（0:1:0)， 因为这个点满足方程 [t1]
- 5.放到普通平面直角坐标系，加上无穷远点（推导过程,射影平面坐标到普通平面坐标的推导过程）  
对普通平面直角坐标系上的点A的坐标(x,y)做如下改造：
令x=X/Z， y=Y/Z, Z!=0, 则A点可以表示为（X：Y：Z），变成了有三个参量的坐标点，这就对平面上的点建立了一个新的坐标体系。

并得到如下方程：
<img src="http://latex.codecogs.com/png.latex?\200dpi \\y^2Z^3+a_1xyZ^3+a_3yZ^3=x^3Z^3+a_2x^2Z^3+a_4xZ^3+a_6Z^3" />--[t2]  
约掉 Z<sup>3</sup>可以得到：  
<img src="http://latex.codecogs.com/png.latex?\200dpi \\y^2+a_1xy+a_3y=x^3+a_2x^2+a_4x+a_6" />--[t3]  
简化版的Weierstrass方程:  
<img src="http://latex.codecogs.com/png.latex?\200dpi \\E:y^2=x^3+ax+b" />--[t4]  
其中(保证曲线是光滑的，所有点没有两个或者以上的不同的切线):  
<img src="http://latex.codecogs.com/png.latex?\200dpi \\\Delta = -16(4a^3+27b^2)\neq 0" />  
比特币椭圆曲线 secpk1:  
<img src="http://latex.codecogs.com/png.latex?\200dpi \\y^{2}=x^{3}+x+7" />--[t5]  






曲线公式：y^3 + axy^2 + bx^2y = x^3 + cyx^2 + dxy^2(齐次方程)
<img src="http://latex.codecogs.com/png.latex?\200dpi \\y^{3}+axy^{2}+bx^{2}y=x^{3}+cyx^{2}+dy^{2}x" />  
（椭圆曲线周长公式）



其一 : y^2=X^3 + ax +b 
<img src="http://latex.codecogs.com/png.latex?\200dpi \\y^{2}=x^{3}+ax+b" /></br>

secpk1: y^2=X^3 + x + 7
<img src="http://latex.codecogs.com/png.latex?\200dpi \\y^{2}=x^{3}+x+7" /></br>

1.假设 q,p 是曲线 e 上两点，过 qp 的直线与 e 交于 r 点，若 qp 重合，则过 qp 的切线与 e 相交于 r 点
2. 过 r 点与无限远点的直线，与 e 交于 r’ 点
条件：ab…
推理：123

<!--- 
<img src="http://latex.codecogs.com/png.latex?\dpi{200}&space;\bg_grey&space;35*d_1_5_3+1(\oe%20)" />
<img src="http://latex.codecogs.com/png.latex?\200dpi \bg_grey \35*d_1_5_3+1(\oe%20)" />
<img src="http://latex.codecogs.com/png.latex?\200dpi \bg_grey \inline \35*d_1_5_3+1(\oe%20)" />
<img src="http://latex.codecogs.com/png.latex?\200dpi \bg_grey \inline \LARGE \35*d_1_5_3+1(\oe%20)" />
<img src="http://latex.codecogs.com/png.latex?\200dpi \bg_grey \\Y^2Z+a_1XYZ+a_3YZ^2=X^3+a_2X^2Z+a_4XZ^2+a_6Z^3" />--[t1]</br>
(mod\ n)
-->

<!--- 
https://www.codecogs.com/latex/eqneditor.php 语法提示
https://www.desmos.com/calculator 函数绘图
https://rechneronline.de/function-graphs/ 函数绘图

-->
笛卡尔坐标到射影坐标，多了无限远点（平行线相交于无限远点）



假设这样一个关系：q 与 p 确定 r, r 与 无限远点*确定 r’
- 表示关于 x 轴取对称点
q+p = r’
r + * = r’ 或者  r’ + * = r 或者 r + r’ = * 
q +p + r = r’ + r = *
q+p+r=*
符合这样一个规律：(阿尔贝群定理)
1 q + p = r’ 等价 r’ +p’ = q 交换律
2 (q + p) + r = q+ (p + r )=* 结合律
3 q = -(* + q’) 逆元（相反数）
4 q,p 属于 e, 则 q+p属于 e (闭包)
5.q+* = *+ q = q(单位元)
这就是一个符合加法规律的集合。


模运算，使图像从平滑曲线变为离散的点集合

那么假设在曲线 y^2=X^3 + x + 7 上
kp = p + p+ p +…p=K
知道 k,p 计算 kp 有简单算法..
知道 K,p 计算 k 只能逐步计算，设 k= 2^256 则为 NP-hard 难题，视为无法完成。

加解密原理：
c1, c2..
实际实现代码：信息是如何加密和被解密的。




椭圆曲线Ep(a,b), p为大质数，x,y属于[0,p-1]
y^2=x^3 + ax +b (mod p)
选择两个满足下列约束条件的小于 p 的非负整数a、b
4a^3 + 27b^2 != 0 (mod p) [why?]保证曲线是光滑的，所有点没有两个或者以上的不同的切线？
<img src="http://latex.codecogs.com/png.latex?\200dpi \\4a^{3}+27b^{2} \neq 0\ (mod\ p)" /></br>
P(x1,y1), Q(x2,y2)的和R(x3,y3)有如下关系：
x3=k^2 -x1-x2(mod p)
y3 =k(x1-x3)-y1(mod p)
斜率 
k=(3x2 + a)/2y1 [P=Q]
k=(y2-y1)/(x2-x1)



K=kG, k<n (G 为椭圆曲线Ep(a，b)上的点，n为G的阶，即 nG=*
点G称作基点(base point)
k(k<n)为私有密钥(private key)
K为公开密钥(public key)
