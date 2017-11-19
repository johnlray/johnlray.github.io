---
title: "On Reverse Coattails in the 2017 Virginia Election"
author: "John Ray"
date: "November 17, 2017"
layout: post

output: 
  md_document:
    variant: markdown_github
---

Here, I present some data on the question of a possible coattail effect in the 2017 Virginia elections. I devise counterfactual predictions of Northam performance based on HoD district demographics, electoral history, and a combined model including both demographic and political fundamentals. I find reason to express some skepticism of reverse coattails: Northam slightly overperformed in districts that had a challenger in 2017 but not in 2013, but overperformed to a higher degree in districts with no Dem Delegate on the ballot. This is in general agreement with other work suggesting red suburbs turned against Gillespie, but did so without the aid of a stellar HoD candidate.

These findings are strongly contrary to my own priors and to my normative hopes for the value of stellar local candidates. I include replication data and welcome scrutiny and feedback.

"Reverse coattails" refers to the possibility that a candidate at a lower level of the ballot influenced the performance of candidates higher on the ballot. A natural approach to modeling reverse coattails is to ask, "how would we expect Northam to perform at the HoD level if we didn't know anything about the HoD candidates - that is, if we assumed HoD candidate quality was uncorrelated with Northam performance?" Then, with an expectation for Northam's performance in mind based on some (hopefully) reasonable assumptions *besides* HoD candidate quality, we can look at Northam's over- or underperformance relative to our expectations at House district level and, from there, do some additional work to figure out if Northam's deviation from what we expect can be attributed to the House of Delegates candidate running in that district.

To construct the counterfactual I first ignore HoD candidate factors and use other reasonable measures to set up our expectations for Northam's performance. Using historical data, I devised three counterfactual scenarios against which I modeled the real 2017 outcomes. Those three counterfactual scenarios are based on worlds where we predict 2017 Democratic Gubernatorial performance using

1.  *Political fundamentals matter most:* Presidential election data.
2.  *Demographic fundamentals matter most:* The race, ethnicity, education, and income composition of the House districts.
3.  *Both political and demographic fundamentals matter:* The whole kitchen sink.

For the purposes of brevity, I present the results of the third approach, and leave data available for others to tinker with. I did not find major differences between the three approaches. Here, I construct the counterfactual scenarios using the prior gubernatorial election (2013), and forecast the results over the 2017 gubernatorial election.

These three counterfactual scenarios are operationalized in the following three OLS models. In each model, the dependent variable of interest, *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup>, is the Democratic two-party vote share in the Gubernatorial election, aggregated at the level of the Virginia House of Delegates.

1.  *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup> ∼ *β*<sub>0</sub> + *β*<sub>1</sub>*V*<sub>*D**e**m*</sub><sup>*P**r**es*</sup> + *ϵ*

2.  *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup> ∼ *β*<sub>0</sub> + *β*<sub>1</sub>*B**l**a**c**k*%+*β*<sub>2</sub>*L**a**t**i**n**o**+**β**<sub>3</sub>*H**H**i**n**c**o**m**e** + *β*<sub>4</sub>*B**a**c**h**e**l**o**r**s**+**ϵ*

3.  *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup> ∼ *β*<sub>0</sub> + *β*<sub>1</sub>*V*<sub>*D**e**m*</sub><sup>*P**r**es*</sup> + *β*<sub>1</sub>*B**l**a**c**k*%+*β*<sub>2</sub>*L**a**t**i**n**o**+**β**<sub>3</sub>*H**H**i**n**c**o**m**e** + *β*<sub>4</sub>*B**a**c**h**e**l**o**r**s**+** + *ϵ

For each equation, the dependent variable is Northam two-party vote share at the House of Delegates district level. For the first equation, the independent variable is Presidential vote share in the nearst Presidential election to the state election, with Presidential election results aggregated at the House of Delegates district level. For the second equation, the independent variables are the House of Delegates districts' percent of the population that is black, the percent that is Latino, the district median household income in dollars, and the percent of the district aged twenty-five and over that has at least a bachelors' degree. Each of those variables are taken from the most recent American Community Survey to the gubernatorial election.

The following table plots the results of the models built on 2013 American Community Survey data, 2013 gubernatorial election data, and 2012 Presidential election data. In the first model, a 1% increase in Democratic Presidential vote share in 2012 is associated with a 1.11% increase in 2013 Democratic gubernatorial vote share, while a 1% increase in Dem Pres vote share in 2012 is associated with a 0.87% Democratic gubernatorial vote share increase in the kitchen sink model. The demographic fundamentals behave in the direction one might expect: a higher black population percent and Latino population percent are both positively correlated with a higher Democratic gubernatorial vote share, while household income is negatively correlated with Democratic gubernatorial vote share and the percent of district 25+ population holding at least a bachelors degree is positively correlated with Democratic gubernatorial vote share.

<table cellspacing="0" align="center" style="border: none;">
<caption align="bottom" style="margin-top:0.3em;">
2013 models
</caption>
<tr>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;">
<b></b>
</th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;">
<b>Political fundamentals</b>
</th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;">
<b>Demographic fundamentals</b>
</th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;">
<b>Kitchen sink</b>
</th>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Intercept
</td>
<td style="padding-right: 12px; border: none;">
-0.01
</td>
<td style="padding-right: 12px; border: none;">
0.10<sup style="vertical-align: 0px;">***</sup>
</td>
<td style="padding-right: 12px; border: none;">
-0.07<sup style="vertical-align: 0px;">***</sup>
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.03)
</td>
<td style="padding-right: 12px; border: none;">
(0.02)
</td>
<td style="padding-right: 12px; border: none;">
(0.02)
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Dem Presidential
</td>
<td style="padding-right: 12px; border: none;">
1.02<sup style="vertical-align: 0px;">***</sup>
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
0.86<sup style="vertical-align: 0px;">***</sup>
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.06)
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.05)
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Dem House of Delegates
</td>
<td style="padding-right: 12px; border: none;">
0.04
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
0.00
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.02)
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.01)
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Black %
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
0.83<sup style="vertical-align: 0px;">***</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.15<sup style="vertical-align: 0px;">***</sup>
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.03)
</td>
<td style="padding-right: 12px; border: none;">
(0.04)
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Latino %
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
0.91<sup style="vertical-align: 0px;">***</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.18<sup style="vertical-align: 0px;">***</sup>
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.08)
</td>
<td style="padding-right: 12px; border: none;">
(0.05)
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Household income
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
-0.00<sup style="vertical-align: 0px;">***</sup>
</td>
<td style="padding-right: 12px; border: none;">
-0.00<sup style="vertical-align: 0px;">*</sup>
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.00)
</td>
<td style="padding-right: 12px; border: none;">
(0.00)
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Bachelors %
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
0.95<sup style="vertical-align: 0px;">***</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.38<sup style="vertical-align: 0px;">***</sup>
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
(0.06)
</td>
<td style="padding-right: 12px; border: none;">
(0.04)
</td>
</tr>
<tr>
<td style="border-top: 1px solid black;">
R<sup style="vertical-align: 0px;">2</sup>
</td>
<td style="border-top: 1px solid black;">
0.93
</td>
<td style="border-top: 1px solid black;">
0.93
</td>
<td style="border-top: 1px solid black;">
0.99
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Adj. R<sup style="vertical-align: 0px;">2</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.93
</td>
<td style="padding-right: 12px; border: none;">
0.93
</td>
<td style="padding-right: 12px; border: none;">
0.99
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">
Num. obs.
</td>
<td style="padding-right: 12px; border: none;">
100
</td>
<td style="padding-right: 12px; border: none;">
100
</td>
<td style="padding-right: 12px; border: none;">
100
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid black;">
RMSE
</td>
<td style="border-bottom: 2px solid black;">
0.05
</td>
<td style="border-bottom: 2px solid black;">
0.05
</td>
<td style="border-bottom: 2px solid black;">
0.02
</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;" colspan="5">
<span style="font-size:0.8em"><sup style="vertical-align: 0px;">***</sup>p &lt; 0.001, <sup style="vertical-align: 0px;">**</sup>p &lt; 0.01, <sup style="vertical-align: 0px;">*</sup>p &lt; 0.05</span>
</td>
</tr>
</table>
Next, I forecast the 2017 election results using these models. I update the historical data on the righthand side of equations one, two, and three with 2016 Presidential election and 2015 American Community Survey data at the level of Virginia House of Delegates districts. The following chart plots predicted 2017 Northam two-party vote share by district vs. actual 2017 Northam two-party vote share, using the combined model. Values above the dotted gray line indicate districts where Northam *over*-performed the prediction from the model, and values below the dotted gray line indicate districts where Northam *under*-performed the prediction from the model.

I color each district by whether it had a Dem candidate on the HoD ballot in both, neither, or one or the cycles.

![](figure/plots-1.png)

The following plot shows actual minus predicted Northam vote share for each district, for the combined model.

![](figure/diff-1.png)

The following tables present the top ten districts where Northam overperformed the various models, according to each model individually.

|     |  District|  Prediction|     Actual|  Difference|
|-----|---------:|-----------:|----------:|-----------:|
| 203 |         3|   0.1029476|  0.2623563|   0.1594087|
| 206 |         6|   0.1678032|  0.3128139|   0.1450108|
| 204 |         4|   0.1806931|  0.3161455|   0.1354525|
| 205 |         5|   0.1618378|  0.2952673|   0.1334296|
| 201 |         1|   0.1251723|  0.2582095|   0.1330372|
| 215 |        15|   0.2095640|  0.3279961|   0.1184321|
| 209 |         9|   0.2289975|  0.3157480|   0.0867504|
| 219 |        19|   0.2351518|  0.3194078|   0.0842560|
| 224 |        24|   0.2791547|  0.3627521|   0.0835974|
| 298 |        98|   0.3070653|  0.3870815|   0.0800162|

![](figure/unnamed-chunk-2-1.png)

![](figure/unnamed-chunk-3-1.png)

Presented in this fashion, I am led by the data to report some skepticism of the reverse coattails effect. Foroemost in my mind is the fact that Danica Roem's district (VA-13) is not prominent according to the model as a standout showing for Northam above what one might predict from just knowing the fundamentals. In fact, in some of the top districts according to each model (1, 3, 4, 6), Northam drastically overperformed despite either a decisive loss by the House of Delegates candidate or the complete absence of a House of Delegates Democrat. In VA-2, Northam overperformed in a district where the Republican primary winner dropped out amid accusations he falsified his ballot signatures.

If one defines the coattails effect as a superstar HoD candidate elevating the Gubernatorial candidates, the results are mixed at best. In some districts, Northam appeared to overperform the prediction without any Democrat on the HoD ballot at all. In the HoD districts of Democratic superstars like Chris Hurst, Wendy Gooditis, and Danica Roem (10, 12, and 13), Northam slightly overperformed, slightly overperformed, and underperformed, respectively.

And [here](data/va_long.csv)s the data I used for this project.