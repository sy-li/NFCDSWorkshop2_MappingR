[<<< Previous](Part2.md) | [Next >>>](Part4.md)  

## Interactive Maps

While `tmap` is a handy toolkit for static mapping, it can also create interactive maps using the same code - just switching to "view mode":

``` diff
# switch into interactive mode
tmap_mode("view")
map_sa

# switch back to static mode
tmap_mode("plot")
```

However, `leaflet` is perhaps the most mature and widely used interactive mapping package in R. [Leaflet](https://leafletjs.com/) is one of the most popular open-source JavaScript libraries for interactive maps, and the R package `leaflet` provides a relatively low-level interface to the Leaflet JavaScript library.

`leaflet` features several aspects: in addition to interactive panning and zooming, it is able to display any combination of maps tiles, markers, popups and highlighted features. It can also render spatial objects read from a variety of R libraries, including the most common ones, such as `sp`, `sf`, `raster` and `terra`. The rest of this tutorial we will be working with `leaflet`.

[Basemaps](#basemaps)

[Markers](#markers)

[Display vectors](#display-vectors)

[Display rasters](#display-rasters)

[Interactive layer display](#interactive-layer-display)

#### Basemaps

To pull up a leaflet map, we just use the simple command `leaflet()`, which brings up an empty space because we haven't given it a basemap yet. 
```
leaflet() 
```

To add default tiles, we pipe our leaflet map (%>%) to the `addTiles()` command:
```
leaflet() %>% 
  addTiles() 
```

We can change to third-party tiles:
```diff
# use ESRI's WorldStreetMap
leaflet() %>% 
  addProviderTiles('Esri.WorldStreetMap') 
```

Users are allowed to set the view window by piping this map to a `setView()` command:
```diff
# take a look at Notre Dame campus!
leaflet() %>% 
  addProviderTiles('Esri.WorldStreetMap') %>%
  setView(lat=41.701807, lng= -86.238913,zoom=14)
```

In fact, we can set multiple basemaps. The code below shows how to add another basetile (aerial imagery) and toggle between it and street maps:
```diff
leaflet() %>% 
  addProviderTiles('Esri.WorldStreetMap', group='Streets') %>%
  addProviderTiles('Esri.WorldImagery',group='Imagery') %>%
  addLayersControl(baseGroups=c('Streets','Imagery'),
                   options = layersControlOptions(collapsed = F, autoZIndex = T)) %>% 
  setView(lat=41.701807, lng= -86.238913,zoom=14)
# Now we can toggle between streets and imagery
```

#### Markers

Markers are usually used to call out points on the map.

- add icon
```
leaflet(data = site_df) %>% addTiles() %>%
  addMarkers(~lon, ~lat, popup = ~as.character(agb), label = ~as.character(agb))
```

- add circle
```
leaflet(site_df) %>% 
  addTiles() %>% 
  addCircleMarkers(~lon, ~lat, popup = ~as.character(agb), label = ~as.character(agb))
```

- add customized markers by `awesomeIcons()`
```diff
# define three classes and corresponding colors
getColor = function(site_df) {
  sapply(site_df$agb, function(agb) {
    if(agb <= 80) {
      "red"
    } else if(agb <= 160) {
      "orange"
    } else {
      "green"
    } })
}

# create customized markers
icons = awesomeIcons(
  icon = 'ios-close',
  iconColor = 'black',
  library = 'ion',
  markerColor = getColor(site_df)
)

# display in map
leaflet(site_df) %>% addTiles() %>%
  addAwesomeMarkers(~lon, ~lat, icon=icons, label=~as.character(agb))
```

#### Display vectors

For interactive mapping, we usually need more than just displaying spatial data, but also highlighting targeted features.

Here's an example of highlighting the moused-over polygon:
```
leaflet(sa_regions) %>%addTiles()%>%
  addPolygons(color = "grey", weight = 1, smoothFactor = 0.5,
              opacity = 1.0, fillOpacity = 0.5,
              fillColor = ~colorQuantile("YlOrRd", POP20)(POP20),
              highlightOptions = highlightOptions(color = "white", weight = 2,
                                                  bringToFront = TRUE))
```

Here's another example showing how to emphasize particular points by adding circles. Note that **circle**s are similar to **circle marker**s; however, circles have their radii specified in meters, while circle markers are specified in pixels. As a result, circles are scaled with the map as the user zooms in and out, while circle markers remain a constant size on the screen regardless of zoom level.
```
leaflet(site_df) %>% addTiles()%>%
  addCircles(lng = ~lon, lat = ~lat,weight = 1,
             radius = ~agb * 1000, popup = ~agb)%>%
  addPolygons(data = sa_regions,color = "grey", weight = 1, smoothFactor = 0.5,
              opacity = 1.0, fillOpacity = 0.3,
              fillColor = ~colorQuantile("YlOrRd", POP20)(POP20))
```     

Or, highlight a rectangle area using `addRectangles()`:
```
leaflet() %>% addTiles() %>%
  addRectangles(
    lng1=-86.248913, lat1=41.711807,
    lng2=-86.228913, lat2=41.691807,
    fillColor = "transparent"
  )
```
                                             
#### Display rasters

Raster data can be turned into images and added to Leaflet maps using the `addRasterImage()` function. 

_Note: be aware of large rasters! `addRasterImage` will error when the PNG is beyond the size indicated by the `maxBytes` argument (defaults to 4MB). If your raster is too large, reducing the raster size (by `resample` or `aggregate`) is recommended instead of increasing `maxBytes`._

```diff
# define color palette
pal = colorNumeric(c("#0C2C84","#41B6C4","#FFFFCC"), values(amz_mar),na.color = "transparent")

leaflet() %>% addTiles() %>%
  addRasterImage(amz_mar, colors = pal, opacity = 0.8) %>%
  addLegend(pal = pal, values = values(amz_mar),title = "Mean Annual Rainfall")
```

#### Interactive layer display

Finally, let's combine what we've learned together and add something fun: show and hide map layers!
```diff
leaflet() %>%
# Base groups
  addTiles(group = "OSM (default)") %>%
  addProviderTiles('Esri.WorldStreetMap', group='Streets') %>%
  addProviderTiles('Esri.WorldImagery',group='Imagery') %>%
# Overlay groups
  addCircles(data = site_df,lng = ~lon, lat = ~lat,weight = 1,
             opacity = 1.0, fillOpacity = 0.3,
             radius = ~agb * 1000, popup = ~agb,group="Abovegroud Biomass")%>%
  addPolygons(data = sa_regions,color = "grey", weight = 1, smoothFactor = 0.5,
              opacity = 1.0, fillOpacity = 0.3,
              fillColor = ~colorQuantile("YlOrRd", AREA)(AREA),
              group = "Total Area")%>%
  addRasterImage(amz_mar, colors = pal, opacity = 0.8,group = "Mean Annual Rainfall") %>%
# Layers control
  addLayersControl(
    baseGroups = c("OSM (default)", "Streets", "Imagery"),
    overlayGroups = c("Abovegroud Biomass", "Total Area","Mean Annual Rainfall"),
    options = layersControlOptions(collapsed = FALSE)
  )
```


[<<< Previous](Part2.md) | [Next >>>](Part4.md)  
