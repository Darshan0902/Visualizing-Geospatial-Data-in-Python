# Creating a choropleth building permit density in Nashville

In this chapter, you will learn about a special map called a choropleth. Then you will learn and practice building choropleths using two different packages: geopandas and folium.



<h3>Finding counts from a spatial join</h3>

Create a Point geometry column in permits from lat and lng.
Create permits_geo, a GeoDataFrame, using permits, the crs from council_districts, and the geometry from permits.
Use a spatial join to find permits issued within each council district. Print the first 2 rows.
Create permit_counts to show the count of permits within each district, using groupby() and .size(). Print permit_counts.


```
from shapely.geometry import Point
 
# Create a shapely Point from lat and lng
permits['geometry'] = permits.apply(lambda x: Point((x.lng , x.lat)), axis = 1)
 
# Build a GeoDataFrame: permits_geo
permits_geo = gpd.GeoDataFrame(permits, crs = council_districts.crs, geometry = permits.geometry)
 
# Spatial join of permits_geo and council_districts
permits_by_district = gpd.sjoin(permits_geo, council_districts, op = 'within')
print(permits_by_district.head(2))
 
# Create permit_counts
permit_counts = permits_by_district.groupby(['district']).size()
print(permit_counts)
```

<h3> Council district areas and permit counts </h3>

Get the area of each council district and store it in a new column, area, in the council_districts GeoDataFrame. Print the first two rows.
```
# Create an area column in council_districts
council_districts['area'] = council_districts.geometry.area
print(council_districts.head(2))
```

Next convert permit_counts to a DataFrame with the .to_frame() method, and print the first two rows.

```
# Convert permit_counts to a DataFrame
permits_df = permit_counts.to_frame()
print(permits_df.head(2))
```
Reset the index using the inplace = True argument and set the columns attribute to a list with the names district and bldg_permits. Print the first two rows again to see what permits_df looks like now.

```
# Reset index and column names
permits_df.reset_index(inplace=True)
permits_df.columns = ['district', 'bldg_permits']
print(permits_df.head(2))
```
Create a new GeoDataFrame called districts_and_permits by merging council_districts and permits_df on the district column. Take a look at the first two rows.


```
# Merge council_districts and permits_df: 
districts_and_permits = pd.merge(council_districts, permits_df, on = 'district')
print(districts_and_permits.head(2))
```

<h3> Calculating a normalized metric </h3>

Print the type() of districts_and_permits.
Add one more column to districts_and_permits. Use a lambda expression and the pandas apply() method to divide the number of building permits issued for projects in each council district by the area of that district to get a normalized value for the permits issued.
Print the first five rows of districts_and_permits.

```
# Print the type of districts_and_permits
print(type(districts_and_permits))
 
# Create permit_density column in districts_and_permits
districts_and_permits['permit_density'] = districts_and_permits.apply(lambda row: row.bldg_permits / row.area, axis = 1)
 
# Print the head of districts_and_permits
print(districts_and_permits.head())
```


<h3>Geopandas choropleths</h3>

Import matplotlib.pyplot, pandas, and geopandas with the customary aliases.

```
# Import packages
import matplotlib.pyplot as plt
import pandas as pd
import geopandas as gpd
```

Plot districts_and_permits, using the permit_density column you just created and the default colormap. Be sure to call plt.show().

```
# Simple plot of building permit_density
districts_and_permits.plot(column = 'permit_density', legend = True);
plt.show();

```

OUTPUT :

![image](https://user-images.githubusercontent.com/77969007/234379715-1774cda3-69a4-417f-9e77-2543f162e87f.png)

Plot districts_and_permits, using the permit_density column you just created and the default colormap. Be sure to call plt.show().

```
# Polished choropleth of building permit_density
districts_and_permits.plot(column = 'permit_density', cmap = 'BuGn', edgecolor = 'black', legend = True)
plt.xlabel('longitude')
plt.ylabel('latitude')
plt.xticks(rotation = 'vertical')
plt.title('2017 Building Project Density by Council District')
plt.show();
```
OP :

![image](https://user-images.githubusercontent.com/77969007/234379923-dfe0944e-68c1-484b-9797-86fad0a8f56b.png)


<h3> Area in km squared, geometry in decimal degrees </h3>

Change the coordinate reference system for the council_districts to EPSG 3857, and print the crs and first two rows again.
Create a column called area. Divide the area of each polygon by sqm_to_sqkm to get the area in kilometers squared.
Change the coordinate reference system for the council_districts back to EPSG 4326. Print the crs and first two rows.

```

# Change council_districts crs to epsg 3857
council_districts = council_districts.to_crs(epsg = 3857)
print(council_districts.crs)
print(council_districts.head())
 
# Create area in square km
sqm_to_sqkm = 10**6
council_districts['area'] = council_districts.geometry.area / sqm_to_sqkm
 
# Change council_districts crs back to epsg 4326
council_districts = council_districts.to_crs(epsg = 4326)
print(council_districts.crs)
print(council_districts.head())


```

<h3> Spatially joining and getting counts </h3>

Create permits_geo from permits, the council_districts.crs, and the geometry in permits.
Spatially join permits_geo and the council_districts to get building permits within each council district. Call this permits_by_district.
Count permits in each district, permit_counts, by chaining groupby() and size() methods.
Create counts_df from permit_counts. Reset the index, and name the columns district and bldg_permits.

```
# Create permits_geo
permits_geo = gpd.GeoDataFrame(permits, crs = council_districts.crs, geometry = permits.geometry)
 
# Spatially join permits_geo and council_districts
permits_by_district = gpd.sjoin(permits_geo, council_districts, op = 'within')
print(permits_by_district.head(2))
 
# Count permits in each district
permit_counts = permits_by_district.groupby('district').size()
 
# Convert permit_counts to a df with 2 columns: district and bldg_permits
counts_df = permit_counts.to_frame()
counts_df.reset_index(inplace=True)
counts_df.columns = ['district', 'bldg_permits']
print(counts_df.head(2))
```
<h3> Building a polished Geopandas choropleth </h3>

Merge permits_by_district and counts_df on district to create districts_and_permits.
Using apply() and a lambda expression, calculate a new column in districts_and_permits called permit_density. Divide counts by areas.
Plot a choropleth of the districts_and_permits, using permit_density with the OrRd colormap, and black outlines.
Add axis labels (longitude and latitude) and the title provided. Show your plot.

```
# Merge permits_by_district and counts_df
districts_and_permits = pd.merge(permits_by_district, counts_df, on = 'district')
 
# Create permit_density column
districts_and_permits['permit_density'] = districts_and_permits.apply(lambda row: row.bldg_permits / row.area, axis = 1)
print(districts_and_permits.head(2))
 
# Create choropleth plot
districts_and_permits.plot(column = 'permit_density', cmap = 'OrRd', edgecolor = 'black', legend = True)
 
# Add axis labels and title
plt.xlabel('longitude')
plt.ylabel('latitude')
plt.title('2017 Building Project Density by Council District')
plt.show()
```
O/P :

![image](https://user-images.githubusercontent.com/77969007/234380505-0844a905-40d3-49a3-9da0-6f8450308d83.png)

<h3> Folium choropleth </h3> 

Create a map object m using the nashville point for location and a zoom_start of 10.

```
# Center point for Nashville
nashville = [36.1636,-86.7823]
 
# Create map
m = folium.Map(location=nashville, zoom_start=10)
```
Use districts_and_permits: geometry for polygons, district & permit_density to color. Use Reds and 0.5 opacity for the fill; Set line_opacity to 1.0.
Set key_on to feature.properties.district. Use the title provided.

```
folium.Choropleth(
   
    geo_data=districts_and_permits,
    name='geometry',
    data=districts_and_permits,
    columns=['district', 'permit_density'],
    key_on='feature.properties.district',
    fill_color='Reds',
    fill_opacity=0.5,
    line_opacity=1.0,
    legend_name='2017 Permitted Building Projects per km squared'
)
 
# Create LayerControl and add it to the map            
folium.LayerControl().add_to(m)
 
# Display the map
display(m)
```

Next create and add a folium LayerControl(). Display the map.

```
# Create LayerControl and add it to the map            
folium.LayerControl().add_to(m)
 
# Display the map
display(m)
```

<h3> Folium choropleth with markers and popups
 </h3>

Find the centroid for each council district and store it in a new column, center in the districts_and_permits GeoDataFrame.
Iterate through districts_and_permits and add a marker at each district's center. Remember to reverse the coordinate pair.
Create popups within your for loop to display the district number and the count of permits issued.
Add the markers to your map with .add_to() and display it.

```
# Create center column for the centroid of each district
districts_and_permits['center'] = districts_and_permits.geometry.centroid
 
# Build markers and popups
for row in districts_and_permits.iterrows():
    row_values = row[1] 
    center_point = row_values['center']
    location = [center_point.y, center_point.x]
    popup = ('Council District: ' + str(row_values['district']) +
             ';  ' + 'permits issued: ' + str(row_values['bldg_permits']))
    marker = folium.Marker(location = location, popup = popup)
    marker.add_to(m)
     
# Display the map
display(m)
```











