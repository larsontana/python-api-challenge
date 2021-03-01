# python-api-challenge
# Weather Segment

# Set up libraries and dependencies. 
import pandas as pd
import numpy as np
import requests
import time
import datetime
from scipy.stats import linregress

# Import Weather API key. 
from config import weather_api_key 

# Add citipy to determine city based on latitude and longitude.
from citipy import citipy
# _Looking up for city names with geo-coordinates has always been a big problem when it comes to dealing with social data._

# Output file.
output_data_file = "cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
# _Typical latitude and longitude values_

# --- Generate Cities List --- 

# Create lists to store the data. 
latsandlongs = [] 
cities = []
# Generate random latitudes and longitudes and assign them to a city. 
lats = np.random.uniform(low=-90.000,high=90.000,size=1500
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
# _The uniform means a uniform distribution_
latsandlngs = zip(lats, lngs)
# _zip() essentially combines two lists into a cohesive data set. Now we are looking at a combined data of random lats and longs_
# Identify random cities that are closes to the random lat and long combinations. 
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
# If the city is unique, add to our cities list. 
if city not in cities:
  cities.append(city)
len(cities)

---- Perform API Calls ----

import pprint
pp = pprint.PrettyPrinter(indent=3)

c_id= []
name = []
country = []
long = []
latt = []
cloudiness= []
date= []
humidity= []
max_temp = []
wind_speed = []
weather_json = {}

try:
    url = "http://api.openweathermap.org/data/2.5/weather?"
    
     for city in cities:
        query_url = url + "&q=" + city + "&APPID=" + weather_api_key 
        weather_response = requests.get(query_url)
        weather_json = weather_response.json()
        
        c_id.append(str(weather_json['id']))
        name.append(str(weather_json['name']))
        country.append(str(weather_json['sys']['country']))
        long.append(float(round(weather_json['coord']['lon'],2)))
        latt.append(float(round(weather_json['coord']['lat'],2)))
        cloudiness.append(float(weather_json['clouds']['all']))
        date.append(str(datetime.datetime.fromtimestamp(weather_json['dt']).strftime("%A, %d. %B %Y %I:%M%p")))
        humidity.append(float(weather_json['main']['humidity']))
        max_temp.append((1.8*(weather_json['main']['temp_max'] - 273) + 32))
        wind_speed.append(float(weather_json['wind']['speed']))
   
   except KeyError:
    pass                         
  
  ---- Convert Raw Data to DataFrame ---- 
  
  weather_list_df = pd.DataFrame({'City ID':c_id,'City':name,'Country':country,'Lng':long,'Lat':latt, 'Cloudiness':cloudiness, 'Date': date, 'Humidity': humidity, 'Max Temp':max_temp, 'Wind Speed':wind_speed})
  
weather_list_df.to_csv(output_data_file,index = False)
weather_list_df.head(10)

---- Plotting the Data ----

# Latitude vs Temperature Plot
plt.scatter(weather_list_df['Lat'], weather_list_df['Max Temp'],marker="v", facecolors="blue", edgecolors="black)
#  Set lower and upper limits
plt.ylim(0,120)
plt.xlim(-60,180)
# Create title and labels. 
plt.title("City Latitude vs. Max Temperature")
plt.ylabel("Maximum Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)
plt.savefig('../output_data/Lat_MT.png')

# Latitude vs. Humidity Plot
plt.scatter(weather_list_df['Lat'], weather_list_df['Humidity'], marker="v", facecolors="blue", edgecolors="black")
# Set the upper and lower limits.
plt.ylim(0,300)
plt.xlim(-60,180)

# Create a title, x label, and y label for our chart
plt.title("City Latitude vs. Humidity")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.savefig('../output_data/Lat_Hum.png')

# Latitude vs. Cloudiness Plot
plt.scatter(weather_list_df['Lat'], weather_list_df['Cloudiness'], marker="v", facecolors="blue", edgecolors="black")
# Set the upper and lower limits.
plt.ylim(0,100)
plt.xlim(-60,180)
# Create a title, x label, and y label for our chart
plt.title("City Latitude vs. Cloudiness")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.savefig('../output_data/Lat_Cl.png')

# Latitude vs. Wind Speed Plot
plt.scatter(weather_list_df['Lat'], weather_list_df['Wind Speed'], marker="v", facecolors="blue", edgecolors="black")

# Set the upper and lower limits of our y axis
plt.ylim(0,50)

# Set the upper and lower limits of our x axis
# plt.xlim(-60,180)

# Create a title, x label, and y label for our chart
plt.title("City Latitude vs. Wind Speed (11/14/2019)")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)
plt.savefig('../output_data/Lat_W.png')
