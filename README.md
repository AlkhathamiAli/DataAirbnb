# insights-on-Airbnb-Florida-
python analysis using calendar and listing dataset of Florida 
## For the following analysis I have download the data from https://data.insideairbnb.com/united-kingdom/england/greater-Florida/2024-09-23/data/calendar.csv.gz
``` diff
import pandas as pd
calendar=pd.read_csv("calendar.csv")
```
## I want to know the number of available and unavailable rooms
``` diff
calendar.available.value_counts()
```
<img width="732" alt="image" src="https://github.com/user-attachments/assets/5c6a7e45-eedd-444d-9c09-c73e72aff99f" />


f(false) means not available, t(true) means available 

## 2 purpose: calculates the percentage of available (t) and unavailable (f) dates.
Explanation value_counts(normalize=True) give proportion. Multiplaying by 100 converts the proportion into percentage


``` diff
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/003bbbb7-ec79-45c6-a316-ec8d2a140b13" />


## 3 Let's Count the busiest day!
Hint We will be counting the most unavailable days (given by f)
``` diff
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/22a4ced9-d30e-47e9-9ad0-cb89c9393d13" />


## 4 Plot a bar graph to show availability percentage
``` diff
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="732" alt="image" src="https://github.com/user-attachments/assets/a32035ec-76bf-4cac-8b5f-2d0f7eee51f5" />


## 5 Plot the busiest day
``` diff
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/7f1b9771-9c42-4770-8fdf-ef229834917b" />


## 📄 Download listings dataset of Florida from
https://data.insideairbnb.com/united-kingdom/england/greater-Florida/2024-09-23/visualisations/listings.csv

``` diff
import pandas as pd
listings = pd.read_csv('listings.csv')
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/ef7bae65-c6f3-49b7-93a2-3c4c315ff6ee" />



``` diff
listings.columns
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/77ce8d74-c4cf-4aa8-a934-0702a66d6888" />



## ✅ Room type and price is given seperately
Let`s Combine and visualize them
``` diff
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='cyan')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/dfe7403c-c5b9-4d4d-9a94-3663a35dc6fc" />


## ✅ Which are the top 10 neighborhoods with the most listings?
``` diff
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/9f71f993-ce75-4d2e-bb92-fdf9685d3623" />


## ✅ What is the distribution of the room types (Private room, Entire home/apt) in the listings data?
```diff
room_type_counts = listings['room_type'].value_counts()
labels = room_type_counts.index
sizes = room_type_counts.values

plt.figure(figsize=(8, 6))
plt.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=30)
plt.axis('equal')
plt.title('Distribution of Room Types')
plt.show()
```
<img width="609" alt="image" src="https://github.com/user-attachments/assets/f453f772-d8f9-43a2-8915-fce12a63f0c3" />

## ✅ How does the price vary across different neighborhoods?
```diff
neighborhood_prices = listings.groupby('neighbourhood')['price'].mean().sort_values().head(15)

plt.figure(figsize=(12, 8))
neighborhood_prices.plot(kind='bar', color='skyblue')
plt.xlabel('Neighborhoods')
plt.ylabel('Average Price')
plt.title('Average Price by Neighborhood')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/79cbd0e6-c7a7-486c-828c-5d093e45f987" />


## ✅ Geographical Distribution of Listings (Price Colored)
``` diff
import matplotlib.pyplot as plt
import seaborn as sns
```
```diff
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="737" alt="image" src="https://github.com/user-attachments/assets/d95541d4-721a-471f-a428-e5451e92e6ce" />


## Let us see the listings on a real map
Hotter Areas (Red/Yellow): High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Florida where people tend to stay more often. Colder Areas (Green/Blue):

Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available. Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:

Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Florida.
``` diff
import folium
from folium.plugins import HeatMap
import pandas as pd


Florida_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around Florida
m = folium.Map(location=[27.6648, 81.5158], zoom_start=10)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in Florida_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('Florida_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
<img width="732" alt="image" src="https://github.com/user-attachments/assets/525bdec5-bd1b-40ce-ace7-ac13d77d03eb" />

