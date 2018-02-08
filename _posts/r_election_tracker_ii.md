---
title: "Special Topics in R, part 2: ggplot2"
author: "John Ray"
date: "February 8, 2018"
layout: post

output: 
  md_document:
    variant: markdown_github
---

In the previous post, we learned how to acquire spatial data and manipulate it in a few basic but important ways in `R`. We did not run much code in the previous post. Indeed, to get up to speed, the only code you need to run is:

``` r
library(rgdal, quietly = T, warn.conflicts = F)
library(ggplot2)

download.file('http://www2.census.gov/geo/tiger/GENZ2016/shp/cb_2016_51_sldl_500k.zip', '~/Downloads/cb_2016_51_sldl_500k.zip')
unzip('~/Downloads/cb_2016_51_sldl_500k.zip', exdir = '~/Downloads/cb_2016_51_sldl_500k')

shp = readOGR('~Downloads/cb_2016_51_sldl_500k/', layer = 'cb_2016_51_sldl_500k')
```

    ## OGR data source with driver: ESRI Shapefile 
    ## Source: "/users/johnray/Downloads/cb_2016_51_sldl_500k/", layer: "cb_2016_51_sldl_500k"
    ## with 100 features
    ## It has 9 fields
    ## Integer64 fields read as strings:  ALAND AWATER

``` r
ggplot(shp, aes(x = long, y = lat, group = group)) +
  geom_polygon()
```

    ## Regions defined for each Polygons

![](https://johnlray.github.io/2018/02/08/va01.png)

It may seem odd to have written so little code for what is supposed to be a tutorial! Code tutorials and `R` tutorials are not the same thing, in my view. In this tutorial I focus on best practices without presenting the false notion there is only one way to things in this language. The best way to do things in `R` is the way you understand, can do quickly, can replicate, and can share with your teammates. There is consensus on a few general practices, and those are what we will focus on.

The domain of data visualization is one with strong general consensus on practice among `R` users. The library `ggplot2` is the modal means of vata visualization. Indeed, you will find many other libraries that contain their own built-in functions for data viz that are almost always a custom call to `ggplot2`! One reason for this is that `ggplot2` is based on a theory of data visualization that is quite coherent and logical, and even sensible once you get the hang of it. There are a few papers on the underlying concept, one of them being [here](http://www.jstor.org/stable/pdf/25651297.pdf?casa_token=5f5uQwd-ZZcAAAAA:qu40H1YSwBg7mwKx37vs9Rop8gzo5cY0EeWuTq5H6MFOngy-HoiOpYY-Nfy4lV2GvYG1PGRH9CPpv7gm0sWXXJ7aqtXKZhgp67q9ezdxsFcmYJnsxozE). The general idea behing `ggplot` is a unified visualization system called the **g**rammer of **g**raphics. That grammar is composed of six distinct types. Those types are:

-   **data**: The data we want to plot
-   **mapping**: Directions that relate the underlying data to the various components of the visualization, such as its x- and y-axes
-   **layers**: The geometric shapes (bars? Points?), statistical transformations (a regression line? A smoother?), and position adjustments (should the bars be next to each other? On top of each other?) that represent the data
-   **scales**: How our mappings relate to the plot's aesthetics (are we shading in points based on the party of the legislator? Are we highlighting estimates that are statistically different from one another?)
-   **a coordinate system**: What are x and y? How far does each go? What is their scale?
-   **a facet speficitation**: How many individual panes we are using to portray our data (does each state get its own plot? Each party? All in one?)

Any object that contains information of all six types is a data visualization.

Most likely, some of these components of our grammar seem too obvious to state out loud. By default, `ggplot2` will make (pretty good) guesses for the coloration and placement of our layers (the **scales** argument), how far x and y should go (the **coordinate system**), and how many plots to draw (the **facet specification**).

We will start with a simpler example than our Virginia map, and then move on to the data I introduced in the previous episode. A simple example is worth our time because, while the code for a nice ggplot can get fairly complex, the underlying concepts being put into code will always be the same. Lets say we have some data on the air temperatures of two cities, over the course of a year. That data is available [here](https://johnlray.github.io/2018/02/08/weather_data.csv). The data comes from the weather archive [U.S. climate data](www.usclimatedata.com) and contains data on daily weather in Los Angeles, California and Boston, Massachusetts -- two of my favorite cities.

I say this is a simpler example, but the truth is I definitely did not find `ggplot` to be simple at first. Looking back, much of thise is attributable to the way I think about datasets versus the way I think about graphs. I think of spreadsheets as rows of observations and columns of observation attributes. A plot, however, is primarily concerned with *mappings,* which often involves concatenating columns. As we will see, data that is optimized for data analysis is often not optimized for data visualization.

You should be able to read the data for this exercise directly from my website, or you can simply download it and read it locally.

``` r
library(dplyr)
library(ggplot2)

dat = read.csv('https://johnlray.github.io/2018/02/08/weather_data.csv')
glimpse(dat)
```

    ## Observations: 365
    ## Variables: 11
    ## $ day           <fctr> 01/01/17, 01/02/17, 01/03/17, 01/04/17, 01/05/1...
    ## $ LA_high       <fctr> 57.9, 55.9, 60.1, 61, 61, 66, 68, 77, 69.1, 59,...
    ## $ LA_low        <fctr> 43, 48.9, 48.9, 51.1, 54, 50, 48.9, 57.9, 53.1,...
    ## $ LA_precip     <fctr> 0.03, 0, 0, 0, 0.77, 0, 0.11, 0, 0.77, 0.06, 0....
    ## $ LA_snow       <fctr> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ LA_snow_depth <fctr> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ Boston_high   <dbl> 44.1, 41.0, 44.1, 48.0, 34.0, 31.1, 24.3, 22.1, ...
    ## $ Boston_low    <dbl> 33.1, 28.2, 39.9, 33.1, 27.1, 24.3, 17.2, 13.1, ...
    ## $ Boston_precip <fctr> 0.07, T, 0.89, 0.06, 0, 0.06, 0.53, 0.01, 0, 0....
    ## $ Boston_snow   <fctr> 0, 0, 0, T, 0, 1.1, 7.01, 0.2, 0, 0, 0, 0, 0, 0...
    ## $ X             <fctr> -, -, -, -, -, -, -, -, -, -, -, -, -, -, -, -,...

Note that here I use the `glimpse()` function, which accomplishes a similar task to the parts of base functions `str()` and `head()` that we want. We have 365 observations realized across elevent different variabes. Also note that many of our variables are stored as a factor rather than as, say, a `Date` for `day`, a numeric for the temperature highs and lows, etc. Before we get started, lets think about what we might do with this data. We have 365 observations, one for each day of 2017, including variables realized for the temperature highs and lows, rainfall, snowfall, and whatever `snow_depth` is. Perhaps that's how much snow actually sticks? But I don't know how much we'd learn from `snow_depth` using the data we have. Perhaps we could show that Boston has more snowfall in the winter than in other seasons, or perhaps we could show that Boston has more snow than Los Angeles? Something tells me there would not be much variation in snowfall in Los Angeles. Lets have a look.

``` r
table(dat$LA_snow)
```

    ## 
    ##   -   0 
    ##  13 352

Well, investigating snowfall in Los Angeles was not an entirely fruitless exercise. Creating a table of Los Angeles' snowfall data not only showed that there is some missingness in the data, but that this missingness is not encoded in the data as `NA`s. Instead, it looks like the author of the data opted to use `-` to represent missing data. Even though I don't think much would be gained from making a plot of snowfall in Los Angeles, we should figure out if this missingness occurs in other variables.

As a longtime Los Angeles resident, and a former Boston resident, I know the plot I want to make. Lets compare daily temperature in Boston and Los Angeles. We already have a hunch about what that plot will look like: Los Angeles as a comfortable, fairly constant temperature, and Boston alternates between Westeros season one and Westeros season six. You already have an idea what this plot will look like, but let me recommend being explicit as a great time-saving strategy. We will save a lot of time in our coding if we pause and just sketch the plot we want to see when we're done. Here, in a nutshell, is what I envision.

![](https://johnlray.github.io/2018/02/08/01.png)

Oh Lord, I came here for an `R` tutorial and this guy's showing me stick figures! While we're stuck here, let me explain what I mean about "variables" versus "mappings." Note that while we're plotting two series, we're only making one *mapping*: We are mapping our data by time and temperature to lines. We will *color* those lines by city, which provides grouping to our data. Maybe we'll also get fancy and include a points mapping. We'll see.

This is the key difference between the traditional use of `plot()` and the use of `ggplot()`. With `plot()` you would probably pass `x = dat$day`, `y = dat$LA_high` and `x = dat$day`, `y = dat$Boston_high` to two separate arguments of `plot()` and `lines()` or something like that. With `gglot()` we are passing `day` as the x-axis, temperature as the y-axis, and we're coloring in those x-y pairings using city.

There's just two problems: Our data is still a mess, and half of those variables don't exist yet! Let's start with the easy problem. We probably want the `day` variable to be interpreted as a `Date` type, and we probably want `LA_high` and `Boston_high` (wasn't that a TV show?) to be numeric, which will mean getting rid of those `-` characters where there is missing data. Let's start by telling `R` not to assume a variable is a factor just because it contains strings, which we will do by reloading the data and passing `FALSE` to the `stringsAsFactors` argument of `read.csv()`.

``` r
dat = read.csv('https://johnlray.github.io/2018/02/08/weather_data.csv', stringsAsFactors = F)
glimpse(dat)
```

    ## Observations: 365
    ## Variables: 11
    ## $ day           <chr> "01/01/17", "01/02/17", "01/03/17", "01/04/17", ...
    ## $ LA_high       <chr> "57.9", "55.9", "60.1", "61", "61", "66", "68", ...
    ## $ LA_low        <chr> "43", "48.9", "48.9", "51.1", "54", "50", "48.9"...
    ## $ LA_precip     <chr> "0.03", "0", "0", "0", "0.77", "0", "0.11", "0",...
    ## $ LA_snow       <chr> "0", "0", "0", "0", "0", "0", "0", "0", "0", "0"...
    ## $ LA_snow_depth <chr> "0", "0", "0", "0", "0", "0", "0", "0", "0", "0"...
    ## $ Boston_high   <dbl> 44.1, 41.0, 44.1, 48.0, 34.0, 31.1, 24.3, 22.1, ...
    ## $ Boston_low    <dbl> 33.1, 28.2, 39.9, 33.1, 27.1, 24.3, 17.2, 13.1, ...
    ## $ Boston_precip <chr> "0.07", "T", "0.89", "0.06", "0", "0.06", "0.53"...
    ## $ Boston_snow   <chr> "0", "0", "0", "T", "0", "1.1", "7.01", "0.2", "...
    ## $ X             <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"...

Not a bad start! Then, we'll convert `day` from a `character` type to a `Date` type. Start by looking at a couple values of the \`day variable.

``` r
dat$day[1:5]
```

    ## [1] "01/01/17" "01/02/17" "01/03/17" "01/04/17" "01/05/17"

Even though the data is not yet in `Date` format, we can already see the date in each string. We first have a "month" value, a "day" value, and then a "year" value separated by forward slashes. We will tell `R` to consider everything before the first forward slash as the month, the stuff before the second forward slash as the day, and everything after the second forward slash as the year. We denote the month with `%m`, day with %d`, and year with, wouldn't you know it,`%y`. In the fairly common case where the year is four characters long (i.e., "2017" instead of "17"), we would simply capitalize the`y\`.

``` r
dat$day <- as.Date(dat$day, "%m/%d/%y")
str(dat$day)
```

    ##  Date[1:365], format: "2017-01-01" "2017-01-02" "2017-01-03" "2017-01-04" "2017-01-05" ...

Next, we'll convert the temperature data to numeric. This is straightforward because we're just replacing the "missing data" character with one more convenient to `R`. `R` will automatically convert our garbage data, the `-`s, to `NA`.

``` r
dat$LA_high <- dat$LA_high %>% as.numeric()
```

    ## Warning in function_list[[k]](value): NAs introduced by coercion

``` r
dat$Boston_high <- dat$Boston_high %>% as.numeric()
```

Alright, on the data side, we're ready. We have our dates, and our temperatures. But we don't have our mappings yet! Lets convert our data so that we can map a theoretical `temperature` to the y-axis and a theoretical `city` to the coloration.

``` r
library(reshape2)
```

    ## Warning: package 'reshape2' was built under R version 3.4.3

``` r
dat <- melt(dat[,c('day','LA_high','Boston_high')], id.vars = 'day')
glimpse(dat)
```

    ## Observations: 730
    ## Variables: 3
    ## $ day      <date> 2017-01-01, 2017-01-02, 2017-01-03, 2017-01-04, 2017...
    ## $ variable <fctr> LA_high, LA_high, LA_high, LA_high, LA_high, LA_high...
    ## $ value    <dbl> 57.9, 55.9, 60.1, 61.0, 61.0, 66.0, 68.0, 77.0, 69.1,...

Now we have three variables and 730 observations. We have our x-axis, `day`, our y-axis, `value`, and our color variable, `variable`. The two new variable names, `variable` and `value`, are created by default from the `melt()` function. You will often find yourself `melt`ing data for plotting purposes, so lets dwell on its use for a minute.

Note that the first argument I passed to `melt()` was a subsetted data frame. It isn't strictly necessary to only include the columns you want to plot in your call to `melt()`, but I find it keeps the result uncluttered and easier to understand. I subsetted by the three variables I knew would go into our call to `ggplot()`. Then notice I passed a value to the `id.vars` argument. The `melt()` function, ultimately, stacks our data on top of itself. Here, I used the `id.vars` data to tell `melt()` I wanted to stack `day` on top of itself, leaving `LA_high` stacked on top of `Boston_high`. In the new data, every value of `day` is duplicated twice, once for each city-temperature pair. The city-temperature pairs are our original `LA_high` and `Boston_high` data. Hence, the phrase `LA_high` and `Boston_high` are the two unique values occupying the `variable` variable.

Notice that we now have a complete grammar of graphics to work with. Lets fill it in:

-   **data**: The dataframe `dat`
-   **mapping**: `day` to x, `value` to y, `variable` to color
-   **layers**: A line and maybe some points
-   **scales**: x is continuous from January 1 and end at December 31, y is continuous from whatever the lowest temperatures are to the highest
-   **a coordinate system**: x will be days, y will be degrees. Neither of these need to be passed explicitly to `ggplot()`, but we know what they are
-   **a facet speficitation**: We will make one plot.

And with that in mind, we can fill in our ggplot syntax.

``` r
ggplot(data = dat, aes(x = day, y = value, color = variable)) +
  geom_line()
```

![](https://johnlray.github.io/2018/02/08/02.png)

Notice `ggplot()` already had guesses in mind for the facet, coordinate system, and scales. There is no facetting (i.e., there is one plot) and the coordinate system and scales were derived from the fact that x was a date and y was a numeric. In fact, `ggplot()` will make do with no layers, and even with no default information...

``` r
ggplot(dat, aes(x = day, y = value))
```

![](https://johnlray.github.io/2018/02/08/blank_lines.png)

``` r
ggplot()
```

![](https://johnlray.github.io/2018/02/08/blank.png)

Its just, you know, silly.

We can stare at our first plot and come up with a number of fixes we might implement to make it look nicer. A few come to mind. The y-axis title "value" is literal but not helpful, as our planet, despite three-point-something billion years of existence, is not yet settled on which scale to use for "hot and cold." (Way to go, planet) The x-axis label "day" may be redundant given our spiffy x-axis tick-mark labels of "Jan 2017", "Apr 2017", etc. (all made possible by the guesses `ggplot2` makes about spacing for a `Date` mapping). The plot might benefit from a title. The legend title "variable" is useless, the labels `LA_high` and `Boston_high` are unappetizing, and we might stand to have a plot title while we're here. The pastel colors may not be our preferred. Each of these we can fix.

We'll add a `labs()` argument to take care of the x-axis label, y-axis label, and title. We'll change the legend title, color selection and color labels by using `scale_color_manual()`.

``` r
ggplot(data = dat, aes(x = day, y = value, color = variable)) +
  geom_line() +
  labs(x = '', y = 'Degrees Fahrenheit', title = 'Daily temperatures in Boston and Los Angeles') +
  scale_color_manual(values = c('orange','blue'), labels = c('Los Angeles', 'Boston'), name = 'City')
```

![](https://johnlray.github.io/2018/02/08/03.png)

Notice that something like a "label" argument occurs twice. First, we use the top-level argument `labs()` to set the major axis labels and titles. Then, because the legend elements are based on the "color" mapping, we set "labels" within `scale_color_manual()` where we also set our color choices.

There are too many options in `ggplot()` to enumerate in full, but it also occurs to me we could clean this graph up a bit by using a smoother instead of the line geometry (which we would prefer to use if smoothness were an important criterion for us instead of, say, precision). `ggplot()` includes a variety of smooth lines and non-linear functions, but for most purposes, if you're going to smooth I think you will find using the basic `stat_smooth()` argument is pretty good.

``` r
ggplot(data = dat, aes(x = day, y = value, color = variable)) +
  stat_smooth(method = 'loess') +
  labs(x = '', y = 'Degrees Fahrenheit', title = 'Daily temperatures in Boston and Los Angeles') +
  scale_color_manual(values = c('orange','blue'), labels = c('Los Angeles', 'Boston'), name = 'City')
```

    ## Warning: Removed 13 rows containing non-finite values (stat_smooth).

![](https://johnlray.github.io/2018/02/08/04.png)

The [Loess smoother](https://en.wikipedia.org/wiki/Local_regression) was specifically devised with x-y plots in mind, and is the canonical smoother for data of this type. `ggplot()` chooses the Loess smoother by default, and so my decision to include an explicit value to the `method` argument is redundant.

Notice that when we create the plot, we get a warning saying that some elements of the data were removed. Recall that we have some `NA`s in the data, which `ggplot()` ignores.

Finally, let's add one more type of geometry to the plot based on the same data. By default, new geometries passed to `ggplot()` will assume the aesthetic properties defined in the top level of the `ggplot()` call. Sometimes this is an unwanted assumption, namely when you want to draw a plot involving multiple dataframes. But for most cases, it is a nice time saver.

``` r
ggplot(data = dat, aes(x = day, y = value, color = variable)) +
  geom_point() +
  stat_smooth(method = 'loess') +
  labs(x = '', y = 'Degrees Fahrenheit', title = 'Daily temperatures in Boston and Los Angeles') +
  scale_color_manual(values = c('orange','blue'), labels = c('Los Angeles', 'Boston'), name = 'City')
```

    ## Warning: Removed 13 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 13 rows containing missing values (geom_point).

![](https://johnlray.github.io/2018/02/08/05.png)

While it is interesting to see there are some days of winter where Boston has a higher high than Los Angeles, I'm not sure this plot is much better. But the smattering of points is somewhat more eyegrabbing than just the lines, so maybe this is a better version of the plot. I'm not sure. It depends on what you want to show.

The purpose of this exercise was mainly to show that `ggplot()` takes as its primary arguments *mappings*, which often call for formatting our data such that all of our data for x is in one column and all of our data for y is in another column, etc., whether or not that data is part of the same *series.* That is a tricky process. Then there is making sure your arguments to `ggplot()` go in the right place. Set the value of color and fill in your `scale_color_` and `scale_fill_` arguments. Recall that the geometries you use will respond to the arguments you set either at the initial call to `ggplot()` or to those you set within the geometry themselves. For example, note that we could replicate the above plot by running

``` r
ggplot() +
  geom_point(data = dat, aes(x = day, y = value, color = variable)) +
  stat_smooth(data = dat, aes(x = day, y = value, color = variable), method = 'loess') +
  labs(x = '', y = 'Degrees Fahrenheit', title = 'Daily temperatures in Boston and Los Angeles') +
  scale_color_manual(values = c('orange','blue'), labels = c('Los Angeles', 'Boston'), name = 'City')
```

    ## Warning: Removed 13 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 13 rows containing missing values (geom_point).

![](https://johnlray.github.io/2018/02/08/06.png)

Here, the only difference is that we've set initial arguments for x and y within the geometry calls themselves, rather than at the top level of `ggplot()`. This involves some redundant code that is possibly clarifying but ultimately unnecessary.

Now that we have an example under our belts, let's do... uh... another example. Let's return to our Virginia data. Now that we have a handle for customizing things in `ggplot2`, we can look at this plot again and recognize there are a lot of problems. We don't need the axis labels at all. We don't really need the axis tick marks, either. As such, we don't really need the lines behind the map and possibly not even that gray background. Virginia looks a little bit stretched.

Additionally, we are stuck with this sort of Mordor Gray fill by default. Even the pastel-errific defaults for color are a little prettier than that. Let's add in some data. Because this is ultimately an election project, I have made available the most recent election data for the Virginia State House [here](https://johnlray.github.io/2018/02/08/va_house.csv). We'll join that data to our shapefile data and use it to create the fill. You can either download the Virginia election data to your local machine or read it from the website url.

``` r
votes <- read.csv('https://johnlray.github.io/2018/02/08/va_house.csv')
glimpse(votes)
```

    ## Observations: 100
    ## Variables: 3
    ## $ state       <fctr> VA, VA, VA, VA, VA, VA, VA, VA, VA, VA, VA, VA, V...
    ## $ District    <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15,...
    ## $ Dem_percent <dbl> 0.00, 0.49, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0....

And recall from the previous episode that within our shapefile is an ordinary dataframe.

``` r
glimpse(shp@data)
```

    ## Observations: 100
    ## Variables: 9
    ## $ STATEFP  <fctr> 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, ...
    ## $ SLDLST   <fctr> 061, 014, 026, 093, 007, 051, 018, 005, 042, 019, 01...
    ## $ AFFGEOID <fctr> 620L500US51061, 620L500US51014, 620L500US51026, 620L...
    ## $ GEOID    <fctr> 51061, 51014, 51026, 51093, 51007, 51051, 51018, 510...
    ## $ NAME     <fctr> 61, 14, 26, 93, 7, 51, 18, 5, 42, 19, 17, 22, 31, 8,...
    ## $ LSAD     <fctr> LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, ...
    ## $ LSY      <fctr> 2016, 2016, 2016, 2016, 2016, 2016, 2016, 2016, 2016...
    ## $ ALAND    <fctr> 5004053111, 1030436974, 1186298583, 179263937, 21277...
    ## $ AWATER   <fctr> 159231775, 6565673, 2747407, 53284336, 26726910, 546...

We can join our election data to the shapefile just as we might join together any two dataframes. In the Virginia data our district names appear in a few fields: in `SLDLST`, in the last two characters of `AFFGEOID`, and in `NAME`. Right now `NAME` is a factor, which I will convert to a numeric.

``` r
shp@data$NAME <- shp@data$NAME %>% as.character() %>% as.numeric()
```

Note that I convert `NAME` to character *before* converting it to numeric, to ensure that the new numeric is the district numbers that I see, rather than the levels of the factor which may be (but are not necessarily) ordered differently. If a factor variable contains strings that could be read as numeric, you will typically want to convert to character *then* convert to numeric.

Next, to join them together, we will use the `left_join()` function from `dplyr` which we will learn more about in an episode or two. All of the `join()` functions out there exist to, wouldn't you guess, join two dataframes together, and here, we're going to consider the shapefile data to be the "left" side. We're "joining the vote data *to* the shapefile data," which puts the shapefile data on the left side. Its location in this sense is partly conceptual. We started with the shapefile data then added in the vote data, so imagine the shapefile data at the start of the sentence, on its *left* side, with the vote data at the end of the sentence, on its *right* side. This metaphor is not applicable to readers of Proust who, as you know, did not write sentences that end.

By name, the two dataframes do not have any variable names in common. But we know they have *data* in common: `NAME` in `shp@data` contains district numbers, and `District` in `votes` contains district numbers. We will tell `left_join()` that `NAME` and `district` are the variables that should glue `shp@data` and `votes` together.

``` r
shp@data = dplyr::left_join(shp@data, votes, by = c('NAME' = 'District'))
glimpse(shp@data)
```

    ## Observations: 100
    ## Variables: 11
    ## $ STATEFP     <fctr> 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, 51, 5...
    ## $ SLDLST      <fctr> 061, 014, 026, 093, 007, 051, 018, 005, 042, 019,...
    ## $ AFFGEOID    <fctr> 620L500US51061, 620L500US51014, 620L500US51026, 6...
    ## $ GEOID       <fctr> 51061, 51014, 51026, 51093, 51007, 51051, 51018, ...
    ## $ NAME        <dbl> 61, 14, 26, 93, 7, 51, 18, 5, 42, 19, 17, 22, 31, ...
    ## $ LSAD        <fctr> LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, LL, L...
    ## $ LSY         <fctr> 2016, 2016, 2016, 2016, 2016, 2016, 2016, 2016, 2...
    ## $ ALAND       <fctr> 5004053111, 1030436974, 1186298583, 179263937, 21...
    ## $ AWATER      <fctr> 159231775, 6565673, 2747407, 53284336, 26726910, ...
    ## $ state       <fctr> VA, VA, VA, VA, VA, VA, VA, VA, VA, VA, VA, VA, V...
    ## $ Dem_percent <dbl> 0.29, 0.00, 0.00, 0.55, 0.00, 0.00, 0.00, 0.00, 0....

Alright, it looks like the join worked. And with that in mind, let's plot. Here, instead of "color" we are going to fiddle with "fill" and "color" because we want to fill in our polygons rather than, say, color their borders. We will use the `Dem_percent` data to fill in our polygons and an arbitrary color to outline them. In the US context, "Dem" refers to the Democratic party, and "percent" refers to that party's two-party vote share in the most recent election.

One additional wrinkle is added along with our data. `ggplot()` has some defaults ready for plotting shapefiles, but we have to convert the shapefile data to a dataframe if we're going to plot more than the lat/long data. This is because plot methods exist for shapefile data that extends to the minimal components needed for a shapefile but not to, say, numeric data thrown in at the last second named `Dem_percent`. No other data type involves a conversion process like this and the reasoning behind the process' necessity is somewhat odd, so we will not dwel on it too long. The process is the same for any shapefile.

-   Create a new variable for the rownames of the shapefile, *not* for, say the "district ID" number as you might think of it
-   Use that new variable to `fortify()` the shapefile data into a dataframe
-   Join in the non-spatial data to the `fortify()`ed using the `id` field

This process is basically analagous to converting a shapefile to a big game of connect-the-dots, and then using a `group` variable (created by default by `fortify()`) to fill in the connected shapes. By convention, the new variable we create is called `id`. You can call the variable whatever you want.

``` r
shp@data$id = rownames(shp@data)
shpf = fortify(shp, by = 'id')
```

    ## Regions defined for each Polygons

``` r
shpf = dplyr::left_join(shpf, shp@data, by = 'id')
```

So, once we're done, we will plot `shpf` rather than `shp`.

``` r
ggplot(shpf, aes(x = long, y = lat, group = group, fill = Dem_percent), color = 'white') +
  geom_polygon(color = 'black', size = 0.3) +
  scale_fill_gradient(low = 'red', high = 'blue', name = 'Dem %', labels = scales::percent) +
  theme_void() +
  coord_equal()
```

![](https://johnlray.github.io/2018/02/08/07.png)

Note that I passed two arguments specifically to the `geom_polygon()` and not to the overall aesthetics of the plot. I set the color of the polygons to be the color "black," and I set the lines to be a little thinner than default. By placing `color = 'black', size = 0.3` outside of the top-level call to `ggplot()` and outside the `aes()` argument. Finally, that I threw in the `percent` argument to the "fill" piece of the plot code, so that the labels are rendered as percents rather than as decimals. The current version of `ggplot2` should load the `scales` library when imported into your `R` session.

In this session, we looked more deeply at `ggplot2`. We focused on the differences between variables and mappings, and we saw that data optimized for analysis is not necessarily optimized for visualization. I provided some election data for the purposes of this exercise. In the next episode I will show you how to get that data yourself, using webscraping.
