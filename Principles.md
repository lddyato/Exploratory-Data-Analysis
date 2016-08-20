* Principles of analysis graphics
* Exploratory Graphics
* Plotting
* Base Plotting System
* Graphics Devies

#Principles of analysis graphics

* Principle 1: Show corparisons 
Evidence for a pyhothesis is always relative to another competing yhpothesis.
Always ask “Compared to What?”
* Principle 2: Show causality, mechanism, explanation, systematic structure 
What is your causal framework for thinking about a question?
* Principle 3: Show multivariate data 
multivariate = more than 2 variables
the real world is multivariate
need to ‘escape flatland’
* Principle 4: Integration of evidence 
completely integrate words, numbers, images, diagrams
data graphics should make use of many modes of data presentation.
don’t let the tool drive the analysis.
* Principle 5: Describle and document the evidence with appropriate labels, scales, scources, et al. 
a data graphic should tell a complete story that is credible.
* Principle 6: Content is king 
analytical presentations ultimately stand or falling depend on the quanlity, relerance, and integrity of their content.
#Exploratory Graphics

Why do we use graphic in data analysis

* To understand data properties
* To find partterns in data
* To suggest modeling strategies
* To ‘debug’ analysis
* To communicate results

Characteristics of exploratory graphics

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
Simple Summary of Data

* One dimension 
Five-number summary
Boxplot
Histograms
Density plot
Barplot
* Two dimesions 
Multiple/overlayed 1-D plots(Lattice/ggplot2)
Scatterplot
Smooth scatter plot
* More than two dimensions 
Ovelayed/multiple 2-D plots,coplots
Use color, size, shape to add dimesions
Spinning plots
Actual 3-D plots(not that useful)
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

#Summary

Exploratory plots are ‘quick and dirty’
Let you summarize the data(usually graphically) and highly any broad features
Explore basic questions and hypotheses(and perhaps rule them out)
Suggest modeling strategies for the ‘next step’
Plotting

There are three plotting systems in R

#The basic plotting system

* ’Artist’s palette’ model
Start with blank canvas and build up from there
Start with plot function (or similar)
Use annotation functions to add/modify(text, lines, points, axis)

(Pros) Convenient, mirrors how we think of building plots and analysis

(Cons) Can’t go back one plot has started(i.e. to adjust margins)
(Cons) Difficult to ‘translate’ to others once a new plot has been created
(Cons) Plot is just a series of R commands
* The Lattive system

(Pros) Plots are created with a single functin cal (xyplot, bwplot, etc)
(Pros) Most useful for conditioning types of plots: looking at how y changes with x cross levels of z
(Pros) Things like margins/spacing set automatically because entire is specified at once
(Pros) Good for putting many many plots on a screen
(Cons) Sometimes arkward to specify an entire plot in a single function call
(Cons) Annotation in plot is not especially intuitive
(Cons) Use of panel functions and subscripts difficult to wield and requires intense preparation.
(COns) Cannot ‘add’ to the plot once it is created
* The ggplot2 system

Splits the difference between base and lattive in a number of ways
Automatically deals with spacing, text, titles but also allows you to annotate by ‘adding’ to a plot
Superficial similarity to lattice but generaly easier/more intuitive to use
Default mode makes many choices for you (but you can still customize to your heart’s desire)
#Summary

Base: ‘artist’s palette’ model
Lattice: Entire plot specified by one function; conditioning
ggplot2: Mixed elements of Base and Lattice
Base Plotting System

The core plotting and graphics engine in R is encasulated in the following two packages 
graphics: contains plotting functions for the ‘base’ graphing system, plot, boxplot and many others
grDevices: contains all the code implementing the various graphics device, including X11, PDF, PostScript, PNG, etc
The lattice plotting system is implementd using the following packages 
lattice: contains code for producing Trellis graphics, which are independent of the ‘base’ graphics system; including functions like xyplot, bwplot, levelplot
grid: implements a different graphing system independent of the ‘base’ system; the lattice package builds on top of grid; we seldom call functions from grid package directly.
```r
# Simple Base Graphcs: histogram
par(mfrow = c(1,1))
library(datasets)
hist(airquality$Ozone)
# Simple Base Graphics: scatterplot
with(airquality, plot(Wind, Ozone))
# Simple Base Graphics: boxplot
airquality <- transform(airquality, Month = factor(Month))
boxplot(Ozone ~ Month, airquality, xlab = 'month', ylab = 'Ozone(ppb)')
boxplot(Ozone~Month, airquality, xlab="Month", ylab="Ozone (ppb)",col.axis="blue",col.lab="red")
```
`par()` function is used to specify global graphics parameters

`las`: the orientation of the axis labels on the plotting
`bg`: the background color
`mar`: the margin size
`oma`: the outer margin size
`mfrow`: number of plot per row, column(plots are filled row-wise)
`mfcol`: number of plot per row, column(plots are filled column-wise)
```r
with(airquality, plot(Wind, Ozone, main = 'Ozone and Wind in New York City'), type = 'n')
with(subset(airquality, Month == 5), points(Wind, Ozone, col = 'blue'))
with(subset(airquality, Month != 5), points(Wind, Ozone, col = 'red'))
legend('topright', pch = 1, col = c('blue', 'red'), legend = c('May', 'Other months'))
with(airquality, plot(Wind, Ozone, main = 'Ozone and Wind in New York City'), pch = 5)
model <- lm(Ozone ~ Wind, data = airquality)
abline(model, lwd = 2)
par(mfrow = c(1, 3), mar = c(4, 4, 2, 1), oma = c(2, 0, 2, 0)) # bottom left top right
with(airquality, {
        plot(Wind, Ozone, main = 'Ozone and Wind')
        plot(Solar.R, Ozone, main = 'Ozone and Solar.R')
        plot(Temp, Ozone, main = 'Ozone and temperature')
        mtext('Ozone and Weather in New York City', outer = TRUE)
})
```
#Graphics Devies

* A graphics device is something where you can make a plot 
  A window on your computer(screen device)
A PDF file(file device)
A PNG or JPEG file(file device)
A scalable vector graphics (SVG) file(file device)
* Screen devices 
Mac quartz()
Windows windows()
Unix/Linux xll()
* Plot on the screen 
Call a plotting function like plot, xyplot, qplot
The plot appears on the screen device
Annotate plot if necessary
Enjoy
* Plot to a file 
Explicity lauch a graphics device
Call a plotting function to make a plot
Annotate plot if necessary
Explicitly close graphics device with dev.off()(this is very important!!!!)
```r
if(!file.exists('figures')) dir.create('figures')
pdf('./figures/myplot.pdf')
with(pollution, plot(pm25, latitude))
title(main = 'Ozone and Wind')
dev.off()
```
* Graphics FIle Devices: There are two basic types of file devices: vector and bitmap 
**Vector formats:**
pdf: useful for line-type graphics, resizes well, usually portable, not efficient if a plot has many objects or points
svg: XML- based scalable vector graphics; supports animation and interactive, potentially useful for web-based plots
win.metafile: Windows metafile format(only on Windows)
postscript: older format, also resizes well, usually portable, can be used to create encapsulated postscript files; Windows systems ofen don’t have a postscript viewer
**bitmap formats:** 
png: bitmapped format, good for line drawings or images with solid color, uses lossless compression(like the old GIF format), most web browers can read this format natively, good for plotting many many points, does nor resize well
jpeg: good for photographs or natural scenes, uses lossy compression, good for plotting many many many points, does not resize well, can be read by almost any computer and any web brower, not great for line drawings
tiff: Creates bitmap files in the TIFF format; supprots lossless compression
bmp: a native Windows bitmapped format
* Copy plots 
- dev.copy: copy a plot from one device to another 
- dev.copy2pdf: specifically copy a plot to a PDF file.
```r
with(faithful, plot(eruptions, waiting))
title(main = 'Old Faithful Geyser data')
dev.copy(png, file = './figures/greyserplot.png')
dev.off()
```
