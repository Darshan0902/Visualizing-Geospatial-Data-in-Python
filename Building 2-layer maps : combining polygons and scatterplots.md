# Styling a scatterplot
_Import the pyplot module from matplotlib with the usual alias._

(1.)
<h3> Create a scatterplot of father and son heights with a square marker (encoded as s) that is 'darkred'. Show your plot. </h3>

```
# Import matplotlib.pyplot
import matplotlib.pyplot as plt

# Scatterplot 1 - father heights vs. son heights with darkred square markers
plt.scatter(father_son.fheight, father_son.sheight, c = 'darkred', marker = 's')

# Show your plot
plt.show()
```

(2.) 
<h3> Edit the code to change the markers for your scatterplot of father heights versus son heights. Make the markers 'yellow' with a 'darkblue' edgecolor. </h3>

```
# Import matplotlib.pyplot
import matplotlib.pyplot as plt

# Scatterplot 2 - yellow markers with darkblue borders
plt.scatter(father_son.fheight, father_son.sheight,c = 'yellow', edgecolor = 'darkblue')

# Show the plot
plt.show()
```
(3.) 
<h3> Add gridlines and axes labels ('father height (inches)' and 'son height (inches)') to your scatterplot.
Give the plot a title of 'Son Height as a Function of Father Height'.</h3>

```


# Import matplotlib.pyplot
import matplotlib.pyplot as plt

# Scatterplot 3
plt.scatter(father_son.fheight, father_son.sheight,  c = 'yellow', edgecolor = 'darkblue')
plt.grid()
plt.xlabel("father height (inches)")
plt.ylabel("son height (inches)")
plt.title('Son Height as a Function of Father Height')

# Show your plot
plt.show()

```

<h3> # Extracting longitude and latitude

A DataFrame named df has been pre-loaded for you. Complete the code to extract longitude and latitude to new, separate columns. </h3>


```

# print the first few rows of df 
print(df.head())

# extract latitude to a new column: lat
df['lat'] = [loc[0] for loc in df.Location]

# extract longitude to a new column: lng
df['lng'] = [loc[1] for loc in df.Location]

# print the first few rows of df again
print(df.head())

```
## Plotting chicken locations
-The path to the chicken dataset is in the variable chickens_path. Use the read_csv function of pandas to load it into a DataFrame called chickens.

-Use the .head() function to look at the first few rows.

-Next add the x and y arguments to plt.scatter() to plot the locations of the Nashville chickens. Use the default marker and color options.

-Show the plot using plt.show().

```
# Import pandas and matplotlib.pyplot using their customary aliases
import pandas as pd
import matplotlib.pyplot as plt

# Load the dataset
chickens = pd.read_csv(chickens_path)

# Look at the first few rows of the chickens DataFrame
print(chickens.head())

# Plot the locations of all Nashville chicken permits
plt.scatter(x = chickens.lng, y = chickens.lat)

# Show the plot
plt.show()
```
OUTPUT :

![image](https://user-images.githubusercontent.com/77969007/234362823-b90f7a11-6e56-484e-8e26-2b551a8be85d.png)



#Creating a GeoDataFrame & examining the geometry

Import geopandas with its common alias gpd. Read in the service district shapefile using geopandas and look at the first 5 rows using the head() method. Print the geometry field in the first row (rowname is '0') to see the data contained in that field. You will pass service_district.loc[0, 'geometry'] to the print() function to do this.

```
# Import geopandas
import geopandas as gpd 

# Read in the services district shapefile and look at the first few rows.
service_district = gpd.read_file(shapefile_path)
print(service_district.head())

# Print the contents of the service districts geometry in the first row
print(service_district.loc[0, 'geometry'])

```

# Plotting shapefile polygons
<n> First plot the service districts without additional arguments by calling .plot() on the GeoDataFrame.</n>
<n> Take a look at it with plt.show(). This has been done for you. </n>
Now use the .plot() method again, but this time add column='name' to color the shapes according to their names and legend=True to see those names. Remember to show the plot

```
# Import pandas and matplotlib.pyplot using their customary aliases
import pandas as pd
import matplotlib.pyplot as plt

# Load the dataset
chickens = pd.read_csv(chickens_path)

# Look at the first few rows of the chickens DataFrame
print(chickens.head())

# Plot the locations of all Nashville chicken permits
plt.scatter(x = chickens.lng, y = chickens.lat)

# Show the plot
plt.show()
```
OUTPUT :

![image](https://user-images.githubusercontent.com/77969007/234364491-110d1041-d03b-49c9-a3f6-db9810694818.png)


<h3> Creating a GeoDataFrame & examining the geometry </h3>

Import geopandas with its common alias gpd. Read in the service district shapefile using geopandas and look at the first 5 rows using the head() method. Print the geometry field in the first row (rowname is '0') to see the data contained in that field. You will pass service_district.loc[0, 'geometry'] to the print() function to do this.

```
# Import geopandas
import geopandas as gpd 

# Read in the services district shapefile and look at the first few rows.
service_district = gpd.read_file(shapefile_path)
print(service_district.head())

# Print the contents of the service districts geometry in the first row
print(service_district.loc[0, 'geometry'])
```

<h3> Plotting shapefile polygons </h3>

- <br> First plot the service districts without additional arguments by calling .plot() on the GeoDataFrame.<br>
- <br> Take a look at it with plt.show(). This has been done for you.<br>
- <br>Now use the .plot() method again, but this time add column='name' to color the shapes according to their names and legend=True to see those names. Remember to show the plot<br>
```
# Import packages
import geopandas as gpd
import matplotlib.pyplot as plt

# Plot the Service Districts without any additional arguments
service_district.plot()
plt.show()

# Plot the Service Districts, color them according to name, and show a legend
service_district.plot(column = 'name', legend = True)
plt.show()
```

OUTPUT :
![image](https://user-images.githubusercontent.com/77969007/234365293-600b00f2-1a7e-4547-8f7e-4f878c75f9e0.png)

# Plotting points over polygons - part 1

- Plot the shapefile for the service districts, using the name column to color the polygons.
- Add chicken locations, using the lat and lng columns from the chickens DataFrame, and make them black.
- Show your plot.

```
# Plot the service district shapefile
service_district.plot(column = 'name')

# Add the chicken locations
plt.scatter(x=chickens.lng, y=chickens.lat, c = 'black')

# Show the plot
plt.show()

```
OUTPUT : 

![image](https://user-images.githubusercontent.com/77969007/234365479-633b5540-1d86-461b-9c15-b020e9d4a162.png)
# Plotting points over polygons - part 2

1. Start by plotting the GeoDataFrame with the service districts. Use the name column for your legend color.
2. Next plot latitude and longitude from the chicken data to create a scatterplot. Specify 'black' for the marker color and give them a 'white' outline.
3. Give the plot a title: 'Nashville Chicken Permits' and label the x-axis as 'longitude' and the y-axis as 'latitude'.
4. Add grid lines and show your plot.

```
# Plot the service district shapefile
service_district.plot(column= 'name' , legend=True)

# Add the chicken locations
plt.scatter(x=chickens.lng, y=chickens.lat, c='black', edgecolor = 'white')


# Add labels and title
plt.title('Nashville Chicken Permits')
plt.xlabel('longitude')
plt.ylabel('latitude')

# Add grid lines and show the plot
plt.grid()
plt.show()
```

OUTPUT :
![image](https://user-images.githubusercontent.com/77969007/234365749-15834675-2836-4046-ab26-253165ed9b44.png)



