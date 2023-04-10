[<<< Previous](Part1.md) | [Next >>>](Part3.md)  

## Static Maps

[Map objects](#map-objects)

[Aesthetics](#aesthetics)

[Color settings](#color-settings)

[Layouts](#layouts)

Before learning more advanced mapping skills, it's good to keep in mind that the basic `plot()` function is always the fastest and easiest way to create a static map, if simplicity and speed are your priorities. In fact, most of R libraries in the field of geospatial analysis (e.g. `sf`, `sp`, `raster`, `terra`, and etc.) support direct plotting with `plot()`, and its argument can be any vector or raster object.

However, other libraries might excel if higher standards of aesthetic and layout are demanded. This tutorial focuses on `tmap` for its powerful and flexible low-level control and sensible default settings. In addition, the syntax of `tmap` follows a similar logic with `ggplot2`, which provides convenience for the users who are familiar with `ggplot2`.

First, let's load the essential libraries.

```diff
library(sf)          
library(terra)       
library(tidyverse)   
library(tmap)        
```
Then read the data used in this workshop. (More about read-in geographic data, please refer to Section 3&4 in the preceding workshop [Introduction to Geospatial R](https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR))

```diff
# vector data: demography of South America
sa_regions = read_sf("data/vectors/SAstats.shp")
# raster data: mean annual rainfall in Amazon forest
amz_mar = rast('data/rasters/amz_MAR.tif')
# tabular data: MAR (mean annual rainfall), MAT (mean annnual temperature) and AGB (aboveground biomass) in Amazon inventory sites
site_df = read.csv('data/tables/AMZ_site.csv')
```

#### Map objects

The core mapping function in `tmap` is `tm_shape()`, which defines input data. However, this function can't stand solely and must be followed by one or more layer elements.

```diff
# Add fill layer to feature object
tm_shape(sa_regions) +
  tm_fill() 
# Add border layer to feature object
tm_shape(sa_regions) +
  tm_borders() 
# Add fill and border layers to feature object
tm_shape(sa_regions) +
  tm_fill() +
  tm_borders() 
# previous operation can be combined into one command
tm_shape(sa_regions) + 
  tm_polygons()
```
  
We can map objects with multiple layers.
```
map_sa = tm_shape(sa_regions) + tm_polygons() 
map_sa + tm_shape(amz_mar) + tm_raster(alpha = 0.7)
```

#### Aesthetics

The default values and other aesthetics can be overridden by passing a value to the corresponding argument.

```diff
# change filling color
map1 = tm_shape(sa_regions) + tm_fill(col = "red")
# change filling color and transparency
map2 = tm_shape(sa_regions) + tm_fill(col = "red", alpha = 0.3)
# change boundary color
map3 = tm_shape(sa_regions) + tm_borders(col = "blue")
# change boundary line width
map4 = tm_shape(sa_regions) + tm_borders(lwd = 3)
# change boundary line type
map5 = tm_shape(sa_regions) + tm_borders(lty = 2)
# change all together
map6 = tm_shape(sa_regions) + tm_fill(col = "red", alpha = 0.3) +
  tm_borders(col = "blue", lwd = 3, lty = 2)
  
tmap_arrange(map1, map2, map3, map4, map5, map6)
```

For the data with multiple attributes, we are able to change display field.

```diff
# display country names
tm_shape(sa_regions) + tm_fill(col = "COUNTRY")
# display country land area
tm_shape(sa_regions) + tm_fill(col = "AREA")
```

And the legend title can be modified to desired text.
```
legend_title = expression("Area (km"^2*")")
tm_shape(sa_regions) + tm_fill(col = "AREA", title = legend_title) + tm_borders()
```

#### Color settings

Here are simples example showing how to mannually change color settings. *For more suggestions on color scheme choosing, please refer to [Resources](Part5.md).*

```diff
# manually set the breaks
breaks = c(0, 1, 2, 5, 10) * 1000000
tm_shape(sa_regions) + tm_polygons(col = "AREA", breaks = breaks)
# sets the number of bins 
tm_shape(sa_regions) + tm_polygons(col = "AREA", n = 10)
# change color scheme
tm_shape(sa_regions) + tm_polygons(col = "AREA", palette = "RdPu")
```

#### Layouts

A professional map usually includes others the objects in addition to the main spatial object to be mapped, such as the title, the scale bar, margins and so on. Here is an example (not perfect but decent).

```diff
tm_shape(sa_regions) + tm_fill(col = "COUNTRY") + tm_borders() +
  tm_compass(type = "8star", position = c("left", "top")) +
  tm_scale_bar(breaks = c(0, 1000, 2000), text.size = 1) +
  tm_layout(title = "South America",bg.color = "lightblue")
```

[<<< Previous](Part1.md) | [Next >>>](Part3.md) 
