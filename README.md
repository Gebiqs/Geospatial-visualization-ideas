# Geospatial-visualization-ideas
Goal of this repository is to describe processes of visualization of geospatial data. Some free softwares offer an ability to visualize geospatial data, but usually it's constrained to some extent. For most softwares it's okay till a country level. But what if your analysis requires a more advances/ in depth analysis? What if your data is more granular? Python with it's libraries of Folium and Geopandas offer unconstrained visualization possibilities.  


## Preparing shp files

In order to visualize geospatial data, first we need a proper template to visualize our data on. We can compare SHP files to a canvas that we will paint on. You can preview sample shapefile data for European and Polish administrative borders in my repository. For geopandas to work, you will need and shp file with .prj, .shx, .cpg files in the same directory. With just few lines of code we can visualize borders of any local muncipality. Here's a simple example of borders of Kuyavian-Pomeranian voivoideship with just X lines of code:

```

import geopandas as gpd

map1 = gpd.read_file("...Gminy\Gminy.shp")
map1['JPT_KOD_JE'] = map1['JPT_KOD_JE'].apply(lambda x: x if x.startswith("04") else None)
map1.dropna(subset=['JPT_KOD_JE'], inplace=True)
map1.plot(facecolor="White", edgecolor="black", figsize=(8,8))
plt.savefig("Kujawsko_pomorskie.svg", format="svg")

```

This generates following output:
![alt text](https://github.com/Gebiqs/Geospatial-visualization-ideas/blob/main/Kujawsko-%20Pomorskie%20-%20borders.png)


More shapefiles you can get on the eurostat website: https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/administrative-units-statistical-units/countries
