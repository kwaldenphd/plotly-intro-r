# Getting Started With `plotly` in RStudio

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

Static visualizations, like those produced by `ggplot2`, provide a wealth of information, but don’t allow the viewer to actively interact with the data. This isn’t a problem if the visualization is to be printed in a report or on a poster, but it can be a limitation if the visual is to be used during a presentation, hosted on a webpage, or incorporated into an R Shiny application.

## Acknowledgements

This lab procedure is adapted from and based on Ryan Miller's ["Interactive and Animated Graphics Using plotly"](https://remiller1450.github.io/s230f19/plotly.html) (Fall 2019, Intro to Data Science STA 230 course, Grinnell College).

# Table of Contents

- [What is `plotly`?](#what-is-plotly)
- [Data and Environment Setup](#data-and-environment-setup)
  * [Packages](#packages)
  * [Data](#data)
- [Getting Started With `ggplotly`](#getting-started-with-ggplotly)
  * [`ggplotly` Package](#ggplotly-package)
  * [`plotly` Basics](#plotly-basics)
  * [Custom Hover Labels](#custom-hover-labels)
  * [Animations](#animations)
  * [OPTIONAL: 3D Graphics](#optional-3d-graphics)
- [Additional Resources](#additional-resources)
- [Additional Questions](#additional-questions)
- [Lab Notebook Questions](#lab-notebook-questions)

[Click here](https://raw.githubusercontent.com/kwaldenphd/plotly-intro-r/main/plotly-intro-r-markdown.Rmd) and select the "Save as" option to download this lab as an R Markdown file.

# What is `plotly`?

1. “plotly is an R package for creating interactive web-based graphs via the open source JavaScript graphing library plotly.js” [[Getting Started with Plotly in R]](https://plotly.com/r/getting-started/)

2. “Plotly's R graphing library makes interactive, publication-quality graphs. Examples of how to make line plots, scatter plots, area charts, bar charts, error bars, box plots, histograms, heatmaps, subplots, multiple-axes, and 3D (WebGL based) charts.” [[Plotly R Open Source Graphing Library]](https://plotly.com/r/)

3. “With ggplotly() by Plotly, you can convert your ggplot2 figures into interactive ones powered by plotly.js” [[Plotly ggplot2 Library]](https://plotly.com/ggplot2/)

4. The graphics package `plotly` is an open-source interactive graphing tool that interfaces with `R` and `ggplot`. 

5. Graphics created using the `plotly` package are compatible with existing tools like R Markdown and R Shiny, allowing for easy publishing and sharing.

# Data and Environment Setup

## Packages

6. We'll be using a number of `tidyverse` packages in this lab.
- [`plotly`](https://plotly.com/r/getting-started/) "is an R package for creating interactive web-based graphs via the open source JavaScript graphing library plotly.js"
- [`ggplot2`](https://ggplot2.tidyverse.org/) "is a system for declaratively creating graphics, based on The Grammar of Graphics. You provide the data, tell ggplot2 how to map variables to aesthetics, what graphical primitives to use, and it takes care of the details."
- [`tidyr`](https://tidyr.tidyverse.org/) "provides a set of functions that help you get to tidy data"
- [`stringr`](https://stringr.tidyverse.org/) "provide[s] a cohesive set of functions designed to make working with strings as easy as possible"
- [`forcats`](https://forcats.tidyverse.org/) "provide[s] a suite of tools that solve common problems with factors, including changing the order of levels or the values"

```R
# install.packages("plotly") # for interactive visuals
# install.packages("ggplot2") # for static visuals
# install.packages("tidyr") # for  tidying data
# install.packages("stringr") # for processing character strings
# install.packages("forcats") # for processing categorical data

library(plotly)
library(ggplot2)
library(tidyr)
library(stringr)
library(forcats)
```

7. These packages are part of what is known as "the Tidyverse," in the RStudio user community.

9. According to https://www.tidyverse.org/, "the tidyverse is an opinionated collection of R packages designed for data science. All packages share an underlying design philosophy, grammar, and data structures."

10. To install and load all packages that are part of the `tidyverse` core, you can install the parent tidyverse package.

```R
install.package("tidyverse")
library(tidyverse)
```

11. As of December 2020, the `tidyverse` core incldues the following packages: 
- `ggplot2`
- `dplyr`
- `tidyr`
- `readr`
- `purrr`
- `tibble`
- `stringr`
- `forcats`

## Data

12. In this tutorial, we will use the `AmesHousing` data, which provides information on the sales of individual residential properties in Ames, Iowa from 2006 to 2010. 

13. The data set contains 2930 observations, and a large number of explanatory variables involved in assessing home values. 

14. [Click here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt) for a full description of this dataset.

```R
# The csv file should be imported into rstudio:
AmesHousing <- read.csv("https://raw.githubusercontent.com/kwaldenphd/plotly-intro-r/main/data/AmesHousing.csv")
# str(AmesHousing)
```

# Getting Started With `ggplotly`

### `ggplotly` package

15. Before learning any of the syntax used by `plotly`, we’ll first use the package to add interactivity to an existing graphic created by `ggplot`. 

16. The `ggplotly` function accepts any `ggplot` object and renders it into an interactive graphic.

17. In the example below, we create a simple scatterplot describing the Ames Housing data, then make it interactive with the `ggplotly` function. 

18. You should notice two key features have been added to the plot:
- A dashboard that allows users to zoom in/out and rescale the plot.
- Popup information at appears when you hover over a data-point.

```R
AmesHousing <- read.csv("https://raw.githubusercontent.com/kwaldenphd/plotly-intro-r/main/data/AmesHousing.csv")

my_ggplot <- ggplot(data=AmesHousing) + geom_point(mapping= aes(x=GrLivArea, y=SalePrice))
ggplotly(my_ggplot)
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_1.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_1.png?raw=true" /></a></p>

19. Let’s now add some complexity and color each data-point by building type:

```R
my_ggplot <- ggplot(data=AmesHousing) + geom_point(mapping= aes(x=GrLivArea, y=SalePrice, color = BldgType))

ggplotly(my_ggplot)
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_2.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_2.png?raw=true" /></a></p>

20. You can click on a building type in the legend to remove that category of data-points from the plot.

21. You now see building type, in addition to x-y coordinates, when you hover over a data-point.

# `plotly` basics

22. `ggplotly` is a simple and effective way to augment an existing `ggplot` graphic, but in order to access the more advanced features of plotly we’ll need to construct graphics from scratch using the `plot_ly` function.

23. The code below illustrates one way to create our previous scatterplot using `plot_ly`:
```R
plot_ly(data = AmesHousing, type = "scatter",  mode = "markers", x = ~GrLivArea, y = ~SalePrice, color = ~BldgType)
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_3.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_3.png?raw=true" /></a></p>

24. Here, the argument `type = "scatter"` tells plotly to create a scatterplot, and the argument `mode = "markers"` plots the data-points as hoverable dots (rather than text labels or other characters).

25. You might also notice the `∼` character used in front of the variables we reference in the `x`, `y`, and `color` arguments. 

26. This character tells `plot_ly` to look for that variable inside of the data.frame supplied in the `data` argument. 

27. Had we omitted it, `plot_ly` would look for vectors with those names in our global `R` environment.

28. Similar to `ggplot`, we can build-up `plot_ly` graphics by adding layers, though this time layers are added using the `%>%` operator. 

29. In the example below, we add a scatterplot layer using a trace with `type = "scatter"`, we then add another layer of lines to the plot (for illustration, this second layer of lines doesn’t have any practical use):

```R
plot_ly(data = AmesHousing) %>% 
  add_trace(type = "scatter",  x = ~GrLivArea, y = ~SalePrice, color = ~BldgType) %>%
  add_lines(x = ~GrLivArea, y = ~SalePrice, color = ~BldgType) 
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_4.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_4.png?raw=true" /></a></p>

30. To see the full range of `plotly` `trace` options (valid inputs to the type argument) visit [the reference page](https://plot.ly/r/reference/). 

31. The different options appear in the navigation panel on the left side of the page, you can click on the type of graph you’re interested in to jump to that section of the documentation.

<blockquote>Q1: Create an R Markdown document and delete the existing sections/code chunks, then add a section (defined by ## Question 1). In a code chunk within this section, load the AmesHousing data and use plotly to create a violin plot displaying the distribution of sale prices for each different house style in the Ames housing data. (Hint: you should use the reference page to learn how to add a violin plot trace).</blockquote>

# Custom Hover Labels

32. The labels that appear when you hover over a data-point are one of the most useful features of `plotly`. 

33. They can be controlled via the `text` argument, which can be given within the `plot_ly` function, or within `add_trace`. 

34. In the example below, we label each home sale using its property id (`PID`):
```R
plot_ly(data = AmesHousing, type = "scatter",
        x = ~GrLivArea, y = ~SalePrice, color = ~BldgType,
        text = ~PID)
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_5.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_5.png?raw=true" /></a></p>

35. `plotly` labels are constructed using hypertext markup langauage, or `HTML`, meaning we can customize their appearance using HTML markup:

```R
plot_ly(data = AmesHousing, type = "scatter",
        x = ~GrLivArea, y = ~SalePrice, color = ~BldgType,
        text = ~paste0("This home was built in: ", YearBuilt, "<br> It was last sold in: ", YrSold))
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_6.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_6.png?raw=true" /></a></p>

36. The `paste0` function is used to combine character strings and variable values into a single string. 

37. The text `< br >` is the HTML code to begin a new line. 

38. A few other useful HTML tags:

Tag | Explanation
--- | ---
`< b > my text < / b >` | Bolds the text in between the tags
`< i > my text < / i >` | Italicizes the text in between the tags
` x < sub > i < / sub >` | Adds a subscript, in this case we get x<sub>i</sub>

39. Along with ` < br >`, the commands indicated above are the most common HTML commands that you might use in label. 

40. If your desired labels require something else, [Stanford's HTML cheatsheet](https://web.stanford.edu/group/csp/cs21/htmlcheatsheet.pdf) is a useful place to start.

41. Something you might have noticed is that our custom text was added to default hover info. 

42. If we wanted to only include our custom label, and not the default information, we could use the argument `hoverinfo = "text"`:

```R
plot_ly(data = AmesHousing, type = "scatter",
        x = ~GrLivArea, y = ~SalePrice, color = ~BldgType,
        hoverinfo = 'text',
        text = ~paste("This home was built in:", YearBuilt, "<br>", "It was last sold in:", YrSold))
```

<blockquote>Q2: The code below uses `plotly` to create a piechart of different house styles. Notice what happens when you click on a category in the chart’s legend! Modify this code to include custom hovertext displaying the house style and that style’s median sale price on separate lines. (Hint: you should use the group_by and summarize functions in the dplyr package to get the information needed for your labels).</blockquote>

```R
prop = table(AmesHousing$HouseStyle)/nrow(AmesHousing)
style = names(prop)

plot_ly() %>%
  add_trace(type = "pie", labels = ~style, values = ~prop, textinfo = "percent")
```
<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_7.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_7.png?raw=true" /></a></p>

# Animations

43. `plotly` graphics can be animated using the `frame` argument where each frame indicates a snapshot in time at which the graphic is to be created. 

44. The example below shows a barchart of US state populations that is animated to show changes from 2010 to 2018.

```R
## Load the data
states <- read.csv("https://raw.githubusercontent.com/kwaldenphd/plotly-intro-r/main/data/state_pops.csv")

## Tidy the data
states_long <- gather(states, key = "Year", value = "Population", 2:ncol(states))
states_long$Year <- str_replace(string = states_long$Year, pattern = "X", replace = "")
states_long$State <- str_replace(string = states_long$State, pattern = ".", replace = "")

## Plotly animation
plot_ly(data = states_long, type = "bar",
        x = ~fct_reorder(State, Population), y = ~Population, frame = ~Year, showlegend = FALSE) 
```

45. Before plotting we first needed to tidy these data, putting them into “long” format.

46. The `frame` argument is used to control the animation. Each “frame” of this animation corresponds with a particular year.

47. We used the `fct_reorder` function in the `forcats` package to reorder the states according to their population.

48. Generally speaking, any type of visual can be animated by supplying using the `frames` argument, and there isn’t a requirement that each data-point exists within every frame. 

49. The example below demonstrates a creative use of the `frame` argument where trends in the size of new homes are displayed over time.

```R
plot_ly(data = AmesHousing, type = "scatter", mode = "markers", 
        x = ~GrLivArea, y = ~TotRmsAbvGrd, frame = ~round(YearBuilt,-1),
        showlegend = FALSE) %>%
   animation_opts(frame = 1000, easing = "elastic", redraw = FALSE)
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_13.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_13.png?raw=true" /></a></p>

50. This example also illustrates a couple of additional animation features, which are modified using the `animation_opts` function:

51. The speed at which the animation progresses is controlled by the `frame` argument (confusing, I know). 

52. The default value is 500 milliseconds, but in this example we increase it to 1000 milliseconds, resulting in slower transitions between frames.

53. The way by which frames transition can be changed via the `easing` argument, here `easing = "elastic"` causes the points to bounce when a new frame occurs. 

54. Different easing options [can be found here](https://github.com/plotly/plotly.js/blob/master/src/plots/animation_attributes.js), with the option names being listed at approximately line 80.

55. The `redraw = FALSE` argument can improve the performance of laggy animations by not entirely re-drawing the plot at each transition. 

56. However, in this example, it doesn’t make much difference.

# OPTIONAL: 3D Graphics

57. `plotly` allows us to graph in three-dimensions, something that `ggplot` cannot accommodate. 

58. The code below creates a simple 3D scatterplot:

```R
plot_ly(data = AmesHousing, type = "scatter3d", mode = "markers",
        x = ~GrLivArea, y = ~SalePrice, z = ~OverallQual)
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_8.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_8.png?raw=true" /></a></p>

59. `plotly` can also display surfaces. 

60. The code below displays the estimated two-dimensional density of overall quality and year built:

```R
# install.packages("MASS")
library(MASS)
kd <- kde2d(AmesHousing$YearBuilt, AmesHousing$OverallQual, n = 50)

plot_ly() %>% add_surface(x = ~kd$x, y = ~kd$y, z = ~kd$z, showscale = FALSE) %>% 
  layout(scene = list(xaxis = list(title = "Year Built"), 
                      yaxis = list(title = "Overall Quality"), 
                      zaxis = list(title = "Density")))
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_9.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_9.png?raw=true" /></a></p>

61. In this example, we see that changing the labels of axes in `plotly` is quite cumbersome, requiring the use of nested lists within the layout function. 

62. In order to reduce the amount code that is shown, the remaining examples in this tutorial will not format axis labels or scales. 

63. However, you should always neatly format your axes and labels when using `plotly` on a project.

64. Perhaps more useful than 3D representations of two-dimensional densities are fitted regression planes:

```R
model <- lm(SalePrice ~ YearBuilt + OverallQual, data = AmesHousing)

xs <- seq(1900, 2020, by = 10)

ys <- seq(1,10, by = 1)

grid <- expand.grid(xs,ys)

names(grid) <- c("YearBuilt", "OverallQual")

z <- predict(model, newdata = grid)

m <- matrix(z, nrow = length(unique(grid$YearBuilt)), ncol = length(unique(grid$OverallQual)))

plot_ly() %>% add_surface(x = ~xs, y = ~ys, z = ~m, colors = c("#d1d1d1", "#000000")) %>% 
  add_markers(x = ~AmesHousing$YearBuilt, y = ~AmesHousing$OverallQual, z = ~AmesHousing$SalePrice, colors = I("blue"))
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_10.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_10.png?raw=true" /></a></p>

65. Let's walk through this line by line.

66. To begin, `plotly` creates a surface using a grid of x-y coordinates and a matrix of heights (z coordinates). 

67. In the z matrix, the element in position `{i,j}` corresponds with the height of the surface at the location defined by `xi` and `yj`, the respective `ith` and `jth` elements of the x and y vectors.

68. For us to use this information to construct the regression plane, we first created sequences of x and y values that we wanted to plot over. 

69. We then used the `expand.grid` function to create matrix containing all possible pairing of these values, as we needed heights for each x-y coordinate. 

70. We used the `predict` function to obtain model predictions for each of these possible x-y pairings, storing the predictions in a matrix with appropriate dimensions (such that position `{i,j}` contains the height that plotly is expecting).

71. The code provided above is pretty general and can be easily adapted to different applications simply by changing the variable names and the sequences of values that are plotted over.

72. Looking more closely at this regression plane, you’ll notice that the effects of year and quality on sale price don’t appear to be linear. 

73. Fortunately, we can use the same general procedure to plot the prediction surface of nearly any model.

74. The plot below displays what the regression surface looks like using a generalized additive model (GAM), a type of model that adds flexibility to allow non-linear relationships between predictors and the outcome using splines:

```R
# install.packages("splines")
# install.packages("mgcv")

library(splines)
library(mgcv)
model <- gam(SalePrice ~ s(YearBuilt) + s(OverallQual), data = AmesHousing)

xs <- seq(1900, 2020, by = 10)
ys <- seq(1,10, by = 1)
grid <- expand.grid(xs,ys)
names(grid) <- c("YearBuilt", "OverallQual")
z <- predict(model, newdata = grid)
m <- matrix(z, nrow = length(unique(grid$YearBuilt)), ncol = length(unique(grid$OverallQual)))

plot_ly() %>% add_surface(x = ~xs, y = ~ys, z = ~m, colors = c("#d1d1d1", "#000000")) %>% 
  add_markers(x = ~AmesHousing$YearBuilt, y = ~AmesHousing$OverallQual, z = ~AmesHousing$SalePrice, color = I("red"))
```

<p align="center"><a href="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_11.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/plotly-intro-r/blob/main/figures/Figure_11.png?raw=true" /></a></p>

75. Within the `gam` function, you’ll notice that predictors are specified within the `s` function, which specifies that spline should be used to smooth the relationship between that predictor and the outcome (see the presentation on smoothing if this doesn’t sound familiar).

<blockquote>Q4: Modify the code given below to display the linear regression model that predicts SalePrice using linear regression model with LotArea and GrLivArea as predictors. Note that you’ll need to change how the xs and ys sequences are defined, as well as many of the variable names that are referenced.</blockquote>

```R
model <- lm(SalePrice ~ YearBuilt + OverallQual, data = AmesHousing)
xs <- seq(1900, 2020, by = 10)
ys <- seq(1,10, by = 1)
grid <- expand.grid(xs,ys)
names(grid) <- c("YearBuilt", "OverallQual")
z <- predict(model, newdata = grid)
m <- matrix(z, nrow = length(unique(grid$YearBuilt)), ncol = length(unique(grid$OverallQual)))

plot_ly() %>% add_surface(x = ~xs, y = ~ys, z = ~m, colors = c("#d1d1d1", "#000000")) %>% 
  add_markers(x = ~AmesHousing$YearBuilt, y = ~AmesHousing$OverallQual, z = ~AmesHousing$SalePrice, colors = I("green"))
```

# Additional Resources

76. Additional `plotly` resources:
- plotly, ["R Figure Reference: Single Page"](https://plot.ly/r/reference/): A reference guide for different plotly traces and the attributes you can modify. Very useful for learning things like “how to not display the percentages on my piechart?” or “how to plot text instead of points on a scatterplot?”
- Carson Sievert, [*Interactive web-based visualization with R, plotly, and shiny*](https://plotly-r.com/) (CRC Press, 2019).
- plotly, ["Plotly R Open Source Graphing Library"](https://plotly.com/r/): A library of examples for many different types of plotly graphics

# Additional Questions

Q3: Load a sport-focused data set of your choosing into RStudio. For this question you should create a plot of your choosing that highlights something you deem to be an important or interesting trend in these data.

# Lab Notebook Questions

Q1: Create an R Markdown document and delete the existing sections/code chunks, then add a section (defined by ## Question 1). In a code chunk within this section, load the AmesHousing data and use plotly to create a violin plot displaying the distribution of sale prices for each different house style in the Ames housing data. (Hint: you should use the reference page to learn how to add a violin plot trace).

Q2: The code below uses `plotly` to create a piechart of different house styles. Notice what happens when you click on a category in the chart’s legend! Modify this code to include custom hovertext displaying the house style and that style’s median sale price on separate lines. (Hint: you should use the group_by and summarize functions in the dplyr package to get the information needed for your labels).

Q3: Load a sport-focused data set of your choosing into RStudio. For this question you should create a plot of your choosing that highlights something you deem to be an important or interesting trend in these data.

***optional*** Q4: Modify the code given below to display the linear regression model that predicts SalePrice using linear regression model with LotArea and GrLivArea as predictors. Note that you’ll need to change how the xs and ys sequences are defined, as well as many of the variable names that are referenced.
