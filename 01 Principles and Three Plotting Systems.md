* Principles of analysis graphics
* Exploratory Graphics
* Plotting
* Base Plotting System
* Graphics Devies

#Principles of analysis graphics

**Principle 1: Show corparisons**   
 + Evidence for a pyhothesis is always relative to another competing yhpothesis.  
 + Always ask “Compared to What?”

**Principle 2: Show causality, mechanism, explanation, systematic structure**  

 + What is your causal framework for thinking about a question?

**Principle 3: Show multivariate data** 

 + multivariate = more than 2 variables
 + the real world is multivariate
 + need to ‘escape flatland’
 
**Principle 4: Integration of evidence** 
 + completely integrate words, numbers, images, diagrams
 + data graphics should make use of many modes of data presentation.
 + don’t let the tool drive the analysis.  
 
**Principle 5: Describle and document the evidence with appropriate labels, scales, scources, et al.** 
 + a data graphic should tell a complete story that is credible.
 
**Principle 6: Content is king**
 + analytical presentations ultimately stand or falling depend on the quanlity, relerance, and integrity of their content.

**Summary**
* Priniple 1: Show Comparisons
* Principle 2: Show causality, mechnism, explanation
* Principle 3: Show multivariate data
* Principle 4: Integrate multiple modes of evidence
* Principle 5: Describe and document the evidence
* Principle 6: Content is king
 
#Exploratory Graphics

**Why do we use graphic in data analysis**

* To understand data properties
* To find partterns in data
* To suggest modeling strategies
* To ‘debug’ analysis
* To communicate results

**Characteristics of exploratory graphics**

* They are made quickly
* A large number are made
* The goal is for personal understanding
* Axes/legends are generally cleaned up
* Colors/size are primarily used for information

# download data set form internet
```r
setwd('E:\\Dropbox\\coursera\\Exploratory Data Analysis')
if(!file.exists('data')) dir.create('data')
fileUrl <- 'https://raw.githubusercontent.com/jtleek/modules/master/04_ExploratoryAnalysis/exploratoryGraphs/data/avgpm25.csv'
download.file(fileUrl, './data/pollution.csv')
# read data
pollution <- read.csv('./data/pollution.csv', header = TRUE)
head(pollution)
```
**Simple Summary of Data**

* One dimension 
 + Five-number summary (Min, 1st Qu., Median, 3rd Qu., Max)
 + Boxplot
 + Histograms
 + Density plot
 + Barplot
* Two dimesions 
 + Multiple/overlayed 1-D plots(Lattice/ggplot2)
 + Scatterplot
 + Smooth scatter plot
* More than two dimensions 
 + Ovelayed/multiple 2-D plots,coplots
 +  + Use color, size, shape to add dimesions
 + Spinning plots
 + Actual 3-D plots(not that useful)
```r
# Five number summary
summary(pollution$pm25)
# Boxplot
boxplot(pollution$pm25, col = 'blue')
# Histograms
hist(pollution$pm25, col = 'green')
rug(pollution$pm25)
hist(pollution$pm25, col = 'green', breaks = 100)
rug(pollution$pm25) 
# Overlayng features
boxplot(pollution$pm25, col = 'blue')
abline(h = 12)
hist(pollution$pm25, col = 'green')
abline(v = 12, lwd = 2)
abline(v = median(pollution$pm25), col = 'red')
# Barplot
barplot(table(pollution$region), col = 'wheat', main = 'NO. of counties in each region')
# Multiple boxplot
boxplot(pm25 ~ region, data = pollution, col = 'red')
# multiple histograms
par(mfrow = c(2,1), mar = c(4, 4, 2, 1))
hist(subset(pollution, region == 'east')$pm25, col = 'green')
hist(subset(pollution, region == 'west')$pm25, col = 'green')
# Scatter plot
par(mfrow = c(1,1))
with(pollution, plot(latitude, pm25))
abline(h = 12, lwd = 2, lty = 2)
# scatter plot using color
with(pollution, plot(latitude, pm25, col = region))
abline(h = 12, lwd = 2, lty = 2)
# multiple scatterplots
par(mfrow = c(1, 2), mar = c(5, 4, 2, 1))
with(subset(pollution, region == 'east'), plot(latitude, pm25), main= 'East')
with(subset(pollution, region == 'west'), plot(latitude, pm25), main = 'West')
```

**Summary**

* Exploratory plots are ‘quick and dirty’
* Let you summarize the data(usually graphically) and highly any broad features
* Explore basic questions and hypotheses(and perhaps rule them out)
* Suggest modeling strategies for the ‘next step’

#Plotting

There are three plotting systems in R

**The basic plotting system**

* ’Artist’s palette’ model
* Start with blank canvas and build up from there
* Start with plot function (or similar)
* Use annotation functions to add/modify(text, lines, points, axis)

(Pros) Convenient, mirrors how we think of building plots and analysis
(Cons) Can’t go back one plot has started(i.e. to adjust margins)
(Cons) Difficult to ‘translate’ to others once a new plot has been created
(Cons) Plot is just a series of R commands

**The Lattive system**

(Pros) Plots are created with a single functin cal (xyplot, bwplot, etc)
(Pros) Most useful for conditioning types of plots: looking at how y changes with x cross levels of z
(Pros) Things like margins/spacing set automatically because entire is specified at once
(Pros) Good for putting many many plots on a screen
(Cons) Sometimes arkward to specify an entire plot in a single function call
(Cons) Annotation in plot is not especially intuitive
(Cons) Use of panel functions and subscripts difficult to wield and requires intense preparation.
(COns) Cannot ‘add’ to the plot once it is created
```r
library(lattice)
state <- data.frame(state.x77, region = state.region)
xyplot(Life.Exp ~ Income | region, data = state, layout = c(4,1))
#Y-axis is Life.Exp; X-axis is Income; 1 row and 4 columns
```
**The ggplot2 system**

* Splits the difference between base and lattive in a number of ways
* Automatically deals with spacing, text, titles but also allows you to annotate by ‘adding’ to a plot
* Superficial similarity to lattice but generaly easier/more intuitive to use
* Default mode makes many choices for you (but you can still customize to your heart’s desire)
```r
library(ggplot2)
data(mpg)
qplot(displ, hwy, data = mpg) # X-axis is displ; Y-axis is hwy
```
**Summary**

* Base: ‘artist’s palette’ model
* Lattice: Entire plot specified by one function; conditioning
* ggplot2: Mixed elements of Base and Lattice

