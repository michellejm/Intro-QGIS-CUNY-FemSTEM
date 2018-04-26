## CUNY FemSTEM Hack Description

### Premise

For this hack, we are going to estimate how many people live within a 1/2 mile radius of subway stations in New York City. We will use our findings to identify "transportation deserts", as well as "transportation hot spots". We will represent our findings with a heatmap that can be printed or embedded into a paper or website. 

### Data 

For this hack, we will need three different datasets:
- A shapefile of New York City
- Population data (from the Census) that can be used with our shapefile
- A shapefile of the subway stations

#### Notes on data
When working with census data from the US Census, there are a few levels of granularity that you can look at: State, County, Census Tracts, and Block Group. Some areas have intermediate levels that may be tabulated or reported by the census, such as Neighborhood Tabulation Areas, or Metropolitan Regions. Usually, these are the result of combining smaller areas to make intermediate areas in regions of high population density. 

Census Tracts are areas created by the census for analytic purposes, and Block Groups are, just as described, groups of city blocks, smaller than census tracts. When trying to make population estimates of any kind, it is best practice to use the smallest area available for the data you are interested in (to protect privacy, some data is only available at the Tract or County level). Because population information is very detailed for New York City, and available at the Block Group level, we are able to use that. 

#### Finding Data
Census data is published by the US Government in their annual American Community Survey Report. You can find historic census data by doing a data query at NHGIS. However, because we are really only interested in New York City, we are able to use the NYC Open Data Portal. In recent years, many municipalities have established these data portals to allow students, researchers, and enthusiasts easy acces to pre-cleaned data. 