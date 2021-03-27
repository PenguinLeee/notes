## 逐步回归和Lasso回归

> 姓名：李启闻	
>
> 学号：3120201453

#### 0x01 数据来源&背景

太阳能属于清洁能源，不产生任何污染物质，因此国内外都非常重视相关技术和产品的开发。不过太阳能的利用有一个很大的问题，那就是很容易受到天气的影响，导致实际发电功率时刻都在变化，这在很大程度上影响了光伏发电的大规模应用。

为了改善光伏发电的实用效能，我们可以考虑不同天气情况对光伏设备发电功率的影响，然后就可以根据天气预报数据，预测未来一段时间光伏设备的发电功率，便于电网的集中调度和发电计划的制定，预测的越准确越有益于电网的稳定和安全运行。

数据取自某地一台10MW光伏发电机工作时记录的自然条件以及输出功率，记录频率为15分钟，一共记录了2443条数据。部分数据如下：

![数据](C:\Users\Lenovo\Desktop\数据.JPG)

光伏发电机运行时，共选取辐照度、实际辐照度、风速、风向、温度、气压、湿度七个指标。以上数据均在发电机工作时完成采集，因此没有夜间数据。我们的目标是使用上述指标来估计发电机的输出功率。

#### 0x02 描述性统计

```R
> summary(dat)
 Radioactivity      ActualRadioactivity  WindVelocity     WindDirection  
 Min.   :-0.89924   Min.   :0.1008      Min.   :-1.0000   Min.   :  0.0  
 1st Qu.:-0.56226   1st Qu.:0.4377      1st Qu.:-0.9151   1st Qu.: 97.0  
 Median :-0.20913   Median :0.7909      Median :-0.8491   Median :129.0  
 Mean   :-0.23161   Mean   :0.7684      Mean   :-0.7985   Mean   :164.6  
 3rd Qu.: 0.08365   3rd Qu.:1.0837      3rd Qu.:-0.7358   3rd Qu.:267.0  
 Max.   : 0.58745   Max.   :1.5875      Max.   : 0.4151   Max.   :358.0  
  Tempreature         Pressure           Humidity           Power      
 Min.   :-0.8222   Min.   :-0.81818   Min.   :-0.8333   Min.   :0.000  
 1st Qu.:-0.4303   1st Qu.:-0.09091   1st Qu.:-0.6667   1st Qu.:2.233  
 Median :-0.2970   Median : 0.09091   Median :-0.5417   Median :5.167  
 Mean   :-0.3028   Mean   : 0.07148   Mean   :-0.4641   Mean   :4.499  
 3rd Qu.:-0.1848   3rd Qu.: 0.27273   3rd Qu.:-0.3333   3rd Qu.:6.700  
 Max.   : 0.1838   Max.   : 0.69697   Max.   : 0.6042   Max.   :8.200  
```

对各个指标进行初步分析可以发现，在每个采样时刻中，Radioactivity和ActualRadioactivity指标都有1的差值，ActualRadioactivity的最小值和最大值分别为0.1008和1.5875，分别对应着清早（黄昏）和正午的光照量；从数据的各个分位数来看，采样时间比较均匀。风向应该是以角度计算，各个方向的气流都存在；气温普遍不高，仅有少数时刻的气温突破0度；气压和湿度总体波动不大。



#### 0x03 逐步回归

在进行回归之前，我们先考察数据的多重共线性性。数据集的条件数为3468.318，说米国本数据集具有较高的多重共线性。

下面，我们使用AIC准则进行变量筛选和逐步回归：

```R
Start:  AIC=2540.1
Power ~ Radioactivity + ActualRadioactivity + WindVelocity + 
    WindDirection + Tempreature + Pressure + Humidity


Step:  AIC=2540.1
Power ~ Radioactivity + WindVelocity + WindDirection + Tempreature + 
    Pressure + Humidity

                Df Sum of Sq     RSS    AIC
- WindDirection  1       0.1  6870.7 2538.1
- Pressure       1       1.9  6872.5 2538.8
<none>                        6870.6 2540.1
- WindVelocity   1     118.2  6988.9 2579.8
- Humidity       1     504.3  7374.9 2711.1
- Tempreature    1    1037.3  7907.9 2881.5
- Radioactivity  1    5948.3 12818.9 4061.1

Step:  AIC=2538.13
Power ~ Radioactivity + WindVelocity + Tempreature + Pressure + 
    Humidity

                Df Sum of Sq     RSS    AIC
- Pressure       1       2.1  6872.9 2536.9
<none>                        6870.7 2538.1
- WindVelocity   1     132.8  7003.5 2582.9
- Humidity       1     537.3  7408.0 2720.0
- Tempreature    1    1095.3  7966.0 2897.3
- Radioactivity  1    6005.8 12876.5 4070.0

Step:  AIC=2536.89
Power ~ Radioactivity + WindVelocity + Tempreature + Humidity

                Df Sum of Sq     RSS    AIC
<none>                        6872.9 2536.9
- WindVelocity   1     136.0  7008.8 2582.7
- Humidity       1     555.4  7428.3 2724.7
- Tempreature    1    1440.1  8313.0 2999.4
- Radioactivity  1    6149.4 13022.3 4095.5

```

我们可以看到，最后被选中作为自变量的仅有照度、风俗、温度和湿度，压强、风向和实际辐照度被剔除。

针对AIC准则筛选出的四个变量进行线性回归，可得：

```R
Residuals:
    Min      1Q  Median      3Q     Max 
-6.4565 -1.1712  0.0688  1.2497  4.2033 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)     2.0318     0.1877  10.823  < 2e-16 ***
Radioactivity   4.7185     0.1010  46.696  < 2e-16 ***
WindVelocity   -1.2405     0.1787  -6.943 4.89e-12 ***
Tempreature    -5.0707     0.2244 -22.597  < 2e-16 ***
Humidity       -2.2278     0.1587 -14.034  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.679 on 2437 degrees of freedom
Multiple R-squared:  0.5622,    Adjusted R-squared:  0.5615 
F-statistic: 782.3 on 4 and 2437 DF,  p-value: < 2.2e-16
```

可以发现，上述四个变量的显著性都较强，且从残差的描述性统计来看，其分布比较均匀。

但是R2的值较小，仅有0.56左右，说明实际情况中存在着比较多的非线性成分。



实际上，我们作出残差的分布图，可以看出数据具有非常明显的非正态性。

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210321193123234.png" alt="23" style="zoom:50%;" />

进行S-W正态性检验可得：

```R
Shapiro-Wilk normality test

data:  model.step$res
W = 0.98748, p-value = 8.123e-14
```

这证明，残差大概率不符合正态分布。



#### 0x04 Lasso回归

下面进行Lasso回归。

Lasso回归的结果如下：

```R
  Df     Rss        Cp
0  1 15698.1 3125.7871
1  1  9057.1  771.2170
2  2  8536.7  588.6951
3  3  8462.2  564.2725
4  4  6875.2    3.5978
5  5  6871.2    4.2066
6  6  6870.6    6.0000
```

上述Lasso回归的结果中选择了具有最小Cp值的模型，此时多重共线性最小。

```R
  Radioactivity ActualRadioactivity        WindVelocity 
     4.70882438          0.00000000         -1.21320532 
  WindDirection         Tempreature            Pressure 
     0.00000000         -5.05928975         -0.06369102 
       Humidity 
    -2.20710103 
```
可以发现，Lasso回归选择了五个变量：辐照度、风速、温度、气压和湿度。



#### 0x05 源代码


```R
library(lars)
library(xlsx)
dat = read.xlsx("data.xlsx",sheetName="Sheet1",header=T,encoding="UTF-8")
summary(dat)
kappa(dat)

# Stepwise
model.step = step(lm(Power~., data = dat))
summary(model.step)
plot(model.step$fit,model.step$res) 
shapiro.test(model.step$res)

# Lasso
dat = as.matrix(dat)
x = dat[, 1:7]
y = dat[,8]
model.lasso = lars(x,y)
plot(model.lasso)
summary(model.lasso)

cv.model.lasso=cv.lars(x,y,K=10)
select=cv.model.lasso$index[which.min(cv.model.lasso$cv)]
coef=coef.lars(model.lasso,mode="fraction",s=select)
coef[which(coef!=0)] 
````
