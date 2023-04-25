# GeoSeries and folium

_First you will learn to get information about the geometries in your data with three different GeoSeries attributes and methods. Then you will learn to create a street map layer using folium._

<h3>Find the area of the Urban Residents neighborhood</h3>

How big is the Urban Residents neighborhood?

Print the urban polygon and notice the units of each longitude/latitude pair.
Create urban_poly_3857 by calling to_crs() on urban_polygon and print the head again. Notice the units of each longitude/latitude pair have changed.
Print the area of urban_poly_3857. Remember to divide by 10**6 to get kilometers squared.

```
# Print the head of the urban polygon 
print(urban_polygon.head())
 
# Create a copy of the urban_polygon using EPSG:3857 and print the head
urban_poly_3857 = urban_polygon.to_crs(epsg = 3857)
print(urban_poly_3857.head())
 
# Print the area of urban_poly_3857 in kilometers squared
area = urban_poly_3857.geometry.area / 10**6
print('The area of the Urban Residents neighborhood is ', area[0], ' km squared')
 

```

<h3>  The center of the Urban Residents neighborhood </h3>

Create downtown_center, from urban_poly_3857 using the GeoSeries centroid attribute.
Print the datatype of downtown_center.
Plot urban_poly_3857 as ax using lightgreen for the color.
Plot the downtown_center, setting ax = ax and color = black. The x-axis ticks are rotated for you. We've included the code to show the plot.

```
# Create downtown_center from urban_poly_3857
downtown_center = urban_poly_3857.geometry.centroid
 
# Print the type of downtown_center 
print(type(downtown_center))
 
# Plot the urban_poly_3857 as ax and add the center point
ax = urban_poly_3857.plot(color = 'lightgreen')
downtown_center.plot(ax = ax, color = 'black')
plt.xticks(rotation = 45)
 
# Show the plot
plt.show()
```

OP : 
![image](https://user-images.githubusercontent.com/77969007/234375441-d3b9de68-b002-4a88-9aee-2a869685c9b5.png)

<h3>Prepare to calculate distances</h3>

Create a GeoDataFrame called art_dist_meters, using the art DataFrame and the geometry column from art. Set crs = {'init': 'epsg:4326'} since the geometry is in decimal degrees. Print the first two rows.
Now explicitly set the coordinate reference system to EPSG:3857 for art_dist_meters by using to_crs(). Print the first two rows again.
Add a column called center to art_dist_meters, setting it equal to center_point for every row .
```
# Import packages
from shapely.geometry import Point
import geopandas as gpd
import pandas as pd
 
# Create art_dist_meters using art and the geometry from art
art_dist_meters = gpd.GeoDataFrame(art, geometry = art.geometry, crs = {'init': 'epsg:4326'})
print(art_dist_meters.head(2))
 
# Set the crs of art_dist_meters to use EPSG:3857
art_dist_meters.geometry = art_dist_meters.geometry.to_crs(epsg = 3857)
print(art_dist_meters.head(2))
 
# Add a column to art_meters, center
art_dist_meters['center'] = center_point
```
<h3> Art distances from neighborhood center </h3> 

Import the package to help with pretty printing.
Create a dictionary, art_distances by iterating through art_dist_meters, using title as the key and the distance() from center as the value. Pass center as the other argument to GeoSeries.distance().
Pretty print art_distances using the pprint method of pprint.
```
# Import package for pretty printing
import pprint
 
# Build a dictionary of titles and distances for Urban Residents art
art_distances = {}
for row in art_dist_meters.iterrows():
    vals = row[1]
    key = vals['title']
    ctr = vals['center']
    art_distances[key] = vals['geometry'].distance(other=ctr)
 
# Pretty print the art_distances
pprint.pprint(art_distances)

```



<h3> Create a folium location from the urban centroid </h3>

Print the head of urban_polygon.
Store the first occurrence of center as urban_center and print urban_center. This has been done for you.
Create an array from urban_center that reverses the order of longitude and latitude. Call this urban_location.
Print urban_location. This has been done for you.

```
# Print the head of the urban_polygon
print(urban_polygon.head())
 
# Create urban_center from the urban_polygon center
urban_center = urban_polygon.center[0]
 
# Print urban_center
print(urban_center)
 
# Create array for folium called urban_location
urban_location = [urban_center.y, urban_center.x]
 
# Print urban_location
print(urban_location)
```
<h3> Create a folium map of downtown Nashville </h3>

Construct a folium map called downtown_map. Use the urban_location array you created in the previous exercise and set the initial zoom level to 15.
Display your folium map object with the provided display function.

```
# Construct a folium map with urban_location
downtown_map = folium.Map(location = urban_location, zoom_start = 15)
 
# Display the map
display(downtown_map)

```
<h3> Folium street map of the downtown neighborhood </h3>

Create an array called folium_loc from urban_polygon.center
Create a folium map called downtown_map. Set the location argument equal to folium_loc and initialize the map with a zoom_start of 15.
Pass the geometry from the urban_polygon GeoDataFrame to the folium.GeoJson() method. Then call add_to() on that.

```
# Create array for called folium_loc from the urban_polygon center point
point = urban_polygon.centroid[0]
folium_loc = [point.y, point.x]
 
# Construct a map from folium_loc: downtown_map
downtown_map = folium.Map(location = folium_loc, zoom_start = 15)
 
# Draw our neighborhood: Urban Residents
folium.GeoJson(urban_polygon.geometry).add_to(downtown_map)
 
# Display the map
display(downtown_map)
```
<h3> Adding markers for the public art </h3>

First take a look at the tuple returned by iterrows() by printing the first and second values.
Assign the second value of the iterrows() tuple to row_values. Create a location formatted for folium, use it to build a marker, and add it to the downtown_map.
Display the map.
```
# Iterate through the urban_art and print each part of tuple returned
for row in urban_art.iterrows():
  print('first part: ', row[0])
  print('second part: ', row[1])
 
# Create a location and marker with each iteration for the downtown_map
for row in urban_art.iterrows():
    row_values = row[1] 
    location = [row_values['lat'], row_values['lng']]
    marker = folium.Marker(location = location)
    marker.add_to(downtown_map)
 
# Display the map
display(downtown_map)

```

<h3> Troubleshooting data issues </h3>

Print and inspect the values in the title column of the urban_art DataFrame.
Print and inspect the values in the desc column of the urban_art DataFrame.
Use the fillna() method to replace the NaN values in the desc column with empty strings, and use .str.replace to replace the apostrophes (') with back-ticks (')
Print the descriptions again to verify your work.

```
# Print the urban_art titles
print(urban_art.title)
 
#Print the urban_art descriptions
print(urban_art.desc)
 
# Replace Nan and ' values in description
urban_art.desc.fillna('', inplace = True)
urban_art.desc = urban_art.desc.str.replace("'", "`")
 
#Print the urban_art descriptions again
print(urban_art.desc)

```

<h3> A map of downtown art </h3>

For each row in urban_art, build a popup message that includes the title for the corresponding artwork.
Complete the code to replace all instances of single quotes (') with backticks () in the popup messages.
Display the finished map.

```
# Construct downtown map
downtown_map = folium.Map(location = nashville, zoom_start = 15)
folium.GeoJson(urban_polygon).add_to(downtown_map)

# Create popups inside the loop you built to create the markers
for row in urban_art.iterrows():
    row_values = row[1] 
    location = [row_values['lat'], row_values['lng']]
    popup = (str(row_values['title'])).replace("'", "`")
    marker = folium.Marker(location = location, popup = popup)
    marker.add_to(downtown_map)

# Display the map.
display(downtown_map)
```






























