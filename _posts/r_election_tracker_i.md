I've been thinking of ways to make a full data course out of one big R project, and the recent [special election in Virginia](https://ballotpedia.org/Virginia_state_legislative_special_elections,_2017) provided just that way. On the day of the election I set up a [dedicated page](https://johnlray.github.io/2017/11/07/election_tracker.html) to gather, format, and plot the results of the election as vote tallies rolled in from the various voting sites across the state. The final version of this project required running one `R` script and committing the subsequent changes to git to update a large dataset gathered from open-source web resources, store and parse it efficiently, generate plots from that data, and then post those plots to the web.

For this project, we will be walking through how to create such a schema. I have chosen to break it down into a few smaller parts so that each unit is a discrete learning lesson on one useful topic. Those topics are:

-   Using R as a GIS
-   The use of the grammar of graphics with the `ggplot2` library
-   Acquiring unruly data from a public resource (webscraping)
-   Formatting unruly data
-   Deploying data visualizations and analysis to the web
-   Conducting statistical analysis
-   Putting together the final product

Thus, this post is the first in a series that will have six discrete lessons, and then a seventh to put it all together. My goal with seven lessons is to have a more or less finished product that can take the user from being a novice to being very good with `R` in one week's time. Specifically, I attempt to focus on certain important `R` skills that are common in practice but not taught in many introductory data science courses. Finally, the order of events may seem somewhat strange - ggplot before data storage? - but this series revolves around a real-life project I worked on and, if I could do it over again, this is the order in which I would have assembled the project.

To tie each lesson into the final product, here is how we are going to fit each of these lessons together:

-   First, we are going to visualize the geography whose elections we want to track - in this case, Virginia in the 2017 statewide election
-   Then, we are going to tinker with our visualization so that it displays our data in an appealing and informative fashion
-   We will get some real data from the Virginia Department of Elections to add to our visualizations...
-   ...then we will learn how to store that data and format it so its easy to work with
-   With our visualizations complete, we will learn how to deploy them to the web using Git and my preferred blogging venue, which is .io with a Jekyll framework
-   While this is not a statistical methods project, but a programming project, we will talk through some simple statistical analysis you can conduct in real-ish time, specifically the difference-in-means test
-   Finally, we will discuss the full workflow to think about how to optimize our production pipeline

I proceed on the following assumptions:
