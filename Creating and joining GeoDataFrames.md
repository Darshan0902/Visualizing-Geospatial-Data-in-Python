#  1) Colormaps

_You'll work with GeoJSON to create polygonal plots, learn about projections and coordinate reference systems, and get practice spatially joining data in this chapter._

<h2> INSTRUCTIONS </h2>

(1.) Plot school_districts with the tab20 colormap. Use 'district' to create a legend and set legend_kwds = lgnd_kwds :

```
# Set legend style
lgnd_kwds = {'title': 'School Districts',
               'loc': 'upper left', 'bbox_to_anchor': (1, 1.03), 'ncol': 1}

# Plot the school districts using the tab20 colormap (qualitative)
school_districts.plot(column = 'district' , cmap = 'tab20', legend = True, legend_kwds = lgnd_kwds)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Nashville School Districts')
plt.show();
```
Output : 

![image](https://user-images.githubusercontent.com/77969007/234368803-edf96c4a-9dbf-47f4-a46c-345b781b9577.png)


(2.) Plot school_districts with the sequential summer colormap. Keep the other arguments as they are.

```
# Set legend style
lgnd_kwds = {'title': 'School Districts',
               'loc': 'upper left', 'bbox_to_anchor': (1, 1.03), 'ncol': 1}

# Plot the school districts using the summer colormap (sequential)
school_districts.plot(column = 'district', cmap = 'summer', legend = True, legend_kwds = lgnd_kwds)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Nashville School Districts')
plt.show();
```
OP :
![image](https://user-images.githubusercontent.com/77969007/234369348-709d1653-3587-4c3e-bb09-c7231c3586fa.png)

(3.) Plot the school districts without setting the column. Use the qualitative Set3 colormap and set legend = True.

```
# Set legend style
lgnd_kwds = {'title': 'School Districts',
               'loc': 'upper left', 'bbox_to_anchor': (1, 1.03), 'ncol': 1}

# Plot the school districts using Set3 colormap without the column argument
school_districts.plot(cmap = 'Set3', legend = True, legend_kwds = lgnd_kwds)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Nashville School Districts')
plt.show();
```
OP :

![image](https://user-images.githubusercontent.com/77969007/234370169-1778de32-04f0-4318-989a-99417042af92.png)

<h3> Map Nashville neighborhoods </h3>

1. Import geopandas and matplotlib.pyplot using the customary aliases.
2. Read in the neighborhoods GeoJSON file to a GeoDataFrame called neighborhoods and print the first few rows with .head(). The path to this file is stored in a variable called neighborhoods_path.
3. Plot neighborhoods coloring the polygons by the name column of the GeoDataFrame and using the Dark2 color map. This time, don't include a legend.
4. Show the plot.


```
import geopandas as gpd
import matplotlib.pyplot as plt
 
# Read in the neighborhoods geojson file
neighborhoods = gpd.read_file(neighborhoods_path)
 
# Print the first few rows of neighborhoods
print(neighborhoods.head())
 
# Plot the neighborhoods, color according to name and use the Dark2 colormap
neighborhoods.plot(column = 'name', cmap = 'Dark2')
 
# Show the plot.
plt.show()
```
OP : 
![image](https://user-images.githubusercontent.com/77969007/234370529-a2e5ba94-8778-41ff-8a6e-9e99e046508c.png)

<h3> Changing coordinate reference systems </h3>

```  
  
  # Print the first row of school districts GeoDataFrame and the crs
print(school_districts.head(1))
print(school_districts.crs)
 
# Convert the crs to epsg:3857
school_districts.geometry = school_districts.geometry.to_crs(epsg = 3857)
                         
# Print the first row of school districts GeoDataFrame and the crs again
print(school_districts.head(1))
print(school_districts.crs)
```

<h3> Construct a GeoDataFrame from a DataFrame </h3>

In this exercise, you will construct a geopandas GeoDataFrame from the Nashville Public Art DataFrame. You will need to import the Point constructor from the shapely.geometry module to create a geometry column in art before you can create a GeoDataFrame from art. This will get you ready to spatially join the art data and the neighborhoods data in order to discover which neighborhood has the most art.

The Nashville Public Art data has been loaded for you as art.

Import Point from the shapely.geometry module.
Print the head() of the art data.
Complete the code for a lambda expression that will create a Point geometry column from the lng and lat columns in art. Remember that longitude comes first!
Build a GeoDataFrame using art and call it art_geo. Set crs equal to neighborhoods.crs and set geometry equal to the column you just created. Print the type() of art_geo to verify it is a GeoDataFrame.

```
import pandas as pd
import geopandas as gpd
from shapely.geometry import Point
import matplotlib.pyplot as plt
 
# Print the first few rows of the art DataFrame
print(art.head())
 
# Create a geometry column from lng & lat
art['geometry'] = art.apply(lambda x: Point(float(x.lng), float(x.lat)), axis=1)
 
# Create a GeoDataFrame from art and verify the type
art_geo = gpd.GeoDataFrame(art, crs = neighborhoods.crs, geometry = art.geometry)
print(type(art_geo))
```

<h3> Spatial join practice </h3>

Write a spatial join to find the art in art_geo that intersects with neighborhoods. Call this art_intersect_neighborhoods and print the .shape property to see how many rows and columns resulted.

```
# Spatially join art_geo and neighborhoods 
art_intersect_neighborhoods = gpd.sjoin(art_geo, neighborhoods, op = 'intersects')
 
# Print the shape property of art_intersect_neighborhoods
print(art_intersect_neighborhoods.shape)
```
Now write a spatial join to find the art in art_geo that is within neighborhoods. Call this art_within_neighborhoods and take a look at the .shape property to see how many rows and columns resulted

```
# Create art_within_neighborhoods by spatially joining art_geo and neighborhoods
art_within_neighborhoods = gpd.sjoin(art_geo, neighborhoods, op = 'within')
 
# Print the shape property of art_within_neighborhoods
print(art_within_neighborhoods.shape)
```

Finally, write a spatial join to find the art locations in art_geo that contain neighborhoods. Call this GeoDataFrame art_containing_neighborhoods and take a look at the .shape property to see how many rows and columns resulted

```
# Spatially join art_geo and neighborhoods and using the contains op
art_containing_neighborhoods = gpd.sjoin(art_geo, neighborhoods, op = 'contains')
 
# Print the shape property of art_containing_neighborhoods
print(art_containing_neighborhoods.shape)
# (0, 13)
```
<h3> Finding the neighborhood with the most public art </h3>
Now that you have created art_geo, a GeoDataFrame, from the art DataFrame, you can join it spatially to the neighborhoods data to see what art is in each neighborhood.

1. Merge the art_geo and neighborhoods GeoDataFrames with a spatial join to create a new GeoDataFrame called neighborhood_art. Find the art that is within neighborhoods.
2. Print the first few rows of the neighborhood art GeoDataFrame.

```
# import packages
import geopandas as gpd
import pandas as pd

# Spatially join neighborhoods with art_geo
neighborhood_art = gpd.sjoin(art_geo, neighborhoods, op = "within")
 
# Print the first few rows
print(neighborhood_art.head())
```

<h3>Aggregating points within polygons</h3> 

```
# Get name and title from neighborhood_art and group by name
neighborhood_art_grouped = neighborhood_art[['name', 'title']].groupby('name')
 
# Aggregate the grouped data and count the artworks within each polygon
print(neighborhood_art_grouped.agg('count').sort_values(by = 'title', ascending = False))
```

<h3> Plotting the Urban Residents neighborhood and art </h3>

Now you know that most art is in the Urban Residents neighborhood. In this exercise, you'll create a plot of art in that neighborhood. First you will subset just the urban_art from neighborhood_art and you'll subset the urban_polygon from neighborhoods. Then you will create a plot of the polygon as ax before adding a plot of the art.

Create a GeoDataFrame called urban_art using the .loc[] accessor to get only the art in the "Urban Residents" neighborhood.
Use .loc[] again to create a GeoDataFrame called urban_polygon from neighborhoods with only the "Urban Residents" polygon.
Plot urban_polygon as ax and set color to lightgreen.
Plot the art in urban_art. Pass three arguments: Set ax = ax to add this plot to the urban_polygon, use type to label the points, and set legend = True.

```
# Create urban_art from neighborhood_art where the neighborhood name is Urban Residents
urban_art = neighborhood_art.loc[neighborhood_art.name == 'Urban Residents']
 
# Get just the Urban Residents neighborhood polygon and save it as urban_polygon
urban_polygon = neighborhoods.loc[neighborhoods.name == "Urban Residents"]
 
# Plot the urban_polygon as ax 
ax = urban_polygon.plot(color = 'lightgreen')
 
# Add a plot of the urban_art and show it
urban_art.plot( ax = ax, column = 'type', legend = True);
plt.show()
```
OUTPUT : 
 
 ![image](https://user-images.githubusercontent.com/77969007/234373814-6d9f50d3-7a0c-449c-8a28-7084bb073aa3.png)





























