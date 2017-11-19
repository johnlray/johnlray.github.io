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

1.  *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup> ∼ *β*<sub>0</sub> + *β*<sub>1</sub>*V*<sub>*D**e**m*</sub><sup>*P**r**e**s*</sup> + *ϵ*

2.  *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup> ∼ *β*<sub>0</sub> + *β*<sub>1</sub>*B**l**a**c**k*%+*β*<sub>2</sub>*L**a**t**i**n**o*%+*β*<sub>3</sub>*H**H**i**n**c**o**m**e* + *β*<sub>4</sub>*B**a**c**h**e**l**o**r**s*%+*ϵ*

3.  *V*<sub>*D**e**m*</sub><sup>*G**o**v*</sup> ∼ *β*<sub>0</sub> + *β*<sub>1</sub>*V*<sub>*D**e**m*</sub><sup>*P**r**e**s*</sup> + *β*<sub>3</sub>*B**l**a**c**k*%+*β*<sub>4</sub>*L**a**t**i**n**o*%+*β*<sub>5</sub>*H**H**i**n**c**o**m**e* + *β*<sub>6</sub>*B**a**c**h**e**l**o**r**s*%+*ϵ*

For each equation, the dependent variable is Northam two-party vote share at the House of Delegates district level. For the first equation, the independent variable is Presidential vote share in the nearst Presidential election to the state election, with Presidential election results aggregated at the House of Delegates district level. For the second equation, the independent variables are the House of Delegates districts' percent of the population that is black, the percent that is Latino, the district median household income in dollars, and the percent of the district aged twenty-five and over that has at least a bachelors' degree. Each of those variables are taken from the most recent American Community Survey to the gubernatorial election.

The following table plots the results of the models built on 2013 American Community Survey data, 2013 gubernatorial election data, and 2012 Presidential election data. In the first model, a 1% increase in Democratic Presidential vote share in 2012 is associated with a 1.11% increase in 2013 Democratic gubernatorial vote share, while a 1% increase in Dem Pres vote share in 2012 is associated with a 0.87% Democratic gubernatorial vote share increase in the kitchen sink model. The demographic fundamentals behave in the direction one might expect: a higher black population percent and Latino population percent are both positively correlated with a higher Democratic gubernatorial vote share, while household income is negatively correlated with Democratic gubernatorial vote share and the percent of district 25+ population holding at least a bachelors degree is positively correlated with Democratic gubernatorial vote share.

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
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
0.10<sup style="vertical-align: 0px;">\*\*\*</sup>
</td>
<td style="padding-right: 12px; border: none;">
-0.07<sup style="vertical-align: 0px;">\*\*\*</sup>
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
1.02<sup style="vertical-align: 0px;">\*\*\*</sup>
</td>
<td style="padding-right: 12px; border: none;">
</td>
<td style="padding-right: 12px; border: none;">
0.86<sup style="vertical-align: 0px;">\*\*\*</sup>
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
0.83<sup style="vertical-align: 0px;">\*\*\*</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.15<sup style="vertical-align: 0px;">\*\*\*</sup>
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
0.91<sup style="vertical-align: 0px;">\*\*\*</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.18<sup style="vertical-align: 0px;">\*\*\*</sup>
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
-0.00<sup style="vertical-align: 0px;">\*\*\*</sup>
</td>
<td style="padding-right: 12px; border: none;">
-0.00<sup style="vertical-align: 0px;">\*</sup>
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
0.95<sup style="vertical-align: 0px;">\*\*\*</sup>
</td>
<td style="padding-right: 12px; border: none;">
0.38<sup style="vertical-align: 0px;">\*\*\*</sup>
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

![](2017/11/19/figure/plots-1.png)

The following plot shows actual minus predicted Northam vote share for each district, for the combined model.

![](2017/11/19/figure/diff-1.png)

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

![](2017/11/19/figure/unnamed-chunk-2-1.png)

![](2017/11/19/figure/unnamed-chunk-3-1.png)

Presented in this fashion, I am led by the data to report some skepticism of the reverse coattails effect. Foroemost in my mind is the fact that Danica Roem's district (VA-13) is not prominent according to the model as a standout showing for Northam above what one might predict from just knowing the fundamentals. In fact, in some of the top districts according to each model (1, 3, 4, 6), Northam drastically overperformed despite either a decisive loss by the House of Delegates candidate or the complete absence of a House of Delegates Democrat. In VA-2, Northam overperformed in a district where the Republican primary winner dropped out amid accusations he falsified his ballot signatures.

If one defines the coattails effect as a superstar HoD candidate elevating the Gubernatorial candidates, the results are mixed at best. In some districts, Northam appeared to overperform the prediction without any Democrat on the HoD ballot at all. In the HoD districts of Democratic superstars like Chris Hurst, Wendy Gooditis, and Danica Roem (10, 12, and 13), Northam slightly overperformed, slightly overperformed, and underperformed, respectively.

Here's the data I used for this project.

``` r
kable(dat, col.names = c('Cycle', 'District', 'Governor R %', 'Governor D %', 'President D %', 'President R %', 'House of Delegates D %', 'House of Delegates R %', 'Census Black %', 'Census Latino %', 'Census Household Income $', 'Census Bachelors %', 'Dem HoD cand on ballot', 'Gained Dem chal in 2017', 'No Dem chal in 2017'))
```

|  Cycle|  District|  Governor R %|  Governor D %|  President D %|  President R %|  House of Delegates D %|  House of Delegates R %|  Census Black %|  Census Latino %|  Census Household Income $|  Census Bachelors %|  Dem HoD cand on ballot|  Gained Dem chal in 2017|  No Dem chal in 2017|
|------:|---------:|-------------:|-------------:|--------------:|--------------:|-----------------------:|-----------------------:|---------------:|----------------:|--------------------------:|-------------------:|-----------------------:|------------------------:|--------------------:|
|   2013|         1|     0.7850000|     0.2040000|      0.2634639|      0.7365361|               0.0000000|               1.0000000|       0.0338134|        0.0129993|                      35939|           0.1356579|                       0|                       NA|                   NA|
|   2013|         2|     0.3670000|     0.6230000|      0.5968258|      0.4031742|               0.4900000|               0.5100000|       0.2489662|        0.1820408|                      95302|           0.3675415|                       1|                       NA|                   NA|
|   2013|         3|     0.8140000|     0.1800000|      0.2519223|      0.7480777|               0.0000000|               1.0000000|       0.0288683|        0.0077473|                      35286|           0.1210839|                       0|                       NA|                   NA|
|   2013|         4|     0.7510000|     0.2410000|      0.3092691|      0.6907309|               0.0000000|               1.0000000|       0.0219619|        0.0131594|                      37853|           0.1622439|                       0|                       NA|                   NA|
|   2013|         5|     0.7620000|     0.2290000|      0.3000437|      0.6999563|               0.0000000|               1.0000000|       0.0277275|        0.0315119|                      37843|           0.1764380|                       0|                       NA|                   NA|
|   2013|         6|     0.7540000|     0.2370000|      0.3207132|      0.6792868|               0.0000000|               1.0000000|       0.0237699|        0.0195139|                      37174|           0.1554902|                       0|                       NA|                   NA|
|   2013|         7|     0.6300000|     0.3560000|      0.3922286|      0.6077714|               0.0000000|               1.0000000|       0.0436700|        0.0220377|                      50948|           0.2546580|                       0|                       NA|                   NA|
|   2013|         8|     0.6140000|     0.3710000|      0.3772213|      0.6227787|               0.0000000|               1.0000000|       0.0430464|        0.0248072|                      55421|           0.3245219|                       0|                       NA|                   NA|
|   2013|         9|     0.7050000|     0.2840000|      0.3528999|      0.6471001|               0.0000000|               1.0000000|       0.1052764|        0.0287895|                      38886|           0.1607371|                       0|                       NA|                   NA|
|   2013|        10|     0.4480000|     0.5410000|      0.4815397|      0.5184603|               0.3800000|               0.6200000|       0.0791116|        0.1276923|                     108798|           0.5014293|                       1|                       NA|                   NA|
|   2013|        11|     0.3280000|     0.6580000|      0.6550369|      0.3449631|               1.0000000|               0.0000000|       0.3233319|        0.0604716|                      39301|           0.2380487|                       1|                       NA|                   NA|
|   2013|        12|     0.4470000|     0.5340000|      0.5211907|      0.4788093|               0.4200000|               0.5800000|       0.0532022|        0.0277513|                      39677|           0.4173278|                       1|                       NA|                   NA|
|   2013|        13|     0.4150000|     0.5710000|      0.5560782|      0.4439218|               0.4400000|               0.5600000|       0.1198853|        0.2676122|                      87768|           0.3689906|                       1|                       NA|                   NA|
|   2013|        14|     0.5510000|     0.4420000|      0.4982915|      0.5017085|               0.0000000|               1.0000000|       0.3566234|        0.0368369|                      35976|           0.1547698|                       0|                       NA|                   NA|
|   2013|        15|     0.7110000|     0.2780000|      0.3458819|      0.6541181|               0.0000000|               1.0000000|       0.0203521|        0.0447199|                      49259|           0.1689253|                       0|                       NA|                   NA|
|   2013|        16|     0.6240000|     0.3680000|      0.4363175|      0.5636825|               0.0000000|               1.0000000|       0.2739487|        0.0355979|                      38872|           0.1469824|                       0|                       NA|                   NA|
|   2013|        17|     0.5950000|     0.3920000|      0.3938531|      0.6061469|               0.0000000|               1.0000000|       0.0675465|        0.0314031|                      53927|           0.2966172|                       0|                       NA|                   NA|
|   2013|        18|     0.5980000|     0.3900000|      0.4092155|      0.5907845|               0.0000000|               1.0000000|       0.0629506|        0.0585564|                      79566|           0.3086874|                       0|                       NA|                   NA|
|   2013|        19|     0.7120000|     0.2790000|      0.3595199|      0.6404801|               0.0000000|               1.0000000|       0.0720147|        0.0140940|                      50337|           0.1946017|                       0|                       NA|                   NA|
|   2013|        20|     0.5740000|     0.4090000|      0.4213896|      0.5786104|               0.2400000|               0.7600000|       0.0881490|        0.0370914|                      46818|           0.2590567|                       1|                       NA|                   NA|
|   2013|        21|     0.4240000|     0.5630000|      0.5260298|      0.4739702|               0.4300000|               0.5700000|       0.2186466|        0.0858112|                      75442|           0.3172894|                       1|                       NA|                   NA|
|   2013|        22|     0.6610000|     0.3300000|      0.3818953|      0.6181047|               0.0000000|               1.0000000|       0.2001918|        0.0248373|                      45358|           0.2482610|                       0|                       NA|                   NA|
|   2013|        23|     0.6260000|     0.3620000|      0.3447091|      0.6552909|               0.0000000|               1.0000000|       0.1598788|        0.0294504|                      50844|           0.3633225|                       0|                       NA|                   NA|
|   2013|        24|     0.6530000|     0.3370000|      0.3775472|      0.6224528|               0.2900000|               0.7100000|       0.0795348|        0.0233208|                      45855|           0.2134042|                       1|                       NA|                   NA|
|   2013|        25|     0.5740000|     0.4140000|      0.3830007|      0.6169993|               0.3400000|               0.6600000|       0.0296406|        0.0324191|                      62042|           0.3602263|                       1|                       NA|                   NA|
|   2013|        26|     0.5280000|     0.4570000|      0.4397278|      0.5602722|               0.0000000|               1.0000000|       0.0502614|        0.1374441|                      44597|           0.3044937|                       0|                       NA|                   NA|
|   2013|        27|     0.4760000|     0.5090000|      0.4591001|      0.5408999|               0.4100000|               0.5900000|       0.1998606|        0.0648636|                      71634|           0.3761799|                       1|                       NA|                   NA|
|   2013|        28|     0.4780000|     0.5080000|      0.5004472|      0.4995528|               0.4000000|               0.6000000|       0.1827658|        0.1080138|                      81509|           0.3543860|                       1|                       NA|                   NA|
|   2013|        29|     0.6070000|     0.3810000|      0.3937815|      0.6062185|               0.0000000|               1.0000000|       0.0633546|        0.0936207|                      59776|           0.2825708|                       0|                       NA|                   NA|
|   2013|        30|     0.6100000|     0.3790000|      0.4324729|      0.5675271|               0.0000000|               1.0000000|       0.1372540|        0.0656757|                      60213|           0.2296427|                       0|                       NA|                   NA|
|   2013|        31|     0.4270000|     0.5640000|      0.5375898|      0.4624102|               0.4700000|               0.5300000|       0.2018396|        0.1542925|                     115004|           0.3909751|                       1|                       NA|                   NA|
|   2013|        32|     0.3700000|     0.6190000|      0.5282258|      0.4717742|               0.4700000|               0.5300000|       0.0831465|        0.1100777|                     123674|           0.6301911|                       1|                       NA|                   NA|
|   2013|        33|     0.5400000|     0.4470000|      0.4255345|      0.5744655|               0.3750000|               0.6250000|       0.0393908|        0.0737671|                     103131|           0.4462532|                       1|                       NA|                   NA|
|   2013|        34|     0.3930000|     0.6000000|      0.5023641|      0.4976359|               0.5000000|               0.5000000|       0.0341999|        0.0767472|                     174557|           0.7472668|                       1|                       NA|                   NA|
|   2013|        35|     0.2930000|     0.6970000|      0.6058070|      0.3941930|               1.0000000|               0.0000000|       0.0649793|        0.1112557|                     112868|           0.7075622|                       1|                       NA|                   NA|
|   2013|        36|     0.2530000|     0.7360000|      0.6488126|      0.3511874|               1.0000000|               0.0000000|       0.0840158|        0.1152684|                     114950|           0.6927748|                       1|                       NA|                   NA|
|   2013|        37|     0.3050000|     0.6830000|      0.6127012|      0.3872988|               0.5700000|               0.4300000|       0.0813701|        0.1256332|                     102474|           0.5836702|                       1|                       NA|                   NA|
|   2013|        38|     0.2630000|     0.7280000|      0.6724476|      0.3275524|               0.7500000|               0.2500000|       0.0841485|        0.3328624|                      84669|           0.4403684|                       1|                       NA|                   NA|
|   2013|        39|     0.3020000|     0.6910000|      0.6262061|      0.3737939|               1.0000000|               0.0000000|       0.1053926|        0.1997981|                     108852|           0.4887493|                       1|                       NA|                   NA|
|   2013|        40|     0.4440000|     0.5470000|      0.4806482|      0.5193518|               0.3500000|               0.6500000|       0.0715105|        0.0926208|                     132983|           0.6122453|                       1|                       NA|                   NA|
|   2013|        41|     0.3310000|     0.6590000|      0.5887733|      0.4112267|               1.0000000|               0.0000000|       0.0553783|        0.1248170|                     122845|           0.6202762|                       1|                       NA|                   NA|
|   2013|        42|     0.3870000|     0.6060000|      0.5329478|      0.4670522|               0.3700000|               0.6300000|       0.0960928|        0.1037399|                     130887|           0.6180922|                       1|                       NA|                   NA|
|   2013|        43|     0.2660000|     0.7240000|      0.6601435|      0.3398565|               0.6547497|               0.3452503|       0.1950708|        0.1433447|                     110196|           0.5777903|                       1|                       NA|                   NA|
|   2013|        44|     0.3000000|     0.6920000|      0.6558170|      0.3441830|               1.0000000|               0.0000000|       0.1797335|        0.2574827|                      84673|           0.4429371|                       1|                       NA|                   NA|
|   2013|        45|     0.2220000|     0.7690000|      0.6874396|      0.3125604|               1.0000000|               0.0000000|       0.0976413|        0.1403563|                     110696|           0.7439729|                       1|                       NA|                   NA|
|   2013|        46|     0.1970000|     0.7950000|      0.7458479|      0.2541521|               0.6715093|               0.3284907|       0.2890426|        0.1802901|                      73081|           0.5244455|                       1|                       NA|                   NA|
|   2013|        47|     0.1930000|     0.7960000|      0.6938941|      0.3061059|               0.7800000|               0.2200000|       0.0449420|        0.1401665|                     115209|           0.7578976|                       1|                       NA|                   NA|
|   2013|        48|     0.2570000|     0.7350000|      0.6408107|      0.3591893|               1.0000000|               0.0000000|       0.0372652|        0.0808492|                     133567|           0.8106567|                       1|                       NA|                   NA|
|   2013|        49|     0.1700000|     0.8200000|      0.7605719|      0.2394281|               1.0000000|               0.0000000|       0.1881535|        0.2550919|                      79809|           0.5464341|                       1|                       NA|                   NA|
|   2013|        50|     0.4120000|     0.5760000|      0.5481378|      0.4518622|               0.4100000|               0.5900000|       0.1360880|        0.2678901|                      90166|           0.3517318|                       1|                       NA|                   NA|
|   2013|        51|     0.4360000|     0.5530000|      0.5192327|      0.4807673|               0.0000000|               1.0000000|       0.1571711|        0.1498051|                     111150|           0.4588779|                       0|                       NA|                   NA|
|   2013|        52|     0.2330000|     0.7570000|      0.7334841|      0.2665159|               1.0000000|               0.0000000|       0.3068508|        0.3049163|                      70011|           0.2596621|                       1|                       NA|                   NA|
|   2013|        53|     0.2420000|     0.7460000|      0.6752410|      0.3247590|               1.0000000|               0.0000000|       0.0475720|        0.1971945|                     102167|           0.6051925|                       1|                       NA|                   NA|
|   2013|        54|     0.5270000|     0.4610000|      0.4665312|      0.5334688|               0.0000000|               1.0000000|       0.1784275|        0.0929887|                      82697|           0.3068005|                       0|                       NA|                   NA|
|   2013|        55|     0.5800000|     0.4050000|      0.4055478|      0.5944522|               0.4000000|               0.6000000|       0.1565794|        0.0273726|                      74013|           0.3082481|                       1|                       NA|                   NA|
|   2013|        56|     0.5800000|     0.4090000|      0.3789879|      0.6210121|               0.0000000|               1.0000000|       0.1146341|        0.0250804|                      80709|           0.4160701|                       0|                       NA|                   NA|
|   2013|        57|     0.1970000|     0.7920000|      0.7135195|      0.2864805|               1.0000000|               0.0000000|       0.1648223|        0.0683031|                      51357|           0.5047714|                       1|                       NA|                   NA|
|   2013|        58|     0.5490000|     0.4390000|      0.4184373|      0.5815627|               0.0000000|               1.0000000|       0.0659606|        0.0411669|                      66439|           0.3390978|                       0|                       NA|                   NA|
|   2013|        59|     0.6300000|     0.3600000|      0.4071674|      0.5928326|               0.0000000|               1.0000000|       0.1929874|        0.0251087|                      49762|           0.1918966|                       0|                       NA|                   NA|
|   2013|        60|     0.5870000|     0.4050000|      0.4813328|      0.5186672|               0.0000000|               1.0000000|       0.3410894|        0.0175766|                      36876|           0.1728421|                       0|                       NA|                   NA|
|   2013|        61|     0.6120000|     0.3810000|      0.4541555|      0.5458445|               0.2900000|               0.7100000|       0.3325244|        0.0219312|                      40634|           0.1506854|                       1|                       NA|                   NA|
|   2013|        62|     0.4940000|     0.4950000|      0.4701012|      0.5298988|               0.4000000|               0.6000000|       0.2643975|        0.0762981|                      61873|           0.2319297|                       1|                       NA|                   NA|
|   2013|        63|     0.3200000|     0.6720000|      0.7326630|      0.2673370|               1.0000000|               0.0000000|       0.5808109|        0.0531434|                      42351|           0.1570480|                       1|                       NA|                   NA|
|   2013|        64|     0.5910000|     0.3990000|      0.4184796|      0.5815204|               0.0000000|               1.0000000|       0.2455004|        0.0261896|                      62725|           0.2464319|                       0|                       NA|                   NA|
|   2013|        65|     0.6080000|     0.3800000|      0.3490581|      0.6509419|               0.0000000|               1.0000000|       0.1301024|        0.0253719|                      78959|           0.3970962|                       0|                       NA|                   NA|
|   2013|        66|     0.5990000|     0.3910000|      0.3726368|      0.6273632|               0.0000000|               1.0000000|       0.1932713|        0.0443444|                      74203|           0.3296247|                       0|                       NA|                   NA|
|   2013|        67|     0.3620000|     0.6270000|      0.5449948|      0.4550052|               0.0000000|               1.0000000|       0.0618355|        0.0990310|                     130162|           0.6516971|                       0|                       NA|                   NA|
|   2013|        68|     0.4220000|     0.5670000|      0.4446740|      0.5553260|               0.3760762|               0.6239238|       0.0837880|        0.0330281|                      80238|           0.6180032|                       1|                       NA|                   NA|
|   2013|        69|     0.1250000|     0.8610000|      0.8570439|      0.1429561|               1.0000000|               0.0000000|       0.5585921|        0.1043869|                      37308|           0.2538909|                       1|                       NA|                   NA|
|   2013|        70|     0.1860000|     0.8050000|      0.7996929|      0.2003071|               1.0000000|               0.0000000|       0.5647612|        0.1498834|                      44803|           0.1905213|                       1|                       NA|                   NA|
|   2013|        71|     0.1070000|     0.8810000|      0.8689599|      0.1310401|               0.8800000|               0.1200000|       0.5720908|        0.0227228|                      34448|           0.3375663|                       1|                       NA|                   NA|
|   2013|        72|     0.4550000|     0.5320000|      0.4560534|      0.5439466|               0.0000000|               1.0000000|       0.1389720|        0.0623085|                      73030|           0.4995904|                       0|                       NA|                   NA|
|   2013|        73|     0.4510000|     0.5350000|      0.4713095|      0.5286905|               0.0000000|               1.0000000|       0.1363973|        0.0835244|                      59816|           0.4273231|                       0|                       NA|                   NA|
|   2013|        74|     0.2360000|     0.7540000|      0.7558539|      0.2441461|               0.7900000|               0.2100000|       0.6064521|        0.0332886|                      48240|           0.2325020|                       1|                       NA|                   NA|
|   2013|        75|     0.4340000|     0.5590000|      0.6252865|      0.3747135|               1.0000000|               0.0000000|       0.5276134|        0.0256771|                      37706|           0.1228756|                       1|                       NA|                   NA|
|   2013|        76|     0.4920000|     0.4980000|      0.4527978|      0.5472022|               0.0000000|               1.0000000|       0.2497144|        0.0404565|                      81654|           0.3306591|                       0|                       NA|                   NA|
|   2013|        77|     0.2130000|     0.7730000|      0.7850857|      0.2149143|               1.0000000|               0.0000000|       0.5949961|        0.0528770|                      43110|           0.1603359|                       1|                       NA|                   NA|
|   2013|        78|     0.5480000|     0.4410000|      0.3971628|      0.6028372|               0.0000000|               1.0000000|       0.1592157|        0.0515793|                      80361|           0.3667022|                       0|                       NA|                   NA|
|   2013|        79|     0.3340000|     0.6520000|      0.6284307|      0.3715693|               1.0000000|               0.0000000|       0.3062669|        0.0720612|                      52004|           0.2806925|                       1|                       NA|                   NA|
|   2013|        80|     0.2350000|     0.7530000|      0.7601968|      0.2398032|               1.0000000|               0.0000000|       0.5757654|        0.0327637|                      45489|           0.2292405|                       1|                       NA|                   NA|
|   2013|        81|     0.5490000|     0.4390000|      0.4210187|      0.5789813|               0.3000000|               0.7000000|       0.1964471|        0.0561329|                      67073|           0.3008357|                       1|                       NA|                   NA|
|   2013|        82|     0.5250000|     0.4640000|      0.4076888|      0.5923112|               0.3500000|               0.6500000|       0.1066091|        0.0490262|                      66729|           0.4420491|                       1|                       NA|                   NA|
|   2013|        83|     0.4830000|     0.5030000|      0.4676518|      0.5323482|               0.0000000|               1.0000000|       0.1608678|        0.0681469|                      61412|           0.3450030|                       0|                       NA|                   NA|
|   2013|        84|     0.4670000|     0.5210000|      0.4967881|      0.5032119|               0.0000000|               1.0000000|       0.1976474|        0.0857713|                      71843|           0.2939760|                       0|                       NA|                   NA|
|   2013|        85|     0.4530000|     0.5330000|      0.4987171|      0.5012829|               0.0000000|               1.0000000|       0.1920934|        0.0809108|                      64514|           0.2949792|                       0|                       NA|                   NA|
|   2013|        86|     0.3110000|     0.6780000|      0.6093758|      0.3906242|               0.5448680|               0.4551320|       0.0971075|        0.2392426|                     118112|           0.5674406|                       1|                       NA|                   NA|
|   2013|        87|     0.3470000|     0.6400000|      0.5700970|      0.4299030|               0.4991473|               0.5008527|       0.0808324|        0.1471037|                     120501|           0.5855765|                       1|                       NA|                   NA|
|   2013|        88|     0.5440000|     0.4440000|      0.4406363|      0.5593637|               0.0000000|               1.0000000|       0.1585482|        0.0969216|                      80814|           0.3363781|                       0|                       NA|                   NA|
|   2013|        89|     0.1700000|     0.8170000|      0.8173166|      0.1826834|               1.0000000|               0.0000000|       0.5571239|        0.0559253|                      40361|           0.2974878|                       1|                       NA|                   NA|
|   2013|        90|     0.1930000|     0.7990000|      0.7966277|      0.2033723|               1.0000000|               0.0000000|       0.5656866|        0.0787949|                      45480|           0.2047112|                       1|                       NA|                   NA|
|   2013|        91|     0.5150000|     0.4730000|      0.4432418|      0.5567582|               0.0000000|               1.0000000|       0.1958512|        0.0524434|                      69496|           0.3592333|                       0|                       NA|                   NA|
|   2013|        92|     0.1910000|     0.8000000|      0.8098063|      0.1901937|               1.0000000|               0.0000000|       0.6148841|        0.0496389|                      40298|           0.1866614|                       1|                       NA|                   NA|
|   2013|        93|     0.3830000|     0.6060000|      0.5710676|      0.4289324|               0.5500000|               0.4500000|       0.2247991|        0.0845166|                      61758|           0.3859071|                       1|                       NA|                   NA|
|   2013|        94|     0.4270000|     0.5600000|      0.5269299|      0.4730701|               0.4200000|               0.5800000|       0.2396696|        0.0776849|                      60192|           0.3038836|                       1|                       NA|                   NA|
|   2013|        95|     0.2210000|     0.7690000|      0.7866111|      0.2133889|               0.7700000|               0.2300000|       0.6223644|        0.0602354|                      39474|           0.1711946|                       1|                       NA|                   NA|
|   2013|        96|     0.5270000|     0.4630000|      0.4177444|      0.5822556|               0.3900000|               0.6100000|       0.1451883|        0.0567751|                      75240|           0.4330516|                       1|                       NA|                   NA|
|   2013|        97|     0.6830000|     0.3050000|      0.3005164|      0.6994836|               0.2100000|               0.7900000|       0.1160100|        0.0245420|                      73016|           0.2863037|                       1|                       NA|                   NA|
|   2013|        98|     0.6210000|     0.3680000|      0.3982831|      0.6017169|               0.0000000|               1.0000000|       0.1625875|        0.0224167|                      56101|           0.2198504|                       0|                       NA|                   NA|
|   2013|        99|     0.5900000|     0.4010000|      0.4482346|      0.5517654|               0.0000000|               1.0000000|       0.2492539|        0.0416377|                      57212|           0.2418621|                       0|                       NA|                   NA|
|   2013|       100|     0.4630000|     0.5270000|      0.5514780|      0.4485220|               0.4300000|               0.5700000|       0.2983257|        0.0891584|                      41013|           0.2009044|                       1|                       NA|                   NA|
|   2017|         1|     0.7417905|     0.2582095|      0.1803360|      0.8196640|               0.2379779|               0.7620221|       0.0346281|        0.0105136|                      35170|           0.1156689|                       1|                        1|                    0|
|   2017|         2|     0.4406443|     0.5593557|      0.6070901|      0.3929099|               0.6267454|               0.3732546|       0.0224444|        0.0060500|                      33008|           0.1123070|                       1|                        0|                    0|
|   2017|         3|     0.7376437|     0.2623563|      0.1670200|      0.8329800|               0.2168466|               0.7831534|       0.0199940|        0.0029079|                      32862|           0.0946715|                       1|                        1|                    0|
|   2017|         4|     0.6838545|     0.3161455|      0.2141093|      0.7858907|               0.0000000|               1.0000000|       0.0242441|        0.0146847|                      37920|           0.1927512|                       0|                        0|                    0|
|   2017|         5|     0.7047327|     0.2952673|      0.2199027|      0.7800973|               0.0000000|               1.0000000|       0.0176463|        0.0248903|                      34487|           0.1236554|                       0|                        0|                    0|
|   2017|         6|     0.6871861|     0.3128139|      0.2138534|      0.7861466|               0.0000000|               1.0000000|       0.0506028|        0.0102304|                      41814|           0.1542243|                       0|                        0|                    0|
|   2017|         7|     0.6269483|     0.3730517|      0.3404795|      0.6595205|               0.3331274|               0.6668726|       0.0475915|        0.0206757|                      45613|           0.1981244|                       1|                        1|                    0|
|   2017|         8|     0.6349137|     0.3650863|      0.3496414|      0.6503586|               0.3598430|               0.6401570|       0.0581905|        0.0161701|                      61441|           0.3337402|                       1|                        1|                    0|
|   2017|         9|     0.6842520|     0.3157480|      0.2730920|      0.7269080|               0.2965059|               0.7034941|       0.0927325|        0.0241183|                      44926|           0.1579094|                       1|                        1|                    0|
|   2017|        10|     0.5164896|     0.4835104|      0.5338450|      0.4661550|               0.5194925|               0.4805075|       0.1459665|        0.0338587|                      32938|           0.1033273|                       1|                        0|                    0|
|   2017|        11|     0.3585554|     0.6414446|      0.6407080|      0.3592920|               1.0000000|               0.0000000|       0.3634338|        0.0614648|                      34510|           0.1399799|                       1|                        0|                    0|
|   2017|        12|     0.4644587|     0.5355413|      0.5159487|      0.4840513|               0.5359631|               0.4640369|       0.0452660|        0.0250813|                      42200|           0.2948216|                       1|                        0|                    0|
|   2017|        13|     0.4965966|     0.5034034|      0.5783832|      0.4216168|               0.5461904|               0.4538096|       0.1042586|        0.1254600|                     117702|           0.4938247|                       1|                        0|                    0|
|   2017|        14|     0.5732369|     0.4267631|      0.4580322|      0.5419678|               0.0000000|               1.0000000|       0.4001314|        0.0276785|                      33616|           0.1389616|                       0|                        0|                    0|
|   2017|        15|     0.6720039|     0.3279961|      0.2604982|      0.7395018|               0.0000000|               1.0000000|       0.0208296|        0.0387790|                      49178|           0.1631517|                       0|                        0|                    0|
|   2017|        16|     0.6205182|     0.3794818|      0.3791559|      0.6208441|               0.0000000|               1.0000000|       0.2268602|        0.0322305|                      39319|           0.1395805|                       0|                        0|                    0|
|   2017|        17|     0.6300388|     0.3699612|      0.3685961|      0.6314039|               0.3935682|               0.6064318|       0.0462688|        0.0239340|                      53645|           0.2921521|                       1|                        1|                    0|
|   2017|        18|     0.6170122|     0.3829878|      0.3695427|      0.6304573|               0.3622911|               0.6377089|       0.0529995|        0.0665960|                      68221|           0.2598904|                       1|                        1|                    0|
|   2017|        19|     0.6805922|     0.3194078|      0.2673070|      0.7326930|               0.0000000|               1.0000000|       0.0630383|        0.0140801|                      57261|           0.2184071|                       0|                        0|                    0|
|   2017|        20|     0.5903847|     0.4096153|      0.3909016|      0.6090984|               0.4383766|               0.5616234|       0.0710368|        0.0213334|                      46952|           0.2203185|                       1|                        0|                    0|
|   2017|        21|     0.4783792|     0.5216208|      0.5240851|      0.4759149|               0.5287616|               0.4712384|       0.2417941|        0.0707966|                      65928|           0.2484985|                       1|                        0|                    0|
|   2017|        22|     0.6726060|     0.3273940|      0.3331135|      0.6668865|               0.0000000|               1.0000000|       0.1170827|        0.0185972|                      48875|           0.1761685|                       0|                        0|                    0|
|   2017|        23|     0.6685243|     0.3314757|      0.3440906|      0.6559094|               0.3451829|               0.6548171|       0.2886751|        0.0287739|                      38105|           0.2267128|                       1|                        1|                    0|
|   2017|        24|     0.6372479|     0.3627521|      0.3225806|      0.6774194|               0.0000000|               1.0000000|       0.0926133|        0.0201405|                      43883|           0.1814842|                       0|                        0|                    1|
|   2017|        25|     0.6016361|     0.3983639|      0.3907255|      0.6092745|               0.4194258|               0.5805742|       0.0492919|        0.0380622|                      52523|           0.2283396|                       1|                        0|                    0|
|   2017|        26|     0.5911550|     0.4088450|      0.4580637|      0.5419363|               0.4539956|               0.5460044|       0.0473926|        0.1235201|                      44699|           0.2015827|                       1|                        1|                    0|
|   2017|        27|     0.5441114|     0.4558886|      0.4875146|      0.5124854|               0.4974902|               0.5025098|       0.2737696|        0.0772068|                      68426|           0.2714176|                       1|                        0|                    0|
|   2017|        28|     0.5249937|     0.4750063|      0.4941851|      0.5058149|               0.4977914|               0.5022086|       0.1909630|        0.0771890|                      77612|           0.3058251|                       1|                        0|                    0|
|   2017|        29|     0.6278637|     0.3721363|      0.3528912|      0.6471088|               0.3563531|               0.6436469|       0.0594383|        0.0814111|                      59398|           0.2364826|                       1|                        1|                    0|
|   2017|        30|     0.6122390|     0.3877610|      0.3710925|      0.6289075|               0.3780903|               0.6219097|       0.1439897|        0.0607199|                      61817|           0.1980582|                       1|                        1|                    0|
|   2017|        31|     0.4856223|     0.5143777|      0.5541238|      0.4458762|               0.5456340|               0.4543660|       0.2283169|        0.2016696|                      92966|           0.2720326|                       1|                        0|                    0|
|   2017|        32|     0.4627207|     0.5372793|      0.6108881|      0.3891119|               0.5854571|               0.4145429|       0.0713852|        0.1041253|                     123865|           0.5802252|                       1|                        0|                    0|
|   2017|        33|     0.5755781|     0.4244219|      0.4292720|      0.5707280|               0.4515515|               0.5484485|       0.0675028|        0.0983561|                     111488|           0.4648184|                       1|                        0|                    0|
|   2017|        34|     0.4461640|     0.5538360|      0.6140718|      0.3859282|               0.6094196|               0.3905804|       0.0268995|        0.0637430|                     155772|           0.7384781|                       1|                        0|                    0|
|   2017|        35|     0.3655640|     0.6344360|      0.7086065|      0.2913935|               1.0000000|               0.0000000|       0.0409147|        0.0887640|                     126196|           0.6713085|                       1|                        0|                    0|
|   2017|        36|     0.3103145|     0.6896855|      0.7362562|      0.2637438|               1.0000000|               0.0000000|       0.0977128|        0.0984650|                     115840|           0.6457871|                       1|                        0|                    0|
|   2017|        37|     0.3940898|     0.6059102|      0.6995124|      0.3004876|               1.0000000|               0.0000000|       0.0618379|        0.1230819|                     111543|           0.5141826|                       1|                        0|                    0|
|   2017|        38|     0.3132445|     0.6867555|      0.7434258|      0.2565742|               0.7368130|               0.2631870|       0.1091581|        0.2505738|                      89084|           0.4580197|                       1|                        0|                    0|
|   2017|        39|     0.3731806|     0.6268194|      0.7098417|      0.2901583|               1.0000000|               0.0000000|       0.0653016|        0.2409082|                      87863|           0.4053408|                       1|                        0|                    0|
|   2017|        40|     0.5321563|     0.4678437|      0.5588767|      0.4411233|               0.5031784|               0.4968216|       0.0581206|        0.1055948|                     122040|           0.5352483|                       1|                        0|                    0|
|   2017|        41|     0.4006001|     0.5993999|      0.6738129|      0.3261871|               1.0000000|               0.0000000|       0.0619805|        0.1395569|                     120467|           0.5529238|                       1|                        0|                    0|
|   2017|        42|     0.4616521|     0.5383479|      0.6219762|      0.3780238|               0.6101932|               0.3898068|       0.1526444|        0.1284141|                     113995|           0.5175646|                       1|                        0|                    0|
|   2017|        43|     0.3297297|     0.6702703|      0.7354839|      0.2645161|               1.0000000|               0.0000000|       0.1568208|        0.1389994|                     105014|           0.5038748|                       1|                        0|                    0|
|   2017|        44|     0.3436261|     0.6563739|      0.7298519|      0.2701481|               1.0000000|               0.0000000|       0.2127692|        0.2407220|                      80360|           0.4155342|                       1|                        0|                    0|
|   2017|        45|     0.2520722|     0.7479278|      0.8042793|      0.1957207|               1.0000000|               0.0000000|       0.1130033|        0.1022194|                     105372|           0.6867474|                       1|                        0|                    0|
|   2017|        46|     0.2352778|     0.7647222|      0.8158537|      0.1841463|               1.0000000|               0.0000000|       0.2833002|        0.1726661|                      69852|           0.5121380|                       1|                        0|                    0|
|   2017|        47|     0.2347351|     0.7652649|      0.8178130|      0.1821870|               1.0000000|               0.0000000|       0.0742529|        0.1493903|                     100938|           0.6777441|                       1|                        0|                    0|
|   2017|        48|     0.2934367|     0.7065633|      0.7649354|      0.2350646|               1.0000000|               0.0000000|       0.0523083|        0.0792477|                     112427|           0.7707326|                       1|                        0|                    0|
|   2017|        49|     0.2098780|     0.7901220|      0.8401879|      0.1598121|               0.8147194|               0.1852806|       0.1832921|        0.3498578|                      70402|           0.4263566|                       1|                        0|                    0|
|   2017|        50|     0.5031877|     0.4968123|      0.5742682|      0.4257318|               0.5455825|               0.4544175|       0.1265967|        0.3369704|                      72355|           0.2244810|                       1|                        0|                    0|
|   2017|        51|     0.5048180|     0.4951820|      0.5474200|      0.4525800|               0.5298760|               0.4701240|       0.2109915|        0.1867572|                      93021|           0.3735031|                       1|                        1|                    0|
|   2017|        52|     0.3130737|     0.6869263|      0.7592750|      0.2407250|               1.0000000|               0.0000000|       0.2777478|        0.2016691|                      88362|           0.3270056|                       1|                        0|                    0|
|   2017|        53|     0.2961382|     0.7038618|      0.7616179|      0.2383821|               0.7520351|               0.2479649|       0.0386279|        0.1212530|                     110699|           0.6166913|                       1|                        0|                    0|
|   2017|        54|     0.5650606|     0.4349394|      0.4427831|      0.5572169|               0.4200366|               0.5799634|       0.1723503|        0.0761710|                      78598|           0.2791371|                       1|                        1|                    0|
|   2017|        55|     0.5986263|     0.4013737|      0.3921781|      0.6078219|               0.4000465|               0.5999535|       0.1003577|        0.0233725|                      78942|           0.3341339|                       1|                        0|                    0|
|   2017|        56|     0.6183989|     0.3816011|      0.4053184|      0.5946816|               0.4041858|               0.5958142|       0.1286590|        0.0298023|                      77761|           0.3938397|                       1|                        1|                    0|
|   2017|        57|     0.2350384|     0.7649616|      0.8009145|      0.1990855|               1.0000000|               0.0000000|       0.1692324|        0.0522465|                      49015|           0.4333649|                       1|                        0|                    0|
|   2017|        58|     0.5611923|     0.4388077|      0.4215418|      0.5784582|               0.3873936|               0.6126064|       0.0861661|        0.0425739|                      71154|           0.3581611|                       1|                        1|                    0|
|   2017|        59|     0.6251716|     0.3748284|      0.3585799|      0.6414201|               0.3590540|               0.6409460|       0.2569730|        0.0199229|                      46711|           0.1913474|                       1|                        1|                    0|
|   2017|        60|     0.5649079|     0.4350921|      0.4302500|      0.5697500|               0.3805297|               0.6194703|       0.3502311|        0.0172231|                      35282|           0.1385848|                       1|                        1|                    0|
|   2017|        61|     0.5818292|     0.4181708|      0.4038136|      0.5961864|               0.0000000|               1.0000000|       0.3555323|        0.0253574|                      39215|           0.1184195|                       0|                        0|                    1|
|   2017|        62|     0.5476876|     0.4523124|      0.4725933|      0.5274067|               0.4819580|               0.5180420|       0.2644679|        0.0597471|                      57987|           0.1484530|                       1|                        0|                    0|
|   2017|        63|     0.2845106|     0.7154894|      0.6904517|      0.3095483|               1.0000000|               0.0000000|       0.5796448|        0.0292300|                      43471|           0.1260246|                       1|                        0|                    0|
|   2017|        64|     0.5817589|     0.4182411|      0.3819700|      0.6180300|               0.3753332|               0.6246668|       0.2172761|        0.0363613|                      64202|           0.2901546|                       1|                        1|                    0|
|   2017|        65|     0.6517488|     0.3482512|      0.3649233|      0.6350767|               0.3570251|               0.6429749|       0.1047266|        0.0273181|                      91185|           0.4272072|                       1|                        1|                    0|
|   2017|        66|     0.6369848|     0.3630152|      0.3816827|      0.6183173|               0.3628985|               0.6371015|       0.1711928|        0.0414500|                      73215|           0.2844106|                       1|                        1|                    0|
|   2017|        67|     0.4506470|     0.5493530|      0.6418973|      0.3581027|               0.5794599|               0.4205401|       0.0741516|        0.1078001|                     117572|           0.5558091|                       1|                        1|                    0|
|   2017|        68|     0.5073517|     0.4926483|      0.5583410|      0.4416590|               0.5060760|               0.4939240|       0.1159848|        0.0484922|                      70617|           0.4944202|                       1|                        0|                    0|
|   2017|        69|     0.1292644|     0.8707356|      0.8729872|      0.1270128|               1.0000000|               0.0000000|       0.5773960|        0.1075106|                      37626|           0.1941634|                       1|                        0|                    0|
|   2017|        70|     0.1966566|     0.8033434|      0.8014329|      0.1985671|               1.0000000|               0.0000000|       0.6419045|        0.1033724|                      40594|           0.1519629|                       1|                        0|                    0|
|   2017|        71|     0.1094512|     0.8905488|      0.8947110|      0.1052890|               1.0000000|               0.0000000|       0.4923090|        0.0260778|                      36501|           0.2741558|                       1|                        0|                    0|
|   2017|        72|     0.5288598|     0.4711402|      0.5287342|      0.4712658|               0.5282737|               0.4717263|       0.0985607|        0.0416366|                      69899|           0.4867564|                       1|                        1|                    0|
|   2017|        73|     0.5274160|     0.4725840|      0.5420146|      0.4579854|               0.5137897|               0.4862103|       0.1716110|        0.0763676|                      56746|           0.3872430|                       1|                        1|                    0|
|   2017|        74|     0.2428350|     0.7571650|      0.7533096|      0.2466904|               0.9058089|               0.0941911|       0.6481537|        0.0344468|                      43938|           0.1834073|                       1|                        0|                    0|
|   2017|        75|     0.3873402|     0.6126598|      0.5804923|      0.4195077|               1.0000000|               0.0000000|       0.5516317|        0.0174279|                      36088|           0.1030280|                       1|                        0|                    0|
|   2017|        76|     0.5262882|     0.4737118|      0.4591791|      0.5408209|               0.0000000|               1.0000000|       0.2558776|        0.0288112|                      79824|           0.2727732|                       0|                        0|                    0|
|   2017|        77|     0.2136959|     0.7863041|      0.7531718|      0.2468282|               0.8305655|               0.1694345|       0.5948972|        0.0466347|                      48528|           0.1600007|                       1|                        0|                    0|
|   2017|        78|     0.5887637|     0.4112363|      0.4008478|      0.5991522|               0.0000000|               1.0000000|       0.1740233|        0.0445110|                      82990|           0.3347870|                       0|                        0|                    0|
|   2017|        79|     0.3519655|     0.6480345|      0.6253109|      0.3746891|               1.0000000|               0.0000000|       0.3986950|        0.0379279|                      56534|           0.2395911|                       1|                        0|                    0|
|   2017|        80|     0.2299294|     0.7700706|      0.7457564|      0.2542436|               1.0000000|               0.0000000|       0.5615563|        0.0233036|                      42117|           0.2003787|                       1|                        0|                    0|
|   2017|        81|     0.5721118|     0.4278882|      0.3915742|      0.6084258|               0.4065079|               0.5934921|       0.1444562|        0.0623721|                      71855|           0.2590706|                       1|                        0|                    0|
|   2017|        82|     0.5608833|     0.4391167|      0.4172214|      0.5827786|               0.4080378|               0.5919622|       0.0842146|        0.0403011|                      69533|           0.4128025|                       1|                        0|                    0|
|   2017|        83|     0.5254175|     0.4745825|      0.4625783|      0.5374217|               0.4359748|               0.5640252|       0.1889451|        0.0719988|                      57854|           0.2822669|                       1|                        1|                    0|
|   2017|        84|     0.5023765|     0.4976235|      0.4762950|      0.5237050|               0.4833561|               0.5166439|       0.2194850|        0.0817409|                      70999|           0.2434422|                       1|                        1|                    0|
|   2017|        85|     0.5113062|     0.4886938|      0.4970956|      0.5029044|               0.5100926|               0.4899074|       0.2059477|        0.0524091|                      70356|           0.2961388|                       1|                        1|                    0|
|   2017|        86|     0.3866593|     0.6133407|      0.6872779|      0.3127221|               0.6910076|               0.3089924|       0.0760826|        0.2412416|                     102797|           0.4761085|                       1|                        0|                    0|
|   2017|        87|     0.4448942|     0.5551058|      0.6354319|      0.3645681|               0.6198163|               0.3801837|       0.2531484|        0.1047647|                      47265|           0.1759812|                       1|                        0|                    0|
|   2017|        88|     0.5992534|     0.4007466|      0.4283717|      0.5716283|               0.4156708|               0.5843292|       0.1395364|        0.0997100|                      88236|           0.2935738|                       1|                        1|                    0|
|   2017|        89|     0.1788139|     0.8211861|      0.8214065|      0.1785935|               0.8532996|               0.1467004|       0.5466316|        0.0491974|                      39767|           0.2247768|                       1|                        0|                    0|
|   2017|        90|     0.2107192|     0.7892808|      0.7789031|      0.2210969|               1.0000000|               0.0000000|       0.5934856|        0.0347957|                      43102|           0.1310178|                       1|                        0|                    0|
|   2017|        91|     0.5586052|     0.4413948|      0.4424278|      0.5575722|               0.4344670|               0.5655330|       0.1815547|        0.0396303|                      69616|           0.2747216|                       1|                        1|                    0|
|   2017|        92|     0.2015800|     0.7984200|      0.7901864|      0.2098136|               1.0000000|               0.0000000|       0.6297181|        0.0439600|                      42653|           0.1661977|                       1|                        0|                    0|
|   2017|        93|     0.4277867|     0.5722133|      0.6016352|      0.3983648|               0.6009252|               0.3990748|       0.3404711|        0.0901132|                      55328|           0.2339365|                       1|                        0|                    0|
|   2017|        94|     0.4833580|     0.5166420|      0.5296560|      0.4703440|               0.4997412|               0.5002588|       0.2712561|        0.0770986|                      62509|           0.2567065|                       1|                        0|                    0|
|   2017|        95|     0.2355191|     0.7644809|      0.7637759|      0.2362241|               1.0000000|               0.0000000|       0.6228259|        0.0318164|                      40862|           0.1601145|                       1|                        0|                    0|
|   2017|        96|     0.5769709|     0.4230291|      0.4401430|      0.5598570|               0.4296560|               0.5703440|       0.1558856|        0.0423394|                      78403|           0.3985770|                       1|                        0|                    0|
|   2017|        97|     0.7024544|     0.2975456|      0.2926891|      0.7073109|               0.2768310|               0.7231690|       0.1895337|        0.0194961|                      66324|           0.1828246|                       1|                        0|                    0|
|   2017|        98|     0.6129185|     0.3870815|      0.3424552|      0.6575448|               0.3486258|               0.6513742|       0.1608316|        0.0249559|                      56128|           0.1888906|                       1|                        1|                    0|
|   2017|        99|     0.5844891|     0.4155109|      0.4033342|      0.5966658|               0.3776814|               0.6223186|       0.2552331|        0.0338513|                      56315|           0.2004984|                       1|                        1|                    0|
|   2017|       100|     0.4860747|     0.5139253|      0.5136195|      0.4863805|               0.4772190|               0.5227810|       0.3062967|        0.0852980|                      40012|           0.1484275|                       1|                        0|                    0|
