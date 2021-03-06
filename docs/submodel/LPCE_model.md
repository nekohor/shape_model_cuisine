# LPCE横向带钢模型

LPCE模块可以说是SSU中最简单的模块了。

本模块主要用于计算以下三个参数。

- 带钢弹性模量elas_modu
- 应力释放系数strn_rlf_cof
- 屈曲判别极限Crit_Bckl_Lim

## 插值计算elas_modu和strn_rlf_cof

带钢弹性模量和应力释放系数主要通过插值计算的方式获得。

利用各个机架的出口温度，和带钢弹性模量和应力释放系数的插值表进行插值计算，获得各个机架的带钢弹性模量和应力释放系数。

一般插值表如下，很少对此插值表进行改动。

| avg_pce_tmp_interp_vec | elas_modu_interp_vec | strn_rlf_cof_interp_vec |
| ---------------------- | -------------------- | ----------------------- |
| 600                    | 138269               | 0                       |
| 650                    | 128069               | 0                       |
| 700                    | 117905               | 0                       |
| 750                    | 107751               | 0.056                   |
| 800                    | 97589                | 0.091                   |
| 850                    | 87415                | 0.16                    |
| 900                    | 77232                | 0.303                   |
| 950                    | 67054                | 0.521                   |
| 1000                   | 56909                | 0.771                   |
| 1050                   | 46829                | 0.968                   |
| 1100                   | 36863                | 0.984                   |
| 1500                   | 27067                | 0.984                   |

## 屈曲判别极限的计算

屈曲判别极限分为边浪的判别极限和中浪的判别极限。各个机架的屈曲判别极限和带钢的出口厚度、出口宽度、纵向平均张力、弹性模量有关，根据以下经验公式进行计算。
$$
\epsilon_{we} = 80\cdot (\frac{h_{ex}}{B_{ex}})^2 + \sigma_{coef\_we}\cdot \frac{\sigma_{t}}{E_{mod}}
$$

$$
\epsilon_{cb} = -40\cdot (\frac{h_{ex}}{B_{ex}})^2 + \sigma_{coef\_cb}\cdot \frac{\sigma_{t}}{E_{mod}}
$$

$$
\sigma_{coef\_we} = 1.5
$$

$$
\sigma_{coef\_cb} = -3
$$

## 屈曲判别极限的调节参数

CTOOL中GSM模块中有CenterBuckleTuning和CenterBuckleTuning两张表，这两张表可以用来对屈曲判别标准的中浪和双边浪极限值进行调整。

这两个表对于每个机架F1到F7分别有两个参数。一个是比例系数multiplier，作为乘数而存在，另一个是补偿值Offset，作为加数而存在。