
# Graphics Devies
* A graphics device is something where you can make a plot 
 + A window on your computer(screen device)
 + A PDF file(file device)
 + A PNG or JPEG file(file device)
 + A scalable vector graphics (SVG) file(file device)
* Screen devices 
 + Mac quartz()
 + Windows windows()
 + Unix/Linux xll()
* Plot on the screen 
 + Call a plotting function like `plot, xyplot, qplot`
 + The plot appears on the screen device
 + Annotate plot if necessary
 + Enjoy
* Plot to a file 
 + Explicity lauch a graphics device
 + Call a plotting function to make a plot
 + Annotate plot if necessary
 + Explicitly close graphics device with dev.off()(this is very important!!!!)

```r
if(!file.exists('figures')) dir.create('figures')
pdf('./figures/myplot.pdf')
with(pollution, plot(pm25, latitude))
title(main = 'Ozone and Wind')
dev.off()
```
* Graphics FIle Devices: There are two basic types of file devices: `vector` and `bitmap`
 + Vector formats: 
    +    `pdf`: useful for line-type graphics, resizes well, usually portable, not efficient if a plot has many objects or points
    +    `svg`: XML- based scalable vector graphics; supports animation and interactive, potentially useful for web-based plots
    +    `win.metafile`: Windows metafile format(only on Windows)
    +    `postscript`: older format, also resizes well, usually portable, can be used to create encapsulated postscript files; Windows systems ofen don’t have a postscript viewer
 + bitmap formats: 
    +    `png`: bitmapped format, good for line drawings or images with solid color, uses lossless compression(like the old GIF format), most web browers can read this format natively, good for plotting many many points, does nor resize well
    +    `jpeg`: good for photographs or natural scenes, uses lossy compression, good for plotting many many many points, does not resize well, can be read by almost any computer and any web brower, not great for line drawings
    +    `tiff`: Creates bitmap files in the TIFF format; supprots lossless compression
    +    `bmp`: a native Windows bitmapped format

* Copy plots 
 - `dev.copy`: copy a plot from one device to another 
 - `dev.copy2pdf`: specifically copy a plot to a PDF file.
```r
with(faithful, plot(eruptions, waiting))
title(main = 'Old Faithful Geyser data')
dev.copy(png, file = './figures/greyserplot.png')
dev.off()
```
