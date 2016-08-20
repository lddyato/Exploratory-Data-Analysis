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
