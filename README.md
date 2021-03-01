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
lat = []
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
  
