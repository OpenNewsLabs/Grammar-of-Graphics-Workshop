The Grammar of Graphics: Designing and Transforming Data
========================================================
author: Sandhya Kambhampati & Christine Zhang 
date: April 8, 2016
font-family: 'Helvetica'

What is R?
========================================================
<br>
"R is a language and environment for statistical computing and graphics" 

Source: R-project.org 

What is ggplot2?
========================================================
* *ggplot2* is a data visualization package for R
  + developed by Hadley Wickham
  + inspired by Leland Wilkinson's "The Grammar of Graphics"
  + provides an organizing philosophy for building graphs -- a structured approach to graphing
</br>

> "The emphasis in *ggplot2* is reducing the amount of thinking time by making it easier to go from the plot in your brain to the plot on the page."

> Wickham, 2012

The philosophy of ggplot2
========================================================
* A *ggplot2* graph is built up from a few basic elements:
  + **Data**: the raw data you want to plot
  + **Aesthetics**: including **Mapping** e.g., which variable is on the x-axis? the y-axis? Should the color/size/position of the plotted data that be mapped to some variable?
  + **Geometries**: the geometric shapes that represent the data
  + **Statistics**: statistical transformations that are used to summarize the data

Source: [Hopper (2014)] (http://tomhopper.me/2014/03/28/a-simple-introduction-to-the-graphing-philosophy-of-ggplot2/)

How to install ggplot2
========================================================
<br>

```r
install.packages('ggplot2')
library('ggplot2')
```

<br>
Make sure you have the most recent version of R to get the most recent version of *ggplot2*

The Data: US States
========================================================

```r
data(state)
states.data <- as.data.frame(state.x77)
states.data$states <- rownames(states.data)
colnames(states.data)[4] <- "Life.Exp"
```


|           | Population| Income| Illiteracy| Life.Exp| Murder| HS Grad|states     |
|:----------|----------:|------:|----------:|--------:|------:|-------:|:----------|
|Alabama    |       3615|   3624|        2.1|    69.05|   15.1|    41.3|Alabama    |
|Alaska     |        365|   6315|        1.5|    69.31|   11.3|    66.7|Alaska     |
|Arizona    |       2212|   4530|        1.8|    70.55|    7.8|    58.1|Arizona    |
|Arkansas   |       2110|   3378|        1.9|    70.66|   10.1|    39.9|Arkansas   |
|California |      21198|   5114|        1.1|    71.71|   10.3|    62.6|California |
|Colorado   |       2541|   4884|        0.7|    72.06|    6.8|    63.9|Colorado   |

The Data: US States
========================================================
<br>
* **Population**: population estimate as of July 1, 1975
* **Income**: per capita income (1974)
* **Illiteracy**: illiteracy (1970, percent of population)
* **Life Exp**: life expectancy in years (1969–71)
* **Murder**: murder and non-negligent manslaughter rate per 100,000 population (1976)
* **HS Grad**: percent high-school graduates (1970)
</br>
<br>
Source: US Census Bureau via [R datasets](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/state.html)

How to plot your data: quick plot
========================================================




```r
qplot(Income, Life.Exp, data = states.data)
```

<img src="ggplot2_pres-figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

How to plot your data: quick plot
========================================================


```r
qplot(Income, Life.Exp, data = states.data, main = "Income v. Life Expectancy")
```

<img src="ggplot2_pres-figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

Basic Features of ggplot2
========================================================
* Scatter Plot
* Line Graph
* Bar Graph

Scatter Plot
========================================================


```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() +
  ggtitle("Income v. Life Expectancy")
```

Scatter Plot
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

Scatter Plot - start x-axis at 1000
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() +
  scale_x_continuous(limits = c(1000, 7000))
```

Scatter Plot - start x-axis at 1000
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" style="display: block; margin: auto;" />

Scatter Plot - start x-axis at 1000 (breaks every 1000), y-axis at 50
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() +
  scale_x_continuous(limits = c(1000, 7000), 
                     breaks = seq(1000, 7000, 1000)) +
  scale_y_continuous(limits = c(60, 75))
```

Scatter Plot - start x-axis at 1000 (breaks every 1000), y-axis at 50
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" style="display: block; margin: auto;" />

Scatter Plot - color mapped to state
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp, 
           color = states)) + 
  geom_point() 
```

Scatter Plot - color mapped to state
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-14-1.png" title="plot of chunk unnamed-chunk-14" alt="plot of chunk unnamed-chunk-14" height="600px" style="display: block; margin: auto;" />


![Line types](https://www.dropbox.com/s/j6qxzorecdxity4/line_types.png?dl=0)
plotsymbols.png



Line Graph
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() + 
  geom_line()
```

Line Graph
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" style="display: block; margin: auto;" />

Line Graph - specific color/size
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp, 
           color = states)) + 
  geom_point(color = 'red', size = 5) + 
  geom_line(color = 'blue')
```

Line Graph - specific color/size
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

Line Graph - points on top, different line type & point shape
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp) + 
  geom_line(color = 'blue', 
            linetype = 'dashed') +
  geom_point(color = 'red', 
             size = 2, 
             shape = 15) 
```

Line Graph - points on top, different line type & point shape
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />

Bar Graph - simple
========================================================

```r
three.states <- states.data[states.data$states == "California" | states.data$states == "New York" | states.data$states == "Texas", ]
```

* We picked three states from the data: California, New York, and Texas


|           | Population| Income| Illiteracy| Life.Exp| Murder| HS Grad|states     |
|:----------|----------:|------:|----------:|--------:|------:|-------:|:----------|
|California |      21198|   5114|        1.1|    71.71|   10.3|    62.6|California |
|New York   |      18076|   4903|        1.4|    70.55|   10.9|    52.7|New York   |
|Texas      |      12237|   4188|        2.2|    70.90|   12.2|    47.4|Texas      |


Bar Graph - simple
========================================================

```r
ggplot(data = three.states, 
       aes(x = states, 
           y = Population)) +
  geom_bar(stat = 'identity')
```

Bar Graph - simple
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-24-1.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" style="display: block; margin: auto;" />

Bar Graph - simple, transparent background
========================================================

```r
ggplot(data = three.states, 
       aes(x = states, 
           y = Population)) +
  geom_bar(stat = 'identity') +
  theme(panel.background = element_blank())
```

Bar Graph - transparent background
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-26-1.png" title="plot of chunk unnamed-chunk-26" alt="plot of chunk unnamed-chunk-26" style="display: block; margin: auto;" />

Bar Graph - stacked
========================================================

```r
three.states$Category <- ifelse(three.states$states == "Texas", "Texas", "California/New York")
```
* We created a variable, *Category*, that places the data into two categories: "Texas" if it's from Texas, and "California/New York" if it's from California or New York


|           | Population| Income| Illiteracy| Life.Exp|Category            |
|:----------|----------:|------:|----------:|--------:|:-------------------|
|California |      21198|   5114|        1.1|    71.71|California/New York |
|New York   |      18076|   4903|        1.4|    70.55|California/New York |
|Texas      |      12237|   4188|        2.2|    70.90|Texas               |

Bar Graph - stacked
========================================================

```r
ggplot(data = three.states, 
       aes(x = Category, 
           fill = states)) +
  geom_bar(position = 'stack') +
  theme(panel.background = element_blank())
```

Bar Graph - stacked
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-30-1.png" title="plot of chunk unnamed-chunk-30" alt="plot of chunk unnamed-chunk-30" style="display: block; margin: auto;" />

Bar Graph - stacked
========================================================

```r
ggplot(data = three.states, 
       aes(x = Category, 
           y = Illiteracy,
           fill = states)) +
  geom_bar(stat = 'identity', 
           position = 'stack') +
  theme(panel.background = element_blank())
```

Bar Graph - stacked
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-32-1.png" title="plot of chunk unnamed-chunk-32" alt="plot of chunk unnamed-chunk-32" style="display: block; margin: auto;" />

Advanced Features
========================================================
* Facetting
* Fitted Lines
* Text Labels

Facetting
========================================================

* Facetting allows you to split up your data by one or two variables
* *facet_grid()* places one or two variables in either vertical or horizontal directions
* *facet_wrap()* places facets next to each other, wrapping with a certain # of rows and/or columns

```
facet_grid(vertical ~ horizontal)

facet_wrap(~ variable, nrow = ___, ncol = ___)
```

Facetting
========================================================


```r
states.data$Pop.Under.2000 <- ifelse(states.data$Population < 2000, "Pop Under 2000", "Pop Above 2000")
```
<br>
* We created a variable, *Pop.Under.2000*, which shows whether the population (in thousands) is under 2000 or above 2000

Facetting
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() +
  facet_grid(. ~ Pop.Under.2000)
```

Facetting
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-35-1.png" title="plot of chunk unnamed-chunk-35" alt="plot of chunk unnamed-chunk-35" style="display: block; margin: auto;" />

Fitted Lines
========================================================
* How do we draw a fitted line through these points?

<img src="ggplot2_pres-figure/unnamed-chunk-36-1.png" title="plot of chunk unnamed-chunk-36" alt="plot of chunk unnamed-chunk-36" style="display: block; margin: auto;" />

Fitted Lines
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() +
  stat_smooth()
```

Fitted Lines
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) + 
  geom_point() +
  stat_smooth(method = 'lm') 
```


Fitted Lines
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-39-1.png" title="plot of chunk unnamed-chunk-39" alt="plot of chunk unnamed-chunk-39" style="display: block; margin: auto;" />

Text Labels 
========================================================

```r
ggplot(data = states.data, 
       aes(x = Income, 
           y = Life.Exp)) +
        geom_text(aes(label = states)) 
```

Text Labels
========================================================
<img src="ggplot2_pres-figure/unnamed-chunk-41-1.png" title="plot of chunk unnamed-chunk-41" alt="plot of chunk unnamed-chunk-41" style="display: block; margin: auto;" />

The philosophy of ggplot2 - recap
========================================================
* A *ggplot2* graph is built up from a few basic elements:
  + **Data**: the raw data you want to plot
  + **Aesthetics**: including **Mapping** e.g., which variable is on the x-axis? the y-axis? Should the color/size/position of the plotted data that be mapped to some variable?
  + **Geometries**: the geometric shapes that represent the data
  + **Statistics**: statistical transformations that are used to summarize the data

Source: [Hopper (2014)] (http://tomhopper.me/2014/03/28/a-simple-introduction-to-the-graphing-philosophy-of-ggplot2/)

Resources
======================================================
* ["A Layered Grammar of Graphics"](http://vita.had.co.nz/papers/layered-grammar.html) by Hadley Wickham

* ["A Simple Introduction to the Graphing Philosophy of ggplot2"](http://tomhopper.me/2014/03/28/a-simple-introduction-to-the-graphing-philosophy-of-ggplot2/) by Tom Hopper

* [ggplot2 Cheat Sheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf) by RStudio

* [ggplot2 Documentation](http://docs.ggplot2.org/current/) by Hadley Wickham & Winston Chang

* [Resources for Doing Data Journalism](http://rddj.info/) by Timo Grossenbacher

* [Shapes and Line Types in R](http://www.cookbook-r.com/Graphs/Shapes_and_line_types/) by Winston Chang

Thank You + Questions
======================================================
<br>

<center>
Sandhya Kambhampati    
[@sandhya__k](https://twitter.com/sandhya__k)

Christine Zhang    
[@christinezhang](https://twitter.com/christinezhang)
</center>
