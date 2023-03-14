[<<< Previous](../README.md) | [Next >>>](Part2.md)  


## Brief Overview of Cartography

[What is Cartography](#what-is-cartography)

[Map Types](#map-types)

[Principles of Map Design](#principles-of-map-design)

[Workshop Preparation](#workshop-preparation)


### What is Cartography

**Cartography** is the art and science of graphically representing a geographical area, usually on a flat surface such as a map or chart. It may involve the superimposition of political, cultural, or other nongeographical divisions onto the representation of a geographical area (_[Encyclopaedia Britannica](https://www.britannica.com/science/cartography)_).

In short, cartography is the study of making and using maps.


### Map Types

One common categorization in the field of cartography is **general maps** vs **thematic maps**.

General maps contain a variety of features, oriented toward general audience.

<p align="middle">
  <img src="https://github.com/sy-li/NFCDSWorkshop2_MappingR/blob/main/Sections/images/world_map_4651_oct22.jpg">
</p>
<p align="middle">
  <em>Example of a general map: Map of the World (credit: UN).
</em>
</p>


Thematic maps involve specific geographic themes, targeted to specific audiences.

<p align="middle">
  <img src="https://github.com/sy-li/NFCDSWorkshop2_MappingR/blob/main/Sections/images/unemployment_remake.png">
</p>
<p align="middle">
  <em>Example of a thematic map: County-level Unemployment Map. (credit: Cary Anderson, Penn State University. Data Sources: Esri, US Census Bureau.)
</em>
</p>

### Principles of Map Design

The central goal of cartography is to communicate spatial information effectively. It combines science, aesthetics and technique. A good map should be both aesthetically pleasing and practically useful for intended purposes.

When designing a map, these aspects are recommended to consider ([Wikipedia](https://www.wikiwand.com/en/Cartography#Map_design)):

- **Map projections**: The foundation of the map is the plane on which it rests (whether paper or screen), but projections are required to flatten the surface of the earth. All projections distort this surface, but the cartographer can be strategic about how and where distortion occurs.

- **Generalization**: All maps must be drawn at a smaller scale than reality, requiring that the information included on a map be a very small sample of the wealth of information about a place. Generalization is the process of adjusting the level of detail in geographic information to be appropriate for the scale and purpose of a map, through procedures such as selection, simplification, and classification.

- **Symbology**: Any map visually represents the location and properties of geographic phenomena using map symbols, graphical depictions composed of several visual variables, such as size, shape, color, and pattern.

- **Composition**: As all of the symbols are brought together, their interactions have major effects on map reading, such as grouping and Visual hierarchy.

- **Typography** or **Labeling**: Text serves a number of purposes on the map, especially aiding the recognition of features, but labels must be designed and positioned well to be effective.

- **Layout**: The map image must be placed on the page (whether paper, web, or other media), along with related elements, such as the title, legend, additional maps, text, images, and so on. Each of these elements have their own design considerations, as does their integration, which largely follows the principles of Graphic design.

- **Map type-specific design**: Different kinds of maps, especially thematic maps, have their own design needs and best practices.


### Workshop Preparation

Before diving into code, let's first install essential R packages. Please copy and paste the code block below into your RStudio console and run it.

```diff
packages.to.install = c(
# handle spatial vector data
'sf'        
# handle spatial raster data
'terra'    
# a standardized way to manage dataset
'tidyverse'  
# for static and interactive maps
'tmap'  
# for interactive maps
'leaflet'
)

install.packages(packages.to.install,repos = 'https://cloud.r-project.org')
```

Please download the [data](https://github.com/sy-li/NFCDSWorkshop2_MappingR/blob/main/data.zip) and unzip it into the same folder with your R script.



[<<< Previous](../README.md) | [Next >>>](Part2.md)  
