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
