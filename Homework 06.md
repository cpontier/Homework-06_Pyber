
Your objective is to build a series of scatter plots to showcase the following relationships:

* Temperature (F) vs. Latitude
* Humidity (%) vs. Latitude
* Cloudiness (%) vs. Latitude
* Wind Speed (mph) vs. Latitude

Your final notebook must:

* Randomly select **at least** 500 unique (non-repeat) cities based on latitude and longitude.
* Perform a weather check on each of the cities using a series of successive API calls. 
* Include a print log of each city as it's being processed with the city number, city name, and requested URL.
* Save both a CSV of all data retrieved and png images for each scatter plot.

As final considerations:

* You must use the Matplotlib and Seaborn libraries.
* You must include a written description of three observable trends based on the data. 
* You must use proper labeling of your plots, including aspects: Plot Titles (with date of analysis) & Axes Labels.
* You must include an exported markdown version of your Notebook called  `README.md` in your GitHub repository.  
* See [Example Solution](WeatherPy_Example.pdf) for a reference on expected format. 

from citipy import citipy
city = citipy.nearest_city(22.99, 120.21)
city
citipy.City instance at 0x1069b6518>

city.city_name     # Tainan, my home town
'tainan'

city.country_code
'tw'                  # And the country is surely Taiwan


```python
# Dependencies
import pandas as pd
import random
import numpy as np
import requests
import json
from citipy import citipy
import matplotlib.pyplot as plt
import seaborn as sns

# Google API Key
wkey = '03f86711a729435d41bf2a6a7729afb3'
```


```python
#create an empty list to store cities
cities = []
citiesdict = []

#loop to fill cities until the list contains 500 entries
while len(cities) < 500:
    lat = random.uniform(-90, 90)
    lng = random.uniform(-180, 180)
    
    city = citipy.nearest_city(lat, lng).city_name
    
    if city not in cities:
        citiesdict.append({'city_name':city,'lat':lat,'lng':lng})
        cities.append(city)

```

example of possible methods

cities = []

while len(cities) <= 5:
    latitude = random.randint(-90.00,90.00)
    longitude = random.randint(-180.00, 180.00)
    city = cp.nearest_city(latitude, longitude)
    if city not in cities:
        cities.append(city.city_name + "," + city.country_code)
    else:
        continue

print(cities)


for x in cities:
    temp = w.get_temperature()
    wind = w.get_wind()
    humid = w.get_humidity()
    cloud = w.get_clouds()
    print(x)
    (?)
    print("temp: " + str(temp['temp']))
    print("wind: " + str(wind['speed']))
    print("humidity: " + str(humid))
    print("cloudiness: " + str(cloud))
    print()


```python
citiesdf = pd.DataFrame(citiesdict)
citiesdf.columns = (['City Name', 'Latitude', 'Longitude'])
citiesdf['Temperature'] = ''
citiesdf['Humidity'] = ''
citiesdf['Cloudiness'] = ''
citiesdf['Windspeed'] = ''
citiesdf.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Name</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Windspeed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bluff</td>
      <td>-82.689777</td>
      <td>155.154586</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>punta arenas</td>
      <td>-56.164619</td>
      <td>-96.538650</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>ushuaia</td>
      <td>-64.767769</td>
      <td>-42.678560</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>new norfolk</td>
      <td>-71.847960</td>
      <td>134.165846</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>puerto ayora</td>
      <td>-20.408405</td>
      <td>-100.397595</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



Description:
To get access to weather API you need an API key whatever account you chose from Free to Enterprise.

Activation of an API key for Free and Startup plans takes 10 minutes. For other tariff plans it is 10 to 60 minutes.

We keep right to not to process API requests without API key.

API call:
http://api.openweathermap.org/data/2.5/forecast?id=524901&APPID={APIKEY}
Parameters:
APPID {APIKEY} is your unique API key 
Example of API call:
api.openweathermap.org/data/2.5/forecast?id=524901&APPID=1111111111 

api.openweathermap.org/data/2.5/weather?q=London


```python

#Testing Indvidual Line Calls
base_url = "http://api.openweathermap.org/data/2.5/weather?q="
city = citiesdf.iloc[0,0]
complete_url = f'{base_url}{city}{end_url}'
data = requests.get(complete_url)
data = data.json()
data

```




    {'base': 'stations',
     'clouds': {'all': 20},
     'cod': 200,
     'coord': {'lat': -23.58, 'lon': 149.07},
     'dt': 1519888334,
     'id': 2175403,
     'main': {'grnd_level': 998.38,
      'humidity': 60,
      'pressure': 998.38,
      'sea_level': 1020.76,
      'temp': 305.014,
      'temp_max': 305.014,
      'temp_min': 305.014},
     'name': 'Bluff',
     'sys': {'country': 'AU',
      'message': 0.003,
      'sunrise': 1519847941,
      'sunset': 1519893165},
     'weather': [{'description': 'few clouds',
       'icon': '02d',
       'id': 801,
       'main': 'Clouds'}],
     'wind': {'deg': 44.0014, 'speed': 4.11}}




```python
#using the airport exercise as a basis -- loop through with rows from the newly created data frame
for index, row in citiesdf.iterrows():
    base_url = "http://api.openweathermap.org/data/2.5/weather?q="
    city = row['City Name']
    end_url = f"&APPID={wkey}"
    complete_url = f'{base_url}{city}{end_url}'
    data = requests.get(complete_url)
    data = data.json()
    
    print(f"Calling information for {city} index {index} from {complete_url}")
    try:
        citiesdf.at[index, "Temperature"]=data['main']['temp']*(9/5)-459.67
        citiesdf.at[index, "Humidity"]=data['main']['humidity']
        citiesdf.at[index, "Cloudiness"]=data['clouds']['all']
        citiesdf.at[index, "Windspeed"]=data['wind']['speed']
    except (KeyError, IndexError) as e:
        #remove row for city that doesn't have coordinates
        citiesdf.drop(index,axis=0,inplace=True)

citiesdf.head()
```

    Calling information for bluff index 0 from http://api.openweathermap.org/data/2.5/weather?q=bluff&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for punta arenas index 1 from http://api.openweathermap.org/data/2.5/weather?q=punta arenas&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ushuaia index 2 from http://api.openweathermap.org/data/2.5/weather?q=ushuaia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for new norfolk index 3 from http://api.openweathermap.org/data/2.5/weather?q=new norfolk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for puerto ayora index 4 from http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port hedland index 5 from http://api.openweathermap.org/data/2.5/weather?q=port hedland&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for torbay index 7 from http://api.openweathermap.org/data/2.5/weather?q=torbay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hithadhoo index 8 from http://api.openweathermap.org/data/2.5/weather?q=hithadhoo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for severo-kurilsk index 9 from http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for xian index 10 from http://api.openweathermap.org/data/2.5/weather?q=xian&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for airai index 11 from http://api.openweathermap.org/data/2.5/weather?q=airai&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vallenar index 12 from http://api.openweathermap.org/data/2.5/weather?q=vallenar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port alfred index 13 from http://api.openweathermap.org/data/2.5/weather?q=port alfred&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hilo index 14 from http://api.openweathermap.org/data/2.5/weather?q=hilo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mataura index 16 from http://api.openweathermap.org/data/2.5/weather?q=mataura&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vestmannaeyjar index 17 from http://api.openweathermap.org/data/2.5/weather?q=vestmannaeyjar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for butaritari index 18 from http://api.openweathermap.org/data/2.5/weather?q=butaritari&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sarany index 19 from http://api.openweathermap.org/data/2.5/weather?q=sarany&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for busselton index 20 from http://api.openweathermap.org/data/2.5/weather?q=busselton&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for quatre cocos index 21 from http://api.openweathermap.org/data/2.5/weather?q=quatre cocos&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ponta do sol index 22 from http://api.openweathermap.org/data/2.5/weather?q=ponta do sol&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for anupgarh index 23 from http://api.openweathermap.org/data/2.5/weather?q=anupgarh&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for codrington index 24 from http://api.openweathermap.org/data/2.5/weather?q=codrington&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sao miguel do araguaia index 25 from http://api.openweathermap.org/data/2.5/weather?q=sao miguel do araguaia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for chuy index 26 from http://api.openweathermap.org/data/2.5/weather?q=chuy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint-philippe index 27 from http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for carnarvon index 29 from http://api.openweathermap.org/data/2.5/weather?q=carnarvon&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mar del plata index 30 from http://api.openweathermap.org/data/2.5/weather?q=mar del plata&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sitka index 31 from http://api.openweathermap.org/data/2.5/weather?q=sitka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for constitucion index 32 from http://api.openweathermap.org/data/2.5/weather?q=constitucion&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bonavista index 33 from http://api.openweathermap.org/data/2.5/weather?q=bonavista&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cayenne index 34 from http://api.openweathermap.org/data/2.5/weather?q=cayenne&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for rikitea index 35 from http://api.openweathermap.org/data/2.5/weather?q=rikitea&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yulara index 36 from http://api.openweathermap.org/data/2.5/weather?q=yulara&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kapaa index 37 from http://api.openweathermap.org/data/2.5/weather?q=kapaa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for barrow index 38 from http://api.openweathermap.org/data/2.5/weather?q=barrow&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for conceicao do araguaia index 39 from http://api.openweathermap.org/data/2.5/weather?q=conceicao do araguaia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lata index 40 from http://api.openweathermap.org/data/2.5/weather?q=lata&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for anderson index 41 from http://api.openweathermap.org/data/2.5/weather?q=anderson&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for khash index 42 from http://api.openweathermap.org/data/2.5/weather?q=khash&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for beboto index 43 from http://api.openweathermap.org/data/2.5/weather?q=beboto&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hobart index 44 from http://api.openweathermap.org/data/2.5/weather?q=hobart&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bredasdorp index 45 from http://api.openweathermap.org/data/2.5/weather?q=bredasdorp&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yellowknife index 46 from http://api.openweathermap.org/data/2.5/weather?q=yellowknife&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for anito index 47 from http://api.openweathermap.org/data/2.5/weather?q=anito&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sao filipe index 48 from http://api.openweathermap.org/data/2.5/weather?q=sao filipe&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jamestown index 49 from http://api.openweathermap.org/data/2.5/weather?q=jamestown&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bethel index 50 from http://api.openweathermap.org/data/2.5/weather?q=bethel&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for westport index 51 from http://api.openweathermap.org/data/2.5/weather?q=westport&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for abu dhabi index 52 from http://api.openweathermap.org/data/2.5/weather?q=abu dhabi&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for leningradskiy index 53 from http://api.openweathermap.org/data/2.5/weather?q=leningradskiy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for matamoros index 55 from http://api.openweathermap.org/data/2.5/weather?q=matamoros&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ribeira grande index 56 from http://api.openweathermap.org/data/2.5/weather?q=ribeira grande&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ripky index 57 from http://api.openweathermap.org/data/2.5/weather?q=ripky&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nuevo ideal index 59 from http://api.openweathermap.org/data/2.5/weather?q=nuevo ideal&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tiksi index 61 from http://api.openweathermap.org/data/2.5/weather?q=tiksi&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for atuona index 63 from http://api.openweathermap.org/data/2.5/weather?q=atuona&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ekhabi index 64 from http://api.openweathermap.org/data/2.5/weather?q=ekhabi&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for dubna index 65 from http://api.openweathermap.org/data/2.5/weather?q=dubna&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint anthony index 66 from http://api.openweathermap.org/data/2.5/weather?q=saint anthony&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for scarborough index 67 from http://api.openweathermap.org/data/2.5/weather?q=scarborough&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ierissos index 68 from http://api.openweathermap.org/data/2.5/weather?q=ierissos&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for altay index 69 from http://api.openweathermap.org/data/2.5/weather?q=altay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tuktoyaktuk index 70 from http://api.openweathermap.org/data/2.5/weather?q=tuktoyaktuk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint-augustin index 71 from http://api.openweathermap.org/data/2.5/weather?q=saint-augustin&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ansalta index 72 from http://api.openweathermap.org/data/2.5/weather?q=ansalta&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for carutapera index 73 from http://api.openweathermap.org/data/2.5/weather?q=carutapera&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for georgetown index 74 from http://api.openweathermap.org/data/2.5/weather?q=georgetown&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for shelbyville index 75 from http://api.openweathermap.org/data/2.5/weather?q=shelbyville&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bandarbeyla index 77 from http://api.openweathermap.org/data/2.5/weather?q=bandarbeyla&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for luderitz index 79 from http://api.openweathermap.org/data/2.5/weather?q=luderitz&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tutoia index 80 from http://api.openweathermap.org/data/2.5/weather?q=tutoia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port elizabeth index 81 from http://api.openweathermap.org/data/2.5/weather?q=port elizabeth&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saldanha index 82 from http://api.openweathermap.org/data/2.5/weather?q=saldanha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for paamiut index 83 from http://api.openweathermap.org/data/2.5/weather?q=paamiut&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sokoni index 84 from http://api.openweathermap.org/data/2.5/weather?q=sokoni&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for dikson index 85 from http://api.openweathermap.org/data/2.5/weather?q=dikson&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for edea index 86 from http://api.openweathermap.org/data/2.5/weather?q=edea&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vao index 87 from http://api.openweathermap.org/data/2.5/weather?q=vao&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hermanus index 89 from http://api.openweathermap.org/data/2.5/weather?q=hermanus&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for salto index 90 from http://api.openweathermap.org/data/2.5/weather?q=salto&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lavrentiya index 91 from http://api.openweathermap.org/data/2.5/weather?q=lavrentiya&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ostersund index 92 from http://api.openweathermap.org/data/2.5/weather?q=ostersund&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ingham index 93 from http://api.openweathermap.org/data/2.5/weather?q=ingham&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sainte-maxime index 94 from http://api.openweathermap.org/data/2.5/weather?q=sainte-maxime&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint-pierre index 96 from http://api.openweathermap.org/data/2.5/weather?q=saint-pierre&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for esmeralda index 97 from http://api.openweathermap.org/data/2.5/weather?q=esmeralda&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kuusamo index 98 from http://api.openweathermap.org/data/2.5/weather?q=kuusamo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nagorsk index 99 from http://api.openweathermap.org/data/2.5/weather?q=nagorsk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ahipara index 100 from http://api.openweathermap.org/data/2.5/weather?q=ahipara&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for longyearbyen index 101 from http://api.openweathermap.org/data/2.5/weather?q=longyearbyen&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ugoofaaru index 102 from http://api.openweathermap.org/data/2.5/weather?q=ugoofaaru&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saskylakh index 103 from http://api.openweathermap.org/data/2.5/weather?q=saskylakh&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kaeo index 104 from http://api.openweathermap.org/data/2.5/weather?q=kaeo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sorland index 105 from http://api.openweathermap.org/data/2.5/weather?q=sorland&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for avarua index 106 from http://api.openweathermap.org/data/2.5/weather?q=avarua&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vostok index 107 from http://api.openweathermap.org/data/2.5/weather?q=vostok&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for verkhnevilyuysk index 108 from http://api.openweathermap.org/data/2.5/weather?q=verkhnevilyuysk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vila franca do campo index 109 from http://api.openweathermap.org/data/2.5/weather?q=vila franca do campo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port blair index 110 from http://api.openweathermap.org/data/2.5/weather?q=port blair&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jalu index 111 from http://api.openweathermap.org/data/2.5/weather?q=jalu&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for wanaka index 112 from http://api.openweathermap.org/data/2.5/weather?q=wanaka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for baoding index 113 from http://api.openweathermap.org/data/2.5/weather?q=baoding&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cape town index 114 from http://api.openweathermap.org/data/2.5/weather?q=cape town&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for faanui index 115 from http://api.openweathermap.org/data/2.5/weather?q=faanui&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ndago index 116 from http://api.openweathermap.org/data/2.5/weather?q=ndago&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mayo index 117 from http://api.openweathermap.org/data/2.5/weather?q=mayo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for muros index 118 from http://api.openweathermap.org/data/2.5/weather?q=muros&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint george index 119 from http://api.openweathermap.org/data/2.5/weather?q=saint george&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mengyin index 120 from http://api.openweathermap.org/data/2.5/weather?q=mengyin&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bunia index 121 from http://api.openweathermap.org/data/2.5/weather?q=bunia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kopavogur index 122 from http://api.openweathermap.org/data/2.5/weather?q=kopavogur&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sekoma index 124 from http://api.openweathermap.org/data/2.5/weather?q=sekoma&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hovd index 125 from http://api.openweathermap.org/data/2.5/weather?q=hovd&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nantucket index 126 from http://api.openweathermap.org/data/2.5/weather?q=nantucket&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vaini index 128 from http://api.openweathermap.org/data/2.5/weather?q=vaini&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kamenka index 129 from http://api.openweathermap.org/data/2.5/weather?q=kamenka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for loanda index 130 from http://api.openweathermap.org/data/2.5/weather?q=loanda&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for qaanaaq index 132 from http://api.openweathermap.org/data/2.5/weather?q=qaanaaq&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for villamontes index 133 from http://api.openweathermap.org/data/2.5/weather?q=villamontes&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for birao index 134 from http://api.openweathermap.org/data/2.5/weather?q=birao&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for novo aripuana index 135 from http://api.openweathermap.org/data/2.5/weather?q=novo aripuana&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for changying index 136 from http://api.openweathermap.org/data/2.5/weather?q=changying&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nanortalik index 137 from http://api.openweathermap.org/data/2.5/weather?q=nanortalik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for caravelas index 138 from http://api.openweathermap.org/data/2.5/weather?q=caravelas&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tandalti index 139 from http://api.openweathermap.org/data/2.5/weather?q=tandalti&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for khatanga index 140 from http://api.openweathermap.org/data/2.5/weather?q=khatanga&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for muncar index 141 from http://api.openweathermap.org/data/2.5/weather?q=muncar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pisco index 142 from http://api.openweathermap.org/data/2.5/weather?q=pisco&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for brae index 143 from http://api.openweathermap.org/data/2.5/weather?q=brae&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for narsaq index 144 from http://api.openweathermap.org/data/2.5/weather?q=narsaq&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for norman wells index 145 from http://api.openweathermap.org/data/2.5/weather?q=norman wells&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for doka index 146 from http://api.openweathermap.org/data/2.5/weather?q=doka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hasaki index 147 from http://api.openweathermap.org/data/2.5/weather?q=hasaki&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bambous virieux index 148 from http://api.openweathermap.org/data/2.5/weather?q=bambous virieux&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for benghazi index 149 from http://api.openweathermap.org/data/2.5/weather?q=benghazi&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for north bend index 150 from http://api.openweathermap.org/data/2.5/weather?q=north bend&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for daru index 151 from http://api.openweathermap.org/data/2.5/weather?q=daru&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for avera index 152 from http://api.openweathermap.org/data/2.5/weather?q=avera&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for puerto escondido index 153 from http://api.openweathermap.org/data/2.5/weather?q=puerto escondido&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for san patricio index 154 from http://api.openweathermap.org/data/2.5/weather?q=san patricio&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for naze index 155 from http://api.openweathermap.org/data/2.5/weather?q=naze&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for albany index 156 from http://api.openweathermap.org/data/2.5/weather?q=albany&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for takoradi index 158 from http://api.openweathermap.org/data/2.5/weather?q=takoradi&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for timra index 160 from http://api.openweathermap.org/data/2.5/weather?q=timra&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bousse index 161 from http://api.openweathermap.org/data/2.5/weather?q=bousse&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for prince rupert index 162 from http://api.openweathermap.org/data/2.5/weather?q=prince rupert&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ilulissat index 163 from http://api.openweathermap.org/data/2.5/weather?q=ilulissat&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for zyryanka index 164 from http://api.openweathermap.org/data/2.5/weather?q=zyryanka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ucluelet index 166 from http://api.openweathermap.org/data/2.5/weather?q=ucluelet&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mortka index 167 from http://api.openweathermap.org/data/2.5/weather?q=mortka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for baykit index 168 from http://api.openweathermap.org/data/2.5/weather?q=baykit&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nikolskoye index 169 from http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for fairbanks index 170 from http://api.openweathermap.org/data/2.5/weather?q=fairbanks&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for solnechnyy index 171 from http://api.openweathermap.org/data/2.5/weather?q=solnechnyy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lorengau index 172 from http://api.openweathermap.org/data/2.5/weather?q=lorengau&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sioux lookout index 173 from http://api.openweathermap.org/data/2.5/weather?q=sioux lookout&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tautira index 174 from http://api.openweathermap.org/data/2.5/weather?q=tautira&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hoi an index 175 from http://api.openweathermap.org/data/2.5/weather?q=hoi an&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ilabaya index 176 from http://api.openweathermap.org/data/2.5/weather?q=ilabaya&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint helens index 177 from http://api.openweathermap.org/data/2.5/weather?q=saint helens&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cabo san lucas index 178 from http://api.openweathermap.org/data/2.5/weather?q=cabo san lucas&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sapa index 179 from http://api.openweathermap.org/data/2.5/weather?q=sapa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for castro index 180 from http://api.openweathermap.org/data/2.5/weather?q=castro&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for senanga index 181 from http://api.openweathermap.org/data/2.5/weather?q=senanga&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for the pas index 182 from http://api.openweathermap.org/data/2.5/weather?q=the pas&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for male index 183 from http://api.openweathermap.org/data/2.5/weather?q=male&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for chokurdakh index 185 from http://api.openweathermap.org/data/2.5/weather?q=chokurdakh&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jian index 186 from http://api.openweathermap.org/data/2.5/weather?q=jian&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for moranbah index 187 from http://api.openweathermap.org/data/2.5/weather?q=moranbah&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for husavik index 188 from http://api.openweathermap.org/data/2.5/weather?q=husavik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bud index 189 from http://api.openweathermap.org/data/2.5/weather?q=bud&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for luanda index 190 from http://api.openweathermap.org/data/2.5/weather?q=luanda&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for souillac index 191 from http://api.openweathermap.org/data/2.5/weather?q=souillac&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for elko index 192 from http://api.openweathermap.org/data/2.5/weather?q=elko&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for qena index 193 from http://api.openweathermap.org/data/2.5/weather?q=qena&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for auki index 194 from http://api.openweathermap.org/data/2.5/weather?q=auki&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kieta index 195 from http://api.openweathermap.org/data/2.5/weather?q=kieta&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tabou index 196 from http://api.openweathermap.org/data/2.5/weather?q=tabou&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jiangyou index 197 from http://api.openweathermap.org/data/2.5/weather?q=jiangyou&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sulangan index 198 from http://api.openweathermap.org/data/2.5/weather?q=sulangan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kasongo-lunda index 199 from http://api.openweathermap.org/data/2.5/weather?q=kasongo-lunda&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for basco index 200 from http://api.openweathermap.org/data/2.5/weather?q=basco&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for san rafael del sur index 201 from http://api.openweathermap.org/data/2.5/weather?q=san rafael del sur&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bowen index 202 from http://api.openweathermap.org/data/2.5/weather?q=bowen&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for te anau index 203 from http://api.openweathermap.org/data/2.5/weather?q=te anau&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for itarema index 204 from http://api.openweathermap.org/data/2.5/weather?q=itarema&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for santander index 205 from http://api.openweathermap.org/data/2.5/weather?q=santander&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for half moon bay index 206 from http://api.openweathermap.org/data/2.5/weather?q=half moon bay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for blagoyevo index 207 from http://api.openweathermap.org/data/2.5/weather?q=blagoyevo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bathsheba index 208 from http://api.openweathermap.org/data/2.5/weather?q=bathsheba&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kavieng index 209 from http://api.openweathermap.org/data/2.5/weather?q=kavieng&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for natal index 211 from http://api.openweathermap.org/data/2.5/weather?q=natal&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hamilton index 212 from http://api.openweathermap.org/data/2.5/weather?q=hamilton&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kuching index 214 from http://api.openweathermap.org/data/2.5/weather?q=kuching&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pedernales index 215 from http://api.openweathermap.org/data/2.5/weather?q=pedernales&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for touros index 217 from http://api.openweathermap.org/data/2.5/weather?q=touros&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for upernavik index 218 from http://api.openweathermap.org/data/2.5/weather?q=upernavik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for arlit index 219 from http://api.openweathermap.org/data/2.5/weather?q=arlit&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kostomuksha index 220 from http://api.openweathermap.org/data/2.5/weather?q=kostomuksha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sisimiut index 221 from http://api.openweathermap.org/data/2.5/weather?q=sisimiut&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for diamantino index 222 from http://api.openweathermap.org/data/2.5/weather?q=diamantino&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for wewak index 223 from http://api.openweathermap.org/data/2.5/weather?q=wewak&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kuytun index 224 from http://api.openweathermap.org/data/2.5/weather?q=kuytun&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for horta index 225 from http://api.openweathermap.org/data/2.5/weather?q=horta&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ewa beach index 226 from http://api.openweathermap.org/data/2.5/weather?q=ewa beach&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for atar index 227 from http://api.openweathermap.org/data/2.5/weather?q=atar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for haysville index 228 from http://api.openweathermap.org/data/2.5/weather?q=haysville&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for uncia index 229 from http://api.openweathermap.org/data/2.5/weather?q=uncia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for shakiso index 230 from http://api.openweathermap.org/data/2.5/weather?q=shakiso&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hobyo index 231 from http://api.openweathermap.org/data/2.5/weather?q=hobyo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for portland index 233 from http://api.openweathermap.org/data/2.5/weather?q=portland&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port lincoln index 234 from http://api.openweathermap.org/data/2.5/weather?q=port lincoln&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for thalgau index 235 from http://api.openweathermap.org/data/2.5/weather?q=thalgau&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for flinders index 236 from http://api.openweathermap.org/data/2.5/weather?q=flinders&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for coahuayana index 237 from http://api.openweathermap.org/data/2.5/weather?q=coahuayana&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for victoria index 238 from http://api.openweathermap.org/data/2.5/weather?q=victoria&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for srednekolymsk index 240 from http://api.openweathermap.org/data/2.5/weather?q=srednekolymsk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nebolchi index 241 from http://api.openweathermap.org/data/2.5/weather?q=nebolchi&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kaitangata index 242 from http://api.openweathermap.org/data/2.5/weather?q=kaitangata&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hasanabad index 243 from http://api.openweathermap.org/data/2.5/weather?q=hasanabad&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint-georges index 244 from http://api.openweathermap.org/data/2.5/weather?q=saint-georges&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for aripuana index 245 from http://api.openweathermap.org/data/2.5/weather?q=aripuana&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for chapais index 246 from http://api.openweathermap.org/data/2.5/weather?q=chapais&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hambantota index 247 from http://api.openweathermap.org/data/2.5/weather?q=hambantota&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pangnirtung index 248 from http://api.openweathermap.org/data/2.5/weather?q=pangnirtung&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for comodoro rivadavia index 249 from http://api.openweathermap.org/data/2.5/weather?q=comodoro rivadavia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mahebourg index 250 from http://api.openweathermap.org/data/2.5/weather?q=mahebourg&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for leh index 251 from http://api.openweathermap.org/data/2.5/weather?q=leh&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ahuimanu index 253 from http://api.openweathermap.org/data/2.5/weather?q=ahuimanu&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tasiilaq index 254 from http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for piracanjuba index 255 from http://api.openweathermap.org/data/2.5/weather?q=piracanjuba&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for buta index 256 from http://api.openweathermap.org/data/2.5/weather?q=buta&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for anadyr index 257 from http://api.openweathermap.org/data/2.5/weather?q=anadyr&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kolvereid index 258 from http://api.openweathermap.org/data/2.5/weather?q=kolvereid&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for luwuk index 260 from http://api.openweathermap.org/data/2.5/weather?q=luwuk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kaspiyskiy index 261 from http://api.openweathermap.org/data/2.5/weather?q=kaspiyskiy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tuatapere index 262 from http://api.openweathermap.org/data/2.5/weather?q=tuatapere&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kodiak index 263 from http://api.openweathermap.org/data/2.5/weather?q=kodiak&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hirara index 264 from http://api.openweathermap.org/data/2.5/weather?q=hirara&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for palkino index 265 from http://api.openweathermap.org/data/2.5/weather?q=palkino&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lucea index 266 from http://api.openweathermap.org/data/2.5/weather?q=lucea&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cururupu index 267 from http://api.openweathermap.org/data/2.5/weather?q=cururupu&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kokopo index 269 from http://api.openweathermap.org/data/2.5/weather?q=kokopo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mount gambier index 270 from http://api.openweathermap.org/data/2.5/weather?q=mount gambier&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for clyde river index 271 from http://api.openweathermap.org/data/2.5/weather?q=clyde river&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for coquimbo index 272 from http://api.openweathermap.org/data/2.5/weather?q=coquimbo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for durres index 273 from http://api.openweathermap.org/data/2.5/weather?q=durres&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sept-iles index 274 from http://api.openweathermap.org/data/2.5/weather?q=sept-iles&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ballina index 275 from http://api.openweathermap.org/data/2.5/weather?q=ballina&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ozernovskiy index 276 from http://api.openweathermap.org/data/2.5/weather?q=ozernovskiy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for north platte index 277 from http://api.openweathermap.org/data/2.5/weather?q=north platte&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for andijon index 278 from http://api.openweathermap.org/data/2.5/weather?q=andijon&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for shimanovsk index 279 from http://api.openweathermap.org/data/2.5/weather?q=shimanovsk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for aksha index 280 from http://api.openweathermap.org/data/2.5/weather?q=aksha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for fairview index 281 from http://api.openweathermap.org/data/2.5/weather?q=fairview&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for padang index 282 from http://api.openweathermap.org/data/2.5/weather?q=padang&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sibolga index 284 from http://api.openweathermap.org/data/2.5/weather?q=sibolga&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for east london index 285 from http://api.openweathermap.org/data/2.5/weather?q=east london&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for richards bay index 286 from http://api.openweathermap.org/data/2.5/weather?q=richards bay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for thompson index 287 from http://api.openweathermap.org/data/2.5/weather?q=thompson&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for komsomolskiy index 288 from http://api.openweathermap.org/data/2.5/weather?q=komsomolskiy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for shilka index 289 from http://api.openweathermap.org/data/2.5/weather?q=shilka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for gravdal index 290 from http://api.openweathermap.org/data/2.5/weather?q=gravdal&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for havoysund index 291 from http://api.openweathermap.org/data/2.5/weather?q=havoysund&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for belyy yar index 292 from http://api.openweathermap.org/data/2.5/weather?q=belyy yar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for codajas index 293 from http://api.openweathermap.org/data/2.5/weather?q=codajas&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lubao index 296 from http://api.openweathermap.org/data/2.5/weather?q=lubao&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hay river index 297 from http://api.openweathermap.org/data/2.5/weather?q=hay river&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for helong index 298 from http://api.openweathermap.org/data/2.5/weather?q=helong&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for northam index 299 from http://api.openweathermap.org/data/2.5/weather?q=northam&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for matara index 300 from http://api.openweathermap.org/data/2.5/weather?q=matara&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yarada index 301 from http://api.openweathermap.org/data/2.5/weather?q=yarada&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for konya index 302 from http://api.openweathermap.org/data/2.5/weather?q=konya&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for maniitsoq index 303 from http://api.openweathermap.org/data/2.5/weather?q=maniitsoq&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for samarai index 304 from http://api.openweathermap.org/data/2.5/weather?q=samarai&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for provideniya index 306 from http://api.openweathermap.org/data/2.5/weather?q=provideniya&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for margate index 308 from http://api.openweathermap.org/data/2.5/weather?q=margate&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kayerkan index 310 from http://api.openweathermap.org/data/2.5/weather?q=kayerkan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kruisfontein index 311 from http://api.openweathermap.org/data/2.5/weather?q=kruisfontein&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for aquiraz index 312 from http://api.openweathermap.org/data/2.5/weather?q=aquiraz&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for arraial do cabo index 313 from http://api.openweathermap.org/data/2.5/weather?q=arraial do cabo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lalsot index 314 from http://api.openweathermap.org/data/2.5/weather?q=lalsot&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for chernoyerkovskaya index 316 from http://api.openweathermap.org/data/2.5/weather?q=chernoyerkovskaya&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ancud index 317 from http://api.openweathermap.org/data/2.5/weather?q=ancud&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for imbituba index 318 from http://api.openweathermap.org/data/2.5/weather?q=imbituba&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for dezhou index 319 from http://api.openweathermap.org/data/2.5/weather?q=dezhou&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lasa index 320 from http://api.openweathermap.org/data/2.5/weather?q=lasa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for boden index 321 from http://api.openweathermap.org/data/2.5/weather?q=boden&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kloulklubed index 322 from http://api.openweathermap.org/data/2.5/weather?q=kloulklubed&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for marsh harbour index 323 from http://api.openweathermap.org/data/2.5/weather?q=marsh harbour&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jawhar index 324 from http://api.openweathermap.org/data/2.5/weather?q=jawhar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for haines junction index 325 from http://api.openweathermap.org/data/2.5/weather?q=haines junction&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port harcourt index 326 from http://api.openweathermap.org/data/2.5/weather?q=port harcourt&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vila velha index 327 from http://api.openweathermap.org/data/2.5/weather?q=vila velha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for isla vista index 328 from http://api.openweathermap.org/data/2.5/weather?q=isla vista&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for baturyn index 329 from http://api.openweathermap.org/data/2.5/weather?q=baturyn&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jinxiang index 330 from http://api.openweathermap.org/data/2.5/weather?q=jinxiang&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nome index 331 from http://api.openweathermap.org/data/2.5/weather?q=nome&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sao felix do xingu index 332 from http://api.openweathermap.org/data/2.5/weather?q=sao felix do xingu&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nabire index 333 from http://api.openweathermap.org/data/2.5/weather?q=nabire&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for byron bay index 334 from http://api.openweathermap.org/data/2.5/weather?q=byron bay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pochutla index 335 from http://api.openweathermap.org/data/2.5/weather?q=pochutla&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jining index 336 from http://api.openweathermap.org/data/2.5/weather?q=jining&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for evensk index 338 from http://api.openweathermap.org/data/2.5/weather?q=evensk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for coihaique index 339 from http://api.openweathermap.org/data/2.5/weather?q=coihaique&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tual index 340 from http://api.openweathermap.org/data/2.5/weather?q=tual&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for rio verde de mato grosso index 341 from http://api.openweathermap.org/data/2.5/weather?q=rio verde de mato grosso&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for palimbang index 342 from http://api.openweathermap.org/data/2.5/weather?q=palimbang&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for morondava index 343 from http://api.openweathermap.org/data/2.5/weather?q=morondava&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for qaqortoq index 344 from http://api.openweathermap.org/data/2.5/weather?q=qaqortoq&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sao joao da barra index 345 from http://api.openweathermap.org/data/2.5/weather?q=sao joao da barra&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mmabatho index 346 from http://api.openweathermap.org/data/2.5/weather?q=mmabatho&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cardenas index 347 from http://api.openweathermap.org/data/2.5/weather?q=cardenas&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cartagena index 350 from http://api.openweathermap.org/data/2.5/weather?q=cartagena&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cherskiy index 351 from http://api.openweathermap.org/data/2.5/weather?q=cherskiy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for gazojak index 352 from http://api.openweathermap.org/data/2.5/weather?q=gazojak&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cap malheureux index 353 from http://api.openweathermap.org/data/2.5/weather?q=cap malheureux&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for russell index 354 from http://api.openweathermap.org/data/2.5/weather?q=russell&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for aklavik index 355 from http://api.openweathermap.org/data/2.5/weather?q=aklavik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for labuhan index 356 from http://api.openweathermap.org/data/2.5/weather?q=labuhan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for esperance index 357 from http://api.openweathermap.org/data/2.5/weather?q=esperance&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for dogbo index 358 from http://api.openweathermap.org/data/2.5/weather?q=dogbo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mantua index 360 from http://api.openweathermap.org/data/2.5/weather?q=mantua&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mikuni index 361 from http://api.openweathermap.org/data/2.5/weather?q=mikuni&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for copperas cove index 362 from http://api.openweathermap.org/data/2.5/weather?q=copperas cove&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for san cristobal index 363 from http://api.openweathermap.org/data/2.5/weather?q=san cristobal&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ornskoldsvik index 364 from http://api.openweathermap.org/data/2.5/weather?q=ornskoldsvik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for houma index 366 from http://api.openweathermap.org/data/2.5/weather?q=houma&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kurilsk index 367 from http://api.openweathermap.org/data/2.5/weather?q=kurilsk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for roald index 368 from http://api.openweathermap.org/data/2.5/weather?q=roald&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for humberto de campos index 370 from http://api.openweathermap.org/data/2.5/weather?q=humberto de campos&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for zhanaozen index 371 from http://api.openweathermap.org/data/2.5/weather?q=zhanaozen&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for inhambane index 372 from http://api.openweathermap.org/data/2.5/weather?q=inhambane&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint-joseph index 373 from http://api.openweathermap.org/data/2.5/weather?q=saint-joseph&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for san ramon index 374 from http://api.openweathermap.org/data/2.5/weather?q=san ramon&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for burnie index 375 from http://api.openweathermap.org/data/2.5/weather?q=burnie&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for krasnyy yar index 376 from http://api.openweathermap.org/data/2.5/weather?q=krasnyy yar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for viedma index 377 from http://api.openweathermap.org/data/2.5/weather?q=viedma&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kudahuvadhoo index 378 from http://api.openweathermap.org/data/2.5/weather?q=kudahuvadhoo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sheridan index 379 from http://api.openweathermap.org/data/2.5/weather?q=sheridan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for doha index 380 from http://api.openweathermap.org/data/2.5/weather?q=doha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bani index 381 from http://api.openweathermap.org/data/2.5/weather?q=bani&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for misratah index 382 from http://api.openweathermap.org/data/2.5/weather?q=misratah&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for alanganallur index 383 from http://api.openweathermap.org/data/2.5/weather?q=alanganallur&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for beringovskiy index 384 from http://api.openweathermap.org/data/2.5/weather?q=beringovskiy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for madinat sittah uktubar index 385 from http://api.openweathermap.org/data/2.5/weather?q=madinat sittah uktubar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tiko index 386 from http://api.openweathermap.org/data/2.5/weather?q=tiko&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for khani index 387 from http://api.openweathermap.org/data/2.5/weather?q=khani&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for dauphin index 388 from http://api.openweathermap.org/data/2.5/weather?q=dauphin&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for srandakan index 389 from http://api.openweathermap.org/data/2.5/weather?q=srandakan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kahului index 390 from http://api.openweathermap.org/data/2.5/weather?q=kahului&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nishihara index 391 from http://api.openweathermap.org/data/2.5/weather?q=nishihara&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for darhan index 392 from http://api.openweathermap.org/data/2.5/weather?q=darhan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pangody index 393 from http://api.openweathermap.org/data/2.5/weather?q=pangody&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for juneau index 395 from http://api.openweathermap.org/data/2.5/weather?q=juneau&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ostrovnoy index 396 from http://api.openweathermap.org/data/2.5/weather?q=ostrovnoy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for gazli index 397 from http://api.openweathermap.org/data/2.5/weather?q=gazli&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nuevo progreso index 398 from http://api.openweathermap.org/data/2.5/weather?q=nuevo progreso&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for terrace bay index 399 from http://api.openweathermap.org/data/2.5/weather?q=terrace bay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port moresby index 402 from http://api.openweathermap.org/data/2.5/weather?q=port moresby&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint paul index 403 from http://api.openweathermap.org/data/2.5/weather?q=saint paul&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ixtapa index 404 from http://api.openweathermap.org/data/2.5/weather?q=ixtapa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pangoa index 406 from http://api.openweathermap.org/data/2.5/weather?q=pangoa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for salalah index 407 from http://api.openweathermap.org/data/2.5/weather?q=salalah&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for gat index 408 from http://api.openweathermap.org/data/2.5/weather?q=gat&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for nemuro index 409 from http://api.openweathermap.org/data/2.5/weather?q=nemuro&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for oranjemund index 410 from http://api.openweathermap.org/data/2.5/weather?q=oranjemund&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tecoanapa index 411 from http://api.openweathermap.org/data/2.5/weather?q=tecoanapa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for palmer index 413 from http://api.openweathermap.org/data/2.5/weather?q=palmer&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for zhigansk index 414 from http://api.openweathermap.org/data/2.5/weather?q=zhigansk&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for san quintin index 415 from http://api.openweathermap.org/data/2.5/weather?q=san quintin&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for collie index 416 from http://api.openweathermap.org/data/2.5/weather?q=collie&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sarangani index 417 from http://api.openweathermap.org/data/2.5/weather?q=sarangani&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for gardendale index 418 from http://api.openweathermap.org/data/2.5/weather?q=gardendale&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for buala index 420 from http://api.openweathermap.org/data/2.5/weather?q=buala&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kununurra index 421 from http://api.openweathermap.org/data/2.5/weather?q=kununurra&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for balabac index 422 from http://api.openweathermap.org/data/2.5/weather?q=balabac&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for eugene index 423 from http://api.openweathermap.org/data/2.5/weather?q=eugene&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for harsin index 424 from http://api.openweathermap.org/data/2.5/weather?q=harsin&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for benguela index 425 from http://api.openweathermap.org/data/2.5/weather?q=benguela&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for iqaluit index 426 from http://api.openweathermap.org/data/2.5/weather?q=iqaluit&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pinawa index 427 from http://api.openweathermap.org/data/2.5/weather?q=pinawa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for chumikan index 428 from http://api.openweathermap.org/data/2.5/weather?q=chumikan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mglin index 429 from http://api.openweathermap.org/data/2.5/weather?q=mglin&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for gueret index 430 from http://api.openweathermap.org/data/2.5/weather?q=gueret&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yatou index 431 from http://api.openweathermap.org/data/2.5/weather?q=yatou&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for iquique index 432 from http://api.openweathermap.org/data/2.5/weather?q=iquique&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for richmond index 433 from http://api.openweathermap.org/data/2.5/weather?q=richmond&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for adrar index 435 from http://api.openweathermap.org/data/2.5/weather?q=adrar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for garwa index 436 from http://api.openweathermap.org/data/2.5/weather?q=garwa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for cidreira index 437 from http://api.openweathermap.org/data/2.5/weather?q=cidreira&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for tomaszow lubelski index 438 from http://api.openweathermap.org/data/2.5/weather?q=tomaszow lubelski&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for vardo index 439 from http://api.openweathermap.org/data/2.5/weather?q=vardo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for campestre index 440 from http://api.openweathermap.org/data/2.5/weather?q=campestre&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for abha index 441 from http://api.openweathermap.org/data/2.5/weather?q=abha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for rosarito index 442 from http://api.openweathermap.org/data/2.5/weather?q=rosarito&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hermosillo index 443 from http://api.openweathermap.org/data/2.5/weather?q=hermosillo&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for topeka index 444 from http://api.openweathermap.org/data/2.5/weather?q=topeka&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lompoc index 446 from http://api.openweathermap.org/data/2.5/weather?q=lompoc&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sangar index 447 from http://api.openweathermap.org/data/2.5/weather?q=sangar&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for olinda index 448 from http://api.openweathermap.org/data/2.5/weather?q=olinda&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for umba index 449 from http://api.openweathermap.org/data/2.5/weather?q=umba&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for zlotow index 450 from http://api.openweathermap.org/data/2.5/weather?q=zlotow&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for liverpool index 453 from http://api.openweathermap.org/data/2.5/weather?q=liverpool&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for marystown index 454 from http://api.openweathermap.org/data/2.5/weather?q=marystown&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bealanana index 455 from http://api.openweathermap.org/data/2.5/weather?q=bealanana&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for karratha index 456 from http://api.openweathermap.org/data/2.5/weather?q=karratha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for canala index 457 from http://api.openweathermap.org/data/2.5/weather?q=canala&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lingao index 458 from http://api.openweathermap.org/data/2.5/weather?q=lingao&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port keats index 459 from http://api.openweathermap.org/data/2.5/weather?q=port keats&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for urengoy index 461 from http://api.openweathermap.org/data/2.5/weather?q=urengoy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for harper index 462 from http://api.openweathermap.org/data/2.5/weather?q=harper&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for wilmington index 463 from http://api.openweathermap.org/data/2.5/weather?q=wilmington&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for sao geraldo do araguaia index 464 from http://api.openweathermap.org/data/2.5/weather?q=sao geraldo do araguaia&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for hofn index 465 from http://api.openweathermap.org/data/2.5/weather?q=hofn&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for saint-francois index 467 from http://api.openweathermap.org/data/2.5/weather?q=saint-francois&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for churapcha index 468 from http://api.openweathermap.org/data/2.5/weather?q=churapcha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for oussouye index 469 from http://api.openweathermap.org/data/2.5/weather?q=oussouye&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for salinopolis index 470 from http://api.openweathermap.org/data/2.5/weather?q=salinopolis&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yurla index 471 from http://api.openweathermap.org/data/2.5/weather?q=yurla&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for dongsheng index 472 from http://api.openweathermap.org/data/2.5/weather?q=dongsheng&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ponerihouen index 473 from http://api.openweathermap.org/data/2.5/weather?q=ponerihouen&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for laguna index 474 from http://api.openweathermap.org/data/2.5/weather?q=laguna&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for port-cartier index 475 from http://api.openweathermap.org/data/2.5/weather?q=port-cartier&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for araruama index 476 from http://api.openweathermap.org/data/2.5/weather?q=araruama&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for turayf index 477 from http://api.openweathermap.org/data/2.5/weather?q=turayf&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yar-sale index 478 from http://api.openweathermap.org/data/2.5/weather?q=yar-sale&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for ust-kulom index 479 from http://api.openweathermap.org/data/2.5/weather?q=ust-kulom&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for rorvik index 480 from http://api.openweathermap.org/data/2.5/weather?q=rorvik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for alliston index 481 from http://api.openweathermap.org/data/2.5/weather?q=alliston&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for yaan index 482 from http://api.openweathermap.org/data/2.5/weather?q=yaan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for mount isa index 483 from http://api.openweathermap.org/data/2.5/weather?q=mount isa&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for acapulco index 484 from http://api.openweathermap.org/data/2.5/weather?q=acapulco&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for alice springs index 486 from http://api.openweathermap.org/data/2.5/weather?q=alice springs&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for umm lajj index 487 from http://api.openweathermap.org/data/2.5/weather?q=umm lajj&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for rocha index 488 from http://api.openweathermap.org/data/2.5/weather?q=rocha&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for grindavik index 489 from http://api.openweathermap.org/data/2.5/weather?q=grindavik&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for jizan index 490 from http://api.openweathermap.org/data/2.5/weather?q=jizan&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for pevek index 491 from http://api.openweathermap.org/data/2.5/weather?q=pevek&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for kedrovyy index 493 from http://api.openweathermap.org/data/2.5/weather?q=kedrovyy&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for lebu index 495 from http://api.openweathermap.org/data/2.5/weather?q=lebu&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for warrensburg index 496 from http://api.openweathermap.org/data/2.5/weather?q=warrensburg&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for matay index 497 from http://api.openweathermap.org/data/2.5/weather?q=matay&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for bowling green index 498 from http://api.openweathermap.org/data/2.5/weather?q=bowling green&APPID=03f86711a729435d41bf2a6a7729afb3
    Calling information for waddan index 499 from http://api.openweathermap.org/data/2.5/weather?q=waddan&APPID=03f86711a729435d41bf2a6a7729afb3





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Name</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Windspeed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bluff</td>
      <td>-82.689777</td>
      <td>155.154586</td>
      <td>89.3552</td>
      <td>60</td>
      <td>20</td>
      <td>4.11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>punta arenas</td>
      <td>-56.164619</td>
      <td>-96.538650</td>
      <td>44.6</td>
      <td>81</td>
      <td>75</td>
      <td>10.3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ushuaia</td>
      <td>-64.767769</td>
      <td>-42.678560</td>
      <td>46.4</td>
      <td>81</td>
      <td>75</td>
      <td>5.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>new norfolk</td>
      <td>-71.847960</td>
      <td>134.165846</td>
      <td>62.6</td>
      <td>72</td>
      <td>75</td>
      <td>5.7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>puerto ayora</td>
      <td>-20.408405</td>
      <td>-100.397595</td>
      <td>76.0802</td>
      <td>100</td>
      <td>20</td>
      <td>0.96</td>
    </tr>
  </tbody>
</table>
</div>




```python
citiesdf.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Name</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Windspeed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bluff</td>
      <td>-82.689777</td>
      <td>155.154586</td>
      <td>89.3552</td>
      <td>60</td>
      <td>20</td>
      <td>4.11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>punta arenas</td>
      <td>-56.164619</td>
      <td>-96.538650</td>
      <td>44.6</td>
      <td>81</td>
      <td>75</td>
      <td>10.3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ushuaia</td>
      <td>-64.767769</td>
      <td>-42.678560</td>
      <td>46.4</td>
      <td>81</td>
      <td>75</td>
      <td>5.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>new norfolk</td>
      <td>-71.847960</td>
      <td>134.165846</td>
      <td>62.6</td>
      <td>72</td>
      <td>75</td>
      <td>5.7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>puerto ayora</td>
      <td>-20.408405</td>
      <td>-100.397595</td>
      <td>76.0802</td>
      <td>100</td>
      <td>20</td>
      <td>0.96</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(y)
```

    0      305.014
    1       280.15
    2       281.15
    3       290.15
    4      297.639
    5       308.15
    7       272.15
    8      301.989
    9      270.114
    10      282.81
    11     298.789
    12     283.889
    13     294.014
    14      289.63
    16     292.039
    17      278.15
    18     300.864
    19     257.539
    20     298.164
    21      301.15
    22     291.389
    23     298.264
    24     304.364
    25     294.764
    26     291.964
    27      273.65
    29     291.589
    30     290.389
    31     280.039
    32     281.864
            ...   
    467     272.15
    468    247.364
    469     293.15
    470    299.764
    471    260.114
    472    287.539
    473    300.364
    474    284.639
    475     262.15
    476    296.514
    477     282.15
    478    260.889
    479    262.914
    480     259.18
    481     275.42
    482    295.589
    483     304.15
    484     299.15
    486     310.15
    487    293.114
    488    288.464
    489     274.35
    490     302.15
    491    261.764
    493     256.15
    495    290.364
    496     283.43
    497    287.214
    498     287.49
    499    279.914
    Name: Temperature, Length: 446, dtype: object



```python
#create scatterplot for temperature
x = citiesdf['Latitude']
plt.xlabel("Latitude")
plt.xlim(-90, 90)

#set y, allow outliers in axis, 

y = citiesdf['Temperature']
plt.ylabel('Temperature (f)', fontsize = 20)
plt.ylim(-50, 130)

# use the scatter function
plt.scatter(x, y)
plt.rcParams["figure.figsize"] = [16,9]

#adding a title
plt.title('Latitude vs. Temperature', fontsize = 25)

plt.savefig('temperature.pdf')
plt.show()
```


![png](output_11_0.png)



```python
#create scatterplot for humidity
x = citiesdf['Latitude']
plt.xlabel("Latitude")
plt.xlim(-90, 90)

#set y, allow outliers in axis, 

y = citiesdf['Humidity']
plt.ylabel('Humidity (%)', fontsize = 20)
plt.ylim(-10, 110)

# use the scatter function
plt.scatter(x, y)
plt.rcParams["figure.figsize"] = [16,9]

#adding a title
plt.title('Latitude vs. Humidity', fontsize = 25)

plt.savefig('humidity.pdf')
plt.show()
```


![png](output_12_0.png)



```python
#create scatterplot for cloudiness
x = citiesdf['Latitude']
plt.xlabel("Latitude")
plt.xlim(-90, 90)

#set y, allow outliers in axis, 

y = citiesdf['Cloudiness']
plt.ylabel('Cloudiness (%)', fontsize = 20)
plt.ylim(-10, 110)

# use the scatter function
plt.scatter(x, y)
plt.rcParams["figure.figsize"] = [16,9]

#adding a title
plt.title('Latitude vs. Cloudiness', fontsize = 25)

plt.savefig('cloudiness.pdf')
plt.show()
```


![png](output_13_0.png)



```python
#create scatterplot for humidity
x = citiesdf['Latitude']
plt.xlabel("Latitude")
plt.xlim(-90, 90)

#set y, allow outliers in axis, 

y = citiesdf['Windspeed']
plt.ylabel('Windspeed', fontsize = 20)
plt.ylim(0, 30)

# use the scatter function
plt.scatter(x, y)
plt.rcParams["figure.figsize"] = [16,9]

#adding a title
plt.title('Latitude vs. Windspeed', fontsize = 25)

plt.savefig('humidity.pdf')
plt.show()
```


![png](output_14_0.png)



```python
print("Observation 1. The latitude vs. temperature relationship is demonstrating the occurence of seasons. Currently, it is still winter in the Northern Hemisphere therefore temps are generally lower. Furthermore, as you go further away from the equator (higher latitude) you encounter lower temperatures.")
```

    Observation 1. The latitude vs. temperature relationship is demonstrating the occurence of seasons. Currently, it is still winter in the Northern Hemisphere therefore temps are generally lower. Furthermore, as you go further away from the equator (higher latitude) you encounter lower temperatures.



```python
print('Observation 2. Higher humidity seems to be clustered around the equator. The further from the equater, the less likely that humidity is at 100%')
```


```python
print("Observation 3. Windspeed appears to increase as we get further from the equator in both directions. However, due to the lack of cities in the southern hemisphere, there isn't a clealry defined symmetry to make this suggestion.")
```
