

```python
import numpy as np 

import pandas as pd 
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)

import json

from geopy.geocoders import Nominatim 

import requests 
from pandas.io.json import json_normalize 

import matplotlib.cm as cm
import matplotlib.colors as colors

from sklearn.cluster import KMeans

import folium
```


```python
CLIENT_ID = 'Y4KMS3PAB54UJZYMXWD3FT05P04QQJ15HFQJNH3Q4YJGKZJM' 
CLIENT_SECRET = 'OU1M4WVB1XD50GXWYSSYZ0AJTE3QZUJGT4ZXEN3ZZZUCDX10'
VERSION = '20200418'
LIMIT= 50
```


```python
city = 'Villa de Leyva'
geolocator = Nominatim(user_agent="foursquare_agent")
location = geolocator.geocode(city)
latitude = location.latitude
longitude = location.longitude
print(latitude, longitude)
```

    5.6336805 -73.523548



```python
search_query = 'Hotel'
radius = 500

url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'\
.format(CLIENT_ID, CLIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)

result_hotel = requests.get(url).json()

result_hotel

venue_hotel = result_hotel['response']['venues']

df = json_normalize(venue_hotel)
df.head()
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
      <th>categories</th>
      <th>hasPerk</th>
      <th>id</th>
      <th>location.address</th>
      <th>location.cc</th>
      <th>location.city</th>
      <th>location.country</th>
      <th>location.crossStreet</th>
      <th>location.distance</th>
      <th>location.formattedAddress</th>
      <th>location.labeledLatLngs</th>
      <th>location.lat</th>
      <th>location.lng</th>
      <th>location.postalCode</th>
      <th>location.state</th>
      <th>name</th>
      <th>referralId</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4e3cb9c7ae604542365c7d1f</td>
      <td>Calle 8 # 10-35</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>496</td>
      <td>[Calle 8 # 10-35, Villa De Leyva, Boyacá, Colo...</td>
      <td>[{'label': 'display', 'lat': 5.633063855150655...</td>
      <td>5.633064</td>
      <td>-73.527990</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>Hotel &amp; Spa Getsemaní</td>
      <td>v-1587172300</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[{'id': '4bf58dd8d48988d1ed941735', 'name': 'S...</td>
      <td>False</td>
      <td>4f526baae4b023845e5fdd2f</td>
      <td>Calle 8 # 10-35</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>497</td>
      <td>[Calle 8 # 10-35, Villa De Leyva, Boyacá, Colo...</td>
      <td>[{'label': 'display', 'lat': 5.633199593051142...</td>
      <td>5.633200</td>
      <td>-73.528011</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>Elea Spa - Hotel &amp; Spa Getsemaní</td>
      <td>v-1587172300</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4ebf521993ad36d7a8e101d0</td>
      <td>Calle 10 7-31</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>87</td>
      <td>[Calle 10 7-31, Villa de Leyva, Boyacá, Boyacá...</td>
      <td>[{'label': 'display', 'lat': 5.634087665696955...</td>
      <td>5.634088</td>
      <td>-73.524222</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>Hotel Plaza Mayor</td>
      <td>v-1587172300</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4cf3f52588de37044318822b</td>
      <td>Calle 8</td>
      <td>CO</td>
      <td>Villa de Lleyva</td>
      <td>Colombia</td>
      <td>Carrera 11</td>
      <td>159</td>
      <td>[Calle 8 (Carrera 11), Villa de Lleyva, Boyacá...</td>
      <td>[{'label': 'display', 'lat': 5.63236, 'lng': -...</td>
      <td>5.632360</td>
      <td>-73.522994</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>Hotel San Antonio</td>
      <td>v-1587172300</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4e0fa8f37d8bb178a8b5221d</td>
      <td>NaN</td>
      <td>CO</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>239</td>
      <td>[Boyacá, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.634973593050659...</td>
      <td>5.634974</td>
      <td>-73.521820</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>Hotel Mesón de los Virreyes</td>
      <td>v-1587172300</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_columns = ['name', 'categories'] + [col for col in df.columns if col.startswith('location.')]+ ['id']
new_df = df.loc[:,new_columns]

def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']
        
    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']

new_df['categories'] = new_df.apply(get_category_type, axis=1)

new_df.columns = [column.split('.')[-1] for column in new_df.columns]

new_df.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>crossStreet</th>
      <th>distance</th>
      <th>formattedAddress</th>
      <th>labeledLatLngs</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Hotel &amp; Spa Getsemaní</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-35</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>496</td>
      <td>[Calle 8 # 10-35, Villa De Leyva, Boyacá, Colo...</td>
      <td>[{'label': 'display', 'lat': 5.633063855150655...</td>
      <td>5.633064</td>
      <td>-73.527990</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>4e3cb9c7ae604542365c7d1f</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Elea Spa - Hotel &amp; Spa Getsemaní</td>
      <td>Spa</td>
      <td>Calle 8 # 10-35</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>497</td>
      <td>[Calle 8 # 10-35, Villa De Leyva, Boyacá, Colo...</td>
      <td>[{'label': 'display', 'lat': 5.633199593051142...</td>
      <td>5.633200</td>
      <td>-73.528011</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>4f526baae4b023845e5fdd2f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Hotel Plaza Mayor</td>
      <td>Hotel</td>
      <td>Calle 10 7-31</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>87</td>
      <td>[Calle 10 7-31, Villa de Leyva, Boyacá, Boyacá...</td>
      <td>[{'label': 'display', 'lat': 5.634087665696955...</td>
      <td>5.634088</td>
      <td>-73.524222</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>4ebf521993ad36d7a8e101d0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hotel San Antonio</td>
      <td>Hotel</td>
      <td>Calle 8</td>
      <td>CO</td>
      <td>Villa de Lleyva</td>
      <td>Colombia</td>
      <td>Carrera 11</td>
      <td>159</td>
      <td>[Calle 8 (Carrera 11), Villa de Lleyva, Boyacá...</td>
      <td>[{'label': 'display', 'lat': 5.63236, 'lng': -...</td>
      <td>5.632360</td>
      <td>-73.522994</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>4cf3f52588de37044318822b</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hotel Mesón de los Virreyes</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>CO</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>239</td>
      <td>[Boyacá, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.634973593050659...</td>
      <td>5.634974</td>
      <td>-73.521820</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>4e0fa8f37d8bb178a8b5221d</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df= new_df.drop(['cc', 'city', 'country', 'crossStreet', 'distance', 'formattedAddress',\
                                        'labeledLatLngs', 'id'], axis=1)
new_df
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Hotel &amp; Spa Getsemaní</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-35</td>
      <td>5.633064</td>
      <td>-73.527990</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Elea Spa - Hotel &amp; Spa Getsemaní</td>
      <td>Spa</td>
      <td>Calle 8 # 10-35</td>
      <td>5.633200</td>
      <td>-73.528011</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Hotel Plaza Mayor</td>
      <td>Hotel</td>
      <td>Calle 10 7-31</td>
      <td>5.634088</td>
      <td>-73.524222</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hotel San Antonio</td>
      <td>Hotel</td>
      <td>Calle 8</td>
      <td>5.632360</td>
      <td>-73.522994</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hotel Mesón de los Virreyes</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634974</td>
      <td>-73.521820</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hotel Y centro de convenciones casa de los fun...</td>
      <td>Hotel</td>
      <td>1km Via Guanani</td>
      <td>5.633522</td>
      <td>-73.524962</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hotel Boutique La Espanola</td>
      <td>Hotel</td>
      <td>Calle 12 No. 11 - 06</td>
      <td>5.634637</td>
      <td>-73.525069</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hotel Villa Paz</td>
      <td>Ski Lodge</td>
      <td>Crr 11a no. 12 - 27</td>
      <td>5.635055</td>
      <td>-73.525024</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Hotel Casa Cantabria</td>
      <td>Bed &amp; Breakfast</td>
      <td>NaN</td>
      <td>5.632814</td>
      <td>-73.525732</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Hotel Antonio Nariño</td>
      <td>Bed &amp; Breakfast</td>
      <td>Villa De Leyva</td>
      <td>5.631885</td>
      <td>-73.524600</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hotel Plazuela de San Agustín</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634718</td>
      <td>-73.521240</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hotel El Peregrino de la Villa</td>
      <td>Bed &amp; Breakfast</td>
      <td>NaN</td>
      <td>5.634078</td>
      <td>-73.526009</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Hotel Aqua Vitae</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634297</td>
      <td>-73.528244</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Hotel Campanario de la Villa</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.635248</td>
      <td>-73.525080</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Hotel Miracle</td>
      <td>Hotel</td>
      <td>Carrera 11 #9-41</td>
      <td>5.633403</td>
      <td>-73.526484</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hotel Fontana Plaza</td>
      <td>Hotel</td>
      <td>Carrera 9 #9-27</td>
      <td>5.631549</td>
      <td>-73.525750</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>16</th>
      <td>hotel villa roma</td>
      <td>Bed &amp; Breakfast</td>
      <td>NaN</td>
      <td>5.631480</td>
      <td>-73.525779</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Hotel Mansión la Toscana</td>
      <td>Bed &amp; Breakfast</td>
      <td>NaN</td>
      <td>5.632132</td>
      <td>-73.520461</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Hotel Villa del Angel</td>
      <td>Bed &amp; Breakfast</td>
      <td>NaN</td>
      <td>5.633472</td>
      <td>-73.526970</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Hotel Andrés Venero De Leyva</td>
      <td>Bed &amp; Breakfast</td>
      <td>Avenida Circunvalar Calle 11 Esquina</td>
      <td>5.635077</td>
      <td>-73.527659</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Hotel Los Frayles</td>
      <td>Hostel</td>
      <td>Calle 8 9-38</td>
      <td>5.631242</td>
      <td>-73.526556</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Hotel Bahia Olivo</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.633185</td>
      <td>-73.527684</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Hotel spa bahía olivo</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.633118</td>
      <td>-73.527534</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Hotel la Hormiga</td>
      <td>Bed &amp; Breakfast</td>
      <td>Cra 9 # 7-59</td>
      <td>5.630166</td>
      <td>-73.526533</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Hotel Getzemani</td>
      <td>Hotel</td>
      <td>Calle 8 villa de leyva</td>
      <td>5.632824</td>
      <td>-73.528074</td>
      <td>NaN</td>
      <td>Boyacá Dept</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hotel Santa Viviana</td>
      <td>Hotel</td>
      <td>Diagonal 12 # 8a-76</td>
      <td>5.634710</td>
      <td>-73.527916</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Hotel Andres Venero de Leyva</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.635013</td>
      <td>-73.527551</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>hotel Abadia de la Villa</td>
      <td>Bed &amp; Breakfast</td>
      <td>Calle 8 10-97</td>
      <td>5.633635</td>
      <td>-73.527911</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Hotel Abahunza</td>
      <td>Resort</td>
      <td>NaN</td>
      <td>5.633583</td>
      <td>-73.527856</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>hotel las orquideas de la villa</td>
      <td>Hotel</td>
      <td>Calle 12 # 10-11</td>
      <td>5.636492</td>
      <td>-73.527236</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Hotel Campanario Real</td>
      <td>Boarding House</td>
      <td>NaN</td>
      <td>5.637163</td>
      <td>-73.526375</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Hotel Arcadia Colonial</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-19B</td>
      <td>5.634379</td>
      <td>-73.528293</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Hotel Marques De San Luis</td>
      <td>Hotel</td>
      <td>Circunvalacion 8-58</td>
      <td>5.634897</td>
      <td>-73.528138</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>33</th>
      <td>El Solar Hotel</td>
      <td>Bed &amp; Breakfast</td>
      <td>Call 10a # 10-60</td>
      <td>5.633351</td>
      <td>-73.525843</td>
      <td>00000</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Hotel Casa Real Villa de Leyva</td>
      <td>Hotel Pool</td>
      <td>Calle 17#10-14</td>
      <td>5.637400</td>
      <td>-73.520202</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Hotel Estancia Los Olivos</td>
      <td>Bed &amp; Breakfast</td>
      <td>NaN</td>
      <td>5.632955</td>
      <td>-73.528230</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>36</th>
      <td>posada de la villa hotel</td>
      <td>Hotel</td>
      <td>Calle 14 carrera 8</td>
      <td>5.633887</td>
      <td>-73.521225</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Hotel Villa Palva</td>
      <td>Hotel</td>
      <td>cra 9a #7 37</td>
      <td>5.630548</td>
      <td>-73.527710</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Hotel Boutique Villa Roma</td>
      <td>Hotel</td>
      <td>Calle 8 #10 -166</td>
      <td>5.634107</td>
      <td>-73.528102</td>
      <td>058</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Bar Hotel Plaza Mayor</td>
      <td>Beer Garden</td>
      <td>NaN</td>
      <td>5.629856</td>
      <td>-73.524243</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>40</th>
      <td>MiniHotel</td>
      <td>Hostel</td>
      <td>Calle 10 A # 10 - 29</td>
      <td>5.632720</td>
      <td>-73.521250</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Molino La Mesopotamia</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.636165</td>
      <td>-73.518837</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
  </tbody>
</table>
</div>




```python


array= ['Hotel']
df_hotel= new_df.loc[new_df['categories'].isin(array)]
df_hotel


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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Hotel &amp; Spa Getsemaní</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-35</td>
      <td>5.633064</td>
      <td>-73.527990</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Hotel Plaza Mayor</td>
      <td>Hotel</td>
      <td>Calle 10 7-31</td>
      <td>5.634088</td>
      <td>-73.524222</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hotel San Antonio</td>
      <td>Hotel</td>
      <td>Calle 8</td>
      <td>5.632360</td>
      <td>-73.522994</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hotel Mesón de los Virreyes</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634974</td>
      <td>-73.521820</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hotel Y centro de convenciones casa de los fun...</td>
      <td>Hotel</td>
      <td>1km Via Guanani</td>
      <td>5.633522</td>
      <td>-73.524962</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hotel Boutique La Espanola</td>
      <td>Hotel</td>
      <td>Calle 12 No. 11 - 06</td>
      <td>5.634637</td>
      <td>-73.525069</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hotel Plazuela de San Agustín</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634718</td>
      <td>-73.521240</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Hotel Aqua Vitae</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634297</td>
      <td>-73.528244</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Hotel Campanario de la Villa</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.635248</td>
      <td>-73.525080</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Hotel Miracle</td>
      <td>Hotel</td>
      <td>Carrera 11 #9-41</td>
      <td>5.633403</td>
      <td>-73.526484</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hotel Fontana Plaza</td>
      <td>Hotel</td>
      <td>Carrera 9 #9-27</td>
      <td>5.631549</td>
      <td>-73.525750</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Hotel Bahia Olivo</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.633185</td>
      <td>-73.527684</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Hotel spa bahía olivo</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.633118</td>
      <td>-73.527534</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Hotel Getzemani</td>
      <td>Hotel</td>
      <td>Calle 8 villa de leyva</td>
      <td>5.632824</td>
      <td>-73.528074</td>
      <td>NaN</td>
      <td>Boyacá Dept</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hotel Santa Viviana</td>
      <td>Hotel</td>
      <td>Diagonal 12 # 8a-76</td>
      <td>5.634710</td>
      <td>-73.527916</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Hotel Andres Venero de Leyva</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.635013</td>
      <td>-73.527551</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>hotel las orquideas de la villa</td>
      <td>Hotel</td>
      <td>Calle 12 # 10-11</td>
      <td>5.636492</td>
      <td>-73.527236</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Hotel Arcadia Colonial</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-19B</td>
      <td>5.634379</td>
      <td>-73.528293</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Hotel Marques De San Luis</td>
      <td>Hotel</td>
      <td>Circunvalacion 8-58</td>
      <td>5.634897</td>
      <td>-73.528138</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>36</th>
      <td>posada de la villa hotel</td>
      <td>Hotel</td>
      <td>Calle 14 carrera 8</td>
      <td>5.633887</td>
      <td>-73.521225</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Hotel Villa Palva</td>
      <td>Hotel</td>
      <td>cra 9a #7 37</td>
      <td>5.630548</td>
      <td>-73.527710</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Hotel Boutique Villa Roma</td>
      <td>Hotel</td>
      <td>Calle 8 #10 -166</td>
      <td>5.634107</td>
      <td>-73.528102</td>
      <td>058</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Molino La Mesopotamia</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.636165</td>
      <td>-73.518837</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
  </tbody>
</table>
</div>




```python
search_query = 'Restaurant'
radius = 10000

url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'.format(CLIENT_ID, CLIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)
url
```




    'https://api.foursquare.com/v2/venues/search?client_id=Y4KMS3PAB54UJZYMXWD3FT05P04QQJ15HFQJNH3Q4YJGKZJM&client_secret=OU1M4WVB1XD50GXWYSSYZ0AJTE3QZUJGT4ZXEN3ZZZUCDX10&ll=5.6336805,-73.523548&v=20200418&query=Restaurant&radius=10000&limit=50'




```python
result_restaurant = requests.get(url).json()

venue_restaurant = result_restaurant['response']['venues']

Restaurant_df = json_normalize(venue_restaurant)
Restaurant_df.head()
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
      <th>categories</th>
      <th>hasPerk</th>
      <th>id</th>
      <th>location.address</th>
      <th>location.cc</th>
      <th>location.city</th>
      <th>location.country</th>
      <th>location.crossStreet</th>
      <th>location.distance</th>
      <th>location.formattedAddress</th>
      <th>location.labeledLatLngs</th>
      <th>location.lat</th>
      <th>location.lng</th>
      <th>location.neighborhood</th>
      <th>location.postalCode</th>
      <th>location.state</th>
      <th>name</th>
      <th>referralId</th>
      <th>venuePage.id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[{'id': '4bf58dd8d48988d1ca941735', 'name': 'P...</td>
      <td>False</td>
      <td>4f370d08e4b0d3ab511ef894</td>
      <td>Villa de leyva</td>
      <td>CO</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>33</td>
      <td>[Villa de leyva, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.633776342358105...</td>
      <td>5.633776</td>
      <td>-73.523260</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Restaurante Portales</td>
      <td>v-1587172503</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[{'id': '52e81612bcbc57f1066b7a00', 'name': 'C...</td>
      <td>False</td>
      <td>534ead7d498e662b28a28cbd</td>
      <td>NaN</td>
      <td>CO</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>32</td>
      <td>[Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.633417588346402...</td>
      <td>5.633418</td>
      <td>-73.523680</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>El Santo Restaurante Gourmet</td>
      <td>v-1587172503</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[{'id': '4bf58dd8d48988d10c941735', 'name': 'F...</td>
      <td>False</td>
      <td>4ef607e20e61a0846c52c3d6</td>
      <td>NaN</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>101</td>
      <td>[Villa de Leyva, Boyacá, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.634233089432019...</td>
      <td>5.634233</td>
      <td>-73.522817</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>Restaurante Chez Rémy</td>
      <td>v-1587172503</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[{'id': '4bf58dd8d48988d1c4941735', 'name': 'R...</td>
      <td>False</td>
      <td>50edd8a9e4b037265a58382e</td>
      <td>Cr10 No 11-50</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>130</td>
      <td>[Cr10 No 11-50, Villa de Leyva, Boyacá, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.633534154682136...</td>
      <td>5.633534</td>
      <td>-73.524718</td>
      <td>Centro Historico</td>
      <td>154001</td>
      <td>Boyacá</td>
      <td>Restaurante  Carnes y Olivas</td>
      <td>v-1587172503</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[{'id': '4bf58dd8d48988d10c941735', 'name': 'F...</td>
      <td>False</td>
      <td>4e8798c630f83936723c7bbc</td>
      <td>Carrera 9 13-51</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>120</td>
      <td>[Carrera 9 13-51, Villa de Leyva, Boyacá, Colo...</td>
      <td>[{'label': 'display', 'lat': 5.634215967092749...</td>
      <td>5.634216</td>
      <td>-73.522599</td>
      <td>NaN</td>
      <td>154001</td>
      <td>Boyacá</td>
      <td>Restaurante Mama Santa</td>
      <td>v-1587172503</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
Restaurant_new_columns = ['name', 'categories'] + [col for col in Restaurant_df.columns if col.startswith('location.')]+ ['id']
new_df_restaurant = Restaurant_df.loc[:,Restaurant_new_columns]

def get_category_type(row):
    try:
        categories_list3 = row['categories']
    except:
        categories_list3 = row['venue.categories']
        
    if len(categories_list3) == 0:
        return None
    else:
        return categories_list3[0]['name']

new_df_restaurant['categories'] = new_df_restaurant.apply(get_category_type, axis=1)

new_df_restaurant.columns = [column.split('.')[-1] for column in new_df_restaurant.columns]

new_df_restaurant.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>crossStreet</th>
      <th>distance</th>
      <th>formattedAddress</th>
      <th>labeledLatLngs</th>
      <th>lat</th>
      <th>lng</th>
      <th>neighborhood</th>
      <th>postalCode</th>
      <th>state</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Restaurante Portales</td>
      <td>Pizza Place</td>
      <td>Villa de leyva</td>
      <td>CO</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>33</td>
      <td>[Villa de leyva, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.633776342358105...</td>
      <td>5.633776</td>
      <td>-73.523260</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4f370d08e4b0d3ab511ef894</td>
    </tr>
    <tr>
      <th>1</th>
      <td>El Santo Restaurante Gourmet</td>
      <td>Comfort Food Restaurant</td>
      <td>NaN</td>
      <td>CO</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>32</td>
      <td>[Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.633417588346402...</td>
      <td>5.633418</td>
      <td>-73.523680</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>534ead7d498e662b28a28cbd</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Restaurante Chez Rémy</td>
      <td>French Restaurant</td>
      <td>NaN</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>101</td>
      <td>[Villa de Leyva, Boyacá, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.634233089432019...</td>
      <td>5.634233</td>
      <td>-73.522817</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Boyacá</td>
      <td>4ef607e20e61a0846c52c3d6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Restaurante  Carnes y Olivas</td>
      <td>Restaurant</td>
      <td>Cr10 No 11-50</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>130</td>
      <td>[Cr10 No 11-50, Villa de Leyva, Boyacá, Colombia]</td>
      <td>[{'label': 'display', 'lat': 5.633534154682136...</td>
      <td>5.633534</td>
      <td>-73.524718</td>
      <td>Centro Historico</td>
      <td>154001</td>
      <td>Boyacá</td>
      <td>50edd8a9e4b037265a58382e</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Restaurante Mama Santa</td>
      <td>French Restaurant</td>
      <td>Carrera 9 13-51</td>
      <td>CO</td>
      <td>Villa de Leiva</td>
      <td>Colombia</td>
      <td>NaN</td>
      <td>120</td>
      <td>[Carrera 9 13-51, Villa de Leyva, Boyacá, Colo...</td>
      <td>[{'label': 'display', 'lat': 5.634215967092749...</td>
      <td>5.634216</td>
      <td>-73.522599</td>
      <td>NaN</td>
      <td>154001</td>
      <td>Boyacá</td>
      <td>4e8798c630f83936723c7bbc</td>
    </tr>
  </tbody>
</table>
</div>




```python


new_df_restaurant= new_df_restaurant.drop(['cc', 'city', 'country', 'crossStreet', 'distance', 'formattedAddress',\
                                        'labeledLatLngs', 'neighborhood', 'id'], axis=1)
new_df_restaurant


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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Restaurante Portales</td>
      <td>Pizza Place</td>
      <td>Villa de leyva</td>
      <td>5.633776</td>
      <td>-73.523260</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>El Santo Restaurante Gourmet</td>
      <td>Comfort Food Restaurant</td>
      <td>NaN</td>
      <td>5.633418</td>
      <td>-73.523680</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Restaurante Chez Rémy</td>
      <td>French Restaurant</td>
      <td>NaN</td>
      <td>5.634233</td>
      <td>-73.522817</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Restaurante  Carnes y Olivas</td>
      <td>Restaurant</td>
      <td>Cr10 No 11-50</td>
      <td>5.633534</td>
      <td>-73.524718</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Restaurante Mama Santa</td>
      <td>French Restaurant</td>
      <td>Carrera 9 13-51</td>
      <td>5.634216</td>
      <td>-73.522599</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Los Portales Restaurante</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.633803</td>
      <td>-73.523068</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Restaurante El Caney</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.634393</td>
      <td>-73.521957</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Restaurante Peruano del Carajo</td>
      <td>Peruvian Restaurant</td>
      <td>NaN</td>
      <td>5.634674</td>
      <td>-73.525313</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>La Romana Restaurante y Pizzeria</td>
      <td>Pizza Place</td>
      <td>Calle 12# 9- 67</td>
      <td>5.633489</td>
      <td>-73.524030</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Savia Orgánica - Restaurante</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.633183</td>
      <td>-73.523708</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Restaurante La Albahaca</td>
      <td>Latin American Restaurant</td>
      <td>Villa de Leyva</td>
      <td>5.633149</td>
      <td>-73.521599</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Restaurante Rincón de Bachué</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.634648</td>
      <td>-73.521396</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Los Arcos Pizzeria Y Restaurante</td>
      <td>American Restaurant</td>
      <td>NaN</td>
      <td>5.633485</td>
      <td>-73.524075</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Terraza Restaurante Bar</td>
      <td>BBQ Joint</td>
      <td>NaN</td>
      <td>5.634082</td>
      <td>-73.523140</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Restaurante Los Kioscos De Los Caciques</td>
      <td>Latin American Restaurant</td>
      <td>NaN</td>
      <td>5.631474</td>
      <td>-73.525944</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>15</th>
      <td>portales restaurante pizzeria y fruteria</td>
      <td>Pizza Place</td>
      <td>NaN</td>
      <td>5.633981</td>
      <td>-73.522995</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Restaurante Casa Blanca</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.632438</td>
      <td>-73.521440</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Arcadia Bar Restaurante</td>
      <td>Italian Restaurant</td>
      <td>calle 13 9-10</td>
      <td>5.633889</td>
      <td>-73.522909</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Restaurante Tabú Cocina &amp; Arte</td>
      <td>Fast Food Restaurant</td>
      <td>Cra 7 N 7b - 49</td>
      <td>5.629348</td>
      <td>-73.525384</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Restaurante Tabú</td>
      <td>Mexican Restaurant</td>
      <td>NaN</td>
      <td>5.629294</td>
      <td>-73.525210</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Restaurante Tipico</td>
      <td>Latin American Restaurant</td>
      <td>NaN</td>
      <td>5.636462</td>
      <td>-73.527466</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Restaurante Nueva Granada</td>
      <td>Latin American Restaurant</td>
      <td>NaN</td>
      <td>5.632230</td>
      <td>-73.529152</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Savia Tienda Restaurante</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>NaN</td>
      <td>5.633287</td>
      <td>-73.524289</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Antique Restaurante Bar</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.634315</td>
      <td>-73.522640</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Frutipicos Restaurante</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.632583</td>
      <td>-73.523247</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Asadero Restaurante Villa Pollo</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.632136</td>
      <td>-73.524646</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Restaurante Rakamandaka</td>
      <td>Winery</td>
      <td>NaN</td>
      <td>5.609699</td>
      <td>-73.563677</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Asadero Restaurante Los Pernilotes</td>
      <td>BBQ Joint</td>
      <td>NaN</td>
      <td>5.631307</td>
      <td>-73.526990</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>al carbon restaurante</td>
      <td>Steakhouse</td>
      <td>Calle 8</td>
      <td>5.634333</td>
      <td>-73.528617</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Restaurante y Piqueteadero Las Vegas</td>
      <td>Cajun / Creole Restaurant</td>
      <td>NaN</td>
      <td>5.620290</td>
      <td>-73.617203</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Restaurante y Piqueteadero Leña y Sabor</td>
      <td>South American Restaurant</td>
      <td>Carrera 2 #5-40</td>
      <td>5.619987</td>
      <td>-73.618721</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>31</th>
      <td>El Tomatino Restaurante Galeria</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.642480</td>
      <td>-73.548108</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Kalahari Restaurante</td>
      <td>Restaurant</td>
      <td>NaN</td>
      <td>5.614637</td>
      <td>-73.558307</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_Restaurant = new_df_restaurant.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
df_Restaurant
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Restaurante  Carnes y Olivas</td>
      <td>Restaurant</td>
      <td>Cr10 No 11-50</td>
      <td>5.633534</td>
      <td>-73.524718</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Restaurante Mama Santa</td>
      <td>French Restaurant</td>
      <td>Carrera 9 13-51</td>
      <td>5.634216</td>
      <td>-73.522599</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Arcadia Bar Restaurante</td>
      <td>Italian Restaurant</td>
      <td>calle 13 9-10</td>
      <td>5.633889</td>
      <td>-73.522909</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Restaurante Tabú Cocina &amp; Arte</td>
      <td>Fast Food Restaurant</td>
      <td>Cra 7 N 7b - 49</td>
      <td>5.629348</td>
      <td>-73.525384</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
  </tbody>
</table>
</div>




```python
neighbourhood_df = pd.concat([df_hotel, df_Restaurant], ignore_index=True)
neighbourhood_df
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Hotel &amp; Spa Getsemaní</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-35</td>
      <td>5.633064</td>
      <td>-73.527990</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hotel Plaza Mayor</td>
      <td>Hotel</td>
      <td>Calle 10 7-31</td>
      <td>5.634088</td>
      <td>-73.524222</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Hotel San Antonio</td>
      <td>Hotel</td>
      <td>Calle 8</td>
      <td>5.632360</td>
      <td>-73.522994</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hotel Mesón de los Virreyes</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634974</td>
      <td>-73.521820</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hotel Y centro de convenciones casa de los fun...</td>
      <td>Hotel</td>
      <td>1km Via Guanani</td>
      <td>5.633522</td>
      <td>-73.524962</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hotel Boutique La Espanola</td>
      <td>Hotel</td>
      <td>Calle 12 No. 11 - 06</td>
      <td>5.634637</td>
      <td>-73.525069</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hotel Plazuela de San Agustín</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634718</td>
      <td>-73.521240</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hotel Aqua Vitae</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.634297</td>
      <td>-73.528244</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Hotel Campanario de la Villa</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.635248</td>
      <td>-73.525080</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Hotel Miracle</td>
      <td>Hotel</td>
      <td>Carrera 11 #9-41</td>
      <td>5.633403</td>
      <td>-73.526484</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hotel Fontana Plaza</td>
      <td>Hotel</td>
      <td>Carrera 9 #9-27</td>
      <td>5.631549</td>
      <td>-73.525750</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hotel Bahia Olivo</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.633185</td>
      <td>-73.527684</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Hotel spa bahía olivo</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.633118</td>
      <td>-73.527534</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Hotel Getzemani</td>
      <td>Hotel</td>
      <td>Calle 8 villa de leyva</td>
      <td>5.632824</td>
      <td>-73.528074</td>
      <td>NaN</td>
      <td>Boyacá Dept</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Hotel Santa Viviana</td>
      <td>Hotel</td>
      <td>Diagonal 12 # 8a-76</td>
      <td>5.634710</td>
      <td>-73.527916</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hotel Andres Venero de Leyva</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.635013</td>
      <td>-73.527551</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>hotel las orquideas de la villa</td>
      <td>Hotel</td>
      <td>Calle 12 # 10-11</td>
      <td>5.636492</td>
      <td>-73.527236</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Hotel Arcadia Colonial</td>
      <td>Hotel</td>
      <td>Calle 8 # 10-19B</td>
      <td>5.634379</td>
      <td>-73.528293</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Hotel Marques De San Luis</td>
      <td>Hotel</td>
      <td>Circunvalacion 8-58</td>
      <td>5.634897</td>
      <td>-73.528138</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>19</th>
      <td>posada de la villa hotel</td>
      <td>Hotel</td>
      <td>Calle 14 carrera 8</td>
      <td>5.633887</td>
      <td>-73.521225</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Hotel Villa Palva</td>
      <td>Hotel</td>
      <td>cra 9a #7 37</td>
      <td>5.630548</td>
      <td>-73.527710</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Hotel Boutique Villa Roma</td>
      <td>Hotel</td>
      <td>Calle 8 #10 -166</td>
      <td>5.634107</td>
      <td>-73.528102</td>
      <td>058</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Molino La Mesopotamia</td>
      <td>Hotel</td>
      <td>NaN</td>
      <td>5.636165</td>
      <td>-73.518837</td>
      <td>NaN</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Restaurante  Carnes y Olivas</td>
      <td>Restaurant</td>
      <td>Cr10 No 11-50</td>
      <td>5.633534</td>
      <td>-73.524718</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Restaurante Mama Santa</td>
      <td>French Restaurant</td>
      <td>Carrera 9 13-51</td>
      <td>5.634216</td>
      <td>-73.522599</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Arcadia Bar Restaurante</td>
      <td>Italian Restaurant</td>
      <td>calle 13 9-10</td>
      <td>5.633889</td>
      <td>-73.522909</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Restaurante Tabú Cocina &amp; Arte</td>
      <td>Fast Food Restaurant</td>
      <td>Cra 7 N 7b - 49</td>
      <td>5.629348</td>
      <td>-73.525384</td>
      <td>154001</td>
      <td>Boyacá</td>
    </tr>
  </tbody>
</table>
</div>




```python


map_villa_de_leyva = folium.Map(location=[latitude, longitude], zoom_start=14)

for lat, lng, name, categories, address in zip(neighbourhood_df['lat'], neighbourhood_df['lng'], 
                                           neighbourhood_df['name'], neighbourhood_df['categories'],\
                                               neighbourhood_df['address']):
    label = '{}, {}'.format(name, address)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        popup=label,
        color='blue',
        fill=True,
        fill_color='blue',
        fill_opacity=0.7,
        parse_html=False).add_to(map_toronto)  
    
map_villa_de_leyva
```


```python

```
