[<<< Previous](Part3.md) | [Next >>>](Part5.md)  

## Exercise

Now, let's try to make a leaflet map with:
- two basemaps: OpenStreetMap as default, Esri WorldImagery as optional;
- three data layers: South American countries as polygon layer, Amazon mean annual rainfall as raster layer, site-level AGB as circle markers;
and only show the polygon layer by default.


```diff
leaflet() %>%
# Base groups
  addTiles() %>%
  addProviderTiles() %>%
# Overlay groups

# Layers control
  addLayersControl()%>%
  hideGroup()
```

The answer is in next page, but please think by yourself first before click "Next".

[<<< Previous](Part3.md) | [Next >>>](Part5.md)  
