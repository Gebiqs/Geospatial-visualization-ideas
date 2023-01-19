# Geospatial-visualization-ideas
Goal of this repository is to describe processes of visualization of geospatial data. Some free softwares offer an ability to visualize geospatial data, but usually it's constrained to some extent. For most softwares it's okay till a country level. But what if your analysis requires a more advances/ in depth analysis? What if your data is more granular? Python with it's libraries of Folium and Geopandas offer unconstrained visualization possibilities.  


## Preparing shp files

In order to visualize geospatial data, first we need a proper template to visualize our data on. We can compare SHP files to a canvas that we will paint on. You can preview sample shapefile data for European and Polish administrative borders in my repository. For geopandas to work, you will need and shp file with .prj, .shx, .cpg files in the same directory. With just few lines of code we can visualize borders of any local muncipality. Here's a simple example of commune borders in Kuyavian-Pomeranian voivodeship with just X lines of code:

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

What we did here is:
1. Opened an shp file. Geopandas reads the files just like regular pandas dataframes. The file contains all the local communes in Poland.
2. In order to show just one we filter the file just to leave communes that are contained in Kuyavian-Pomeranian voivodeship ("Jednostka podziału terytorialnego nr 4" in english is teritorial division entity, which is just a voivodeship)
3. we drop any possible NaN rows
4. Plot
5. save  


More shapefiles you can get on the eurostat website: https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/administrative-units-statistical-units/countries




## Visualizing your data:

Data that you acquire on the internet usually can be easily visualized in geopandas with just one or two joins. In cases when you get data with string names of the communes, it's just a matter of  using .dbf files that come along with shapefile as a dictionary to map names to codes. Once you acquire codes for each commune, perform next join to map the data to the shapefile. We can do this using following code: 

```

df = pd.read_excel(".../wyniki_SOM_cluster_new_table.xlsx", index_col=False)
df['terytorium'] = df['teryt'].apply(lambda x: (str("0"+str(x))))                             #add a zero at the beginning of a string to match the format
merged1 = map1.merge(df,how="right", right_on="terytorium", left_on="JPT_KOD_JE")             #performing a join between data and a shapefile
colors = ["#F8F1B8", "#FDBF71", "#F67B4A", "#DA372A", "#9D0C1B"]                              #creating a custom colormap
cmap = clr.ListedColormap(colors)
merged = gpd.GeoDataFrame(merged1)                                                            #making a geopandas dataframe from a pandas dataframe
fig = plt.figure(dpi=500)
merged.plot(column="SOM", figsize=(20,20),cmap=cmap, legend=True, edgecolor="Black" , linewidth=.4)
plt.xticks(ticks=[])
plt.yticks(ticks=[])
plt.savefig("Kujawsko_pomorskie.svg", format="svg")

```
Output:

![alt text](https://github.com/Gebiqs/Geospatial-visualization-ideas/blob/main/Kujawsko_pomorskie.png)

![alt text](https://github.com/Gebiqs/Geospatial-visualization-ideas/blob/main/Crime_rate.html)
