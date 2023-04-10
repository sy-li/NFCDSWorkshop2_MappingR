[<<< Previous](Part4.md) | [Home](../README.md)

## Resources

To further explore or troubleshoot issues in map-making with R, consider taking advantage of these resources:

1. [tmap: get started!](https://r-tmap.github.io/tmap/articles/tmap-getstarted.html) - a tutorial for `tmap` package.

2. [Leaflet for R](https://rstudio.github.io/leaflet/) - a tutorial for `leaflet` package.

3. Helpful notes on **colour schemes and templates**:
  
    - [R colors cheat-sheet](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf) - hundreds of color names can be used in R;
  
    - [R color palettes cheat-sheet](https://www.nceas.ucsb.edu/sites/default/files/2020-04/colorPaletteCheatsheet.pdf) - guidance on color representation with hex code and many other palettes;
  
    - [Color schemes for all people](https://personal.sron.nl/~pault/) - colors should be carefully chosen to be distinct for all people, including colour-blind readers. 

4. [ND Udemy](https://nd.udemy.com/) - a website offering thousands of professional development and training videos, freely accessible by all Notre Dame students.

5. [Stack Exchange](https://gis.stackexchange.com/) - excellent source for GIS-related troubleshooting.


### Exercise Answer Key

```diff
leaflet() %>%
# Base groups
  addTiles(group = "OSM (default)") %>%
  addProviderTiles('Esri.WorldImagery',group='Imagery') %>%
# Overlay groups
  addPolygons(data = sa_regions,color = "grey", weight = 1, smoothFactor = 0.5,
              opacity = 1.0, fillOpacity = 0.3,
              fillColor = ~colorQuantile("YlOrRd", AREA)(AREA),
              group = "Total Area")%>%
  addRasterImage(amz_mar, colors = pal, opacity = 0.8,group = "Mean Annual Rainfall") %>%
  addCircleMarkers(data = site_df,~lon, ~lat, 
                   popup = ~as.character(agb), label = ~as.character(agb),
                   group="Abovegroud Biomass")%>%
# Layers control
  addLayersControl(
    baseGroups = c("OSM (default)", "Imagery"),
    overlayGroups = c("Abovegroud Biomass", "Total Area","Mean Annual Rainfall"),
    options = layersControlOptions(collapsed = FALSE)
  ) %>%
# Hide the last two layers
  hideGroup(c("Total Area","Mean Annual Rainfall"))
```

[<<< Previous](Part4.md) | [Home](../README.md)
