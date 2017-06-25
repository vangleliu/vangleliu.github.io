---
layout:            post
title:             "AndroidShake--a simple demo of sensor usage in Android"
menutitle:         "AndroidShake--a simple demo of sensor usage in Android"
category:          Programming
author:            vangle
tags:              Android demo
---

最近为了给工科创课程写指导书，写了一个关于如何使用安卓上传感器的demo。

功能：使用加速度传感器，检测用户“摇一摇”的动作，用户每摇一次手机，counter的数值加一。

用户在摇手机时，手机加速度的pattern如下：

![My helpful screenshot](/assets/shakepattern.png)

检测“摇一摇”动作的代码如下：

{% highlight java %}
if(acc < 15 && timeout2 == 0 && timeout1 == 0)
{
    lowcount = lowcount+1;
}

if(lowcount > 15 && timeout2 == 0 && timeout1 == 0)
{
    if(acc > 15) {
        timeout1 = 15;
        lowcount = 0;
    }
}

if(timeout1 > 0)
{
    if(acc < 15) {
        timeout2 = 15;
        timeout1 = 0;
    }
    else{
        timeout1--;
    }
}

if(timeout2 > 0)
{
    if(acc > 15){
        ShakeCount++;
        timeout2 = 0;
    }
    else{
        timeout2--;
    }
}
{% endhighlight %}

大致流程如下：
1.	在一段时间内加速度较低，认为是平静期（平静期用于区分两次“摇一摇”）。
2.	在平静期过后，如果加速度达某一阈值，则认为“摇一摇”开始，设置一个timer，在timer耗尽之前，如果加速度再次回到较低值，则设置另一个timer。
3.	如果第二个timer耗尽之前加速度再次达到较高值，则认为完成了一次“摇一摇”（加速度pattern：高-低-高），counter加1，并回到1状态。

完整代码见https://github.com/vangleliu/AndroidShake


