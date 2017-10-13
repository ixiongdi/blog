今天要解决的是如何用牛顿法求一个数的平方根：

根据牛顿法的定义求![](https://1-im.guokr.com/formula/d6930be70098c14c10f885c780e57279ab91461f.svg)，即求![](https://1-im.guokr.com/formula/edf374d3d7b406ca2d218363e7c3da11db7c407d.svg)的正根。

更一般地，求![](https://3-im.guokr.com/formula/cc869563b8cc6fd2fa23cffa141decad82f25334.svg)，即求![](https://2-im.guokr.com/formula/6a80c4f82cc7a0495bdb18f5e51495162267afee.svg)的正根。

注意牛顿迭代法只能逼近解，不能计算精确解。不过实际应用中，我们都不要求绝对精确的解，例如计算器得出结果也不需要给出无限位，只需要给出十几位小数就足够了，所以牛顿迭代法被广泛用在各种科学计算中。

【牛顿迭代法】

假设方程![](https://3-im.guokr.com/formula/c0da7cf18442d200c0963291fbe1e2f8ffdc4ff5.svg)在![](https://3-im.guokr.com/formula/efbda784ad565c1c5201fdc948a570d0426bc6e6.svg)附近有一个根，那么用以下迭代式子：  
![](https://2-im.guokr.com/formula/aefc36d26e2b81c7381b838b5cbc1f3c5dad173a.svg)  
依次计算![](https://1-im.guokr.com/formula/593f4cff5d4210d46e140db57bafc4f692493f76.svg)、![](https://2-im.guokr.com/formula/a8728ff397f08f1999170f64ff5838333f755380.svg)、![](https://1-im.guokr.com/formula/49510ae6a807501d0723acc06f628c6bbd2f68e2.svg)、……，那么序列将无限逼近方程的根。

牛顿迭代法的原理很简单，其实是根据f\(x\)在x0附近的值和斜率，估计f\(x\)和x轴的交点，看下面的动态图：  
![](http://3.im.guokr.com/Q2Uk_sHeyQSe6VIOp7PnB7PRNYlLd-bHonnaQ2If6O4sAQAA1gAAAEdJ.gif)

【用牛顿迭代法开平方】

令：  
![](https://3-im.guokr.com/formula/1848f3b7365287b7962f2dcaed9ca482438d1c60.svg)  
所以f\(x\)的一次导是：  
![](https://3-im.guokr.com/formula/44df64cc9dc5767483c43d3c46fbd72cb78e7f97.svg)  
牛顿迭代式：  
![](https://1-im.guokr.com/formula/768b411ac2ba3b8e23a2e85f2dfc565ecf2d13b9.svg)

随便一个迭代的初始值，例如![](https://2-im.guokr.com/formula/0c1d7f319728a07a57d000f2379b5215e4130147.svg)，代入上面的式子迭代。

例如计算![](https://1-im.guokr.com/formula/bfe16f27ebc966df6f10ba356a1547b6e7242dd7.svg)，即a=2。  
![](https://2-im.guokr.com/formula/0c1d7f319728a07a57d000f2379b5215e4130147.svg)  
![](https://1-im.guokr.com/formula/c1838533d54bb7d1bf09520443906c90c861cba2.svg)  
![](https://2-im.guokr.com/formula/ade97b4a977688e3486015b4ae430d6c902139b0.svg)  
……

  
计算器上可给出![](https://2-im.guokr.com/formula/477c57e54790c6a773dd634f07f2e8e5486d983e.svg)

【用牛顿迭代法开任意次方】

求![](https://3-im.guokr.com/formula/cc869563b8cc6fd2fa23cffa141decad82f25334.svg)的递推式是：  
![](https://1-im.guokr.com/formula/191287c347892a4751452852d9930d80c8c2c292.svg)



来看看我们的代码实现：

```java
package com.github.xiongdi.study;

public class Calc {
    // 设定一个误差值
    private double err = 0.001;

    public Double sqrt(int a) {
        // 设定一个初始的值
        double x = a / 2;

        // 循环的结束条件是误差小于误差值
        while (Math.abs(x * x - a) > err) {
            // 牛顿迭代式
            x = (x + a / x) / 2;
        }
        return x;
    }
}

```







