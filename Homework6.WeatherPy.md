

```python
# Dependencies
import csv
import matplotlib.pyplot as plt
import requests
import pandas as pd
import random
from config import gkey
import json
```


```python
#City Datafile
city_data = pd.read_csv('worldcities.csv')
city_data.head()
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
      <th>country</th>
      <th>subcountry</th>
      <th>geonameid</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>les Escaldes</td>
      <td>Andorra</td>
      <td>Escaldes-Engordany</td>
      <td>3040051</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Andorra la Vella</td>
      <td>Andorra</td>
      <td>Andorra la Vella</td>
      <td>3041563</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Umm al Qaywayn</td>
      <td>United Arab Emirates</td>
      <td>Umm al Qaywayn</td>
      <td>290594</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ras al-Khaimah</td>
      <td>United Arab Emirates</td>
      <td>Ra_s al Khaymah</td>
      <td>291074</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Khawr Fakk_n</td>
      <td>United Arab Emirates</td>
      <td>Ash Sh_riqah</td>
      <td>291696</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_data['Max Temp'] = ""
city_data['Humidity'] = ""
city_data['Cloudiness'] = ""
city_data['Wind Speed'] = ""
city_data['Lng'] = ""
city_data['Lat'] = ""

city_data.head(5)
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
      <th>country</th>
      <th>subcountry</th>
      <th>geonameid</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Lng</th>
      <th>Lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>les Escaldes</td>
      <td>Andorra</td>
      <td>Escaldes-Engordany</td>
      <td>3040051</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>Andorra la Vella</td>
      <td>Andorra</td>
      <td>Andorra la Vella</td>
      <td>3041563</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>Umm al Qaywayn</td>
      <td>United Arab Emirates</td>
      <td>Umm al Qaywayn</td>
      <td>290594</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ras al-Khaimah</td>
      <td>United Arab Emirates</td>
      <td>Ra_s al Khaymah</td>
      <td>291074</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>Khawr Fakk_n</td>
      <td>United Arab Emirates</td>
      <td>Ash Sh_riqah</td>
      <td>291696</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
city_sample = city_data.sample(n=550)
city_sample.head(5)
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
      <th>country</th>
      <th>subcountry</th>
      <th>geonameid</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Lng</th>
      <th>Lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20756</th>
      <td>Deerfield</td>
      <td>United States</td>
      <td>Illinois</td>
      <td>4889668</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>22160</th>
      <td>Kuna</td>
      <td>United States</td>
      <td>Idaho</td>
      <td>5597955</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>10490</th>
      <td>Erode</td>
      <td>India</td>
      <td>Tamil Nadu</td>
      <td>1272013</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>19651</th>
      <td>Boryspil’</td>
      <td>Ukraine</td>
      <td>Kiev</td>
      <td>711660</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>20591</th>
      <td>Kingsport</td>
      <td>United States</td>
      <td>Tennessee</td>
      <td>4634662</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
# create a params dict that will be updated with new city each iteration
params = {"key": gkey}

# Loop through the cities_pd and run a lat/long search for each city
for index, row in city_sample.iterrows():
    base_url = "https://maps.googleapis.com/maps/api/geocode/json"

    city = row['name']
    country = row['country']

    # update address key value
    params['address'] = f"{city},{country}"

    # make request, print url
    cities_lat_lng = requests.get(base_url, params=params)
    print(cities_lat_lng.url)
    # convert to json
    cities_lat_lng = cities_lat_lng.json()

    city_sample.loc[index, "Lat"] = cities_lat_lng["results"][0]["geometry"]["location"]["lat"]
    city_sample.loc[index, "Lng"] = cities_lat_lng["results"][0]["geometry"]["location"]["lng"]

# Visualize to confirm lat lng appear
city_sample.head()
```

    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Deerfield%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kuna%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Erode%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Boryspil%E2%80%99%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kingsport%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Concei%C3%A7%C3%A3o+do+Araguaia%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sama%2CSpain
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hesperia%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mungaa%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bah%C3%ADa+Honda%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Baumschulenweg%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mi_r_tah%2CLibya
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Katsuta%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Schaffhausen%2CSwitzerland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Stenl%C3%B8se%2CDenmark
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Highland%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hongjiang%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sangari_%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Landover%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lubliniec%2CPoland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mustaf_b_d%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Manavgat%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=La+Paz%2CArgentina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=S%C3%B6ke%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kashin%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tongshan%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Terme%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Daska%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Chilm_ri%2CBangladesh
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Chino%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Levoberezhnyy%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hadyach%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Carolina%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Paungde%2CMyanmar
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_umperk%2CCzech+Republic
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jilib%2CSomalia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=T%C3%A4by%2CSweden
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Taquari%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=A%C3%AFn+el+Bya%2CAlgeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Overijse%2CBelgium
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jacob_b_d%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Wollongong%2CAustralia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Divin%C3%B3polis%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Freguesia+do+Ribeirao+da+Ilha%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=G_d_r%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=El_r%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Amatitl%C3%A1n%2CGuatemala
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Piton+Saint-Leu%2CReunion
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tipasa%2CAlgeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Huber+Heights%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Helena%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lennestadt%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Satuba%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bulacan%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_iauliai%2CLithuania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Gelsenkirchen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Dunkerque%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Santiago+de+las+Vegas%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Madaoua%2CNiger
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sibs_gar%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Matozinhos%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cienfuegos%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Patz%C3%BAn%2CGuatemala
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Citrus+Heights%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bordeaux%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Surrey%2CCanada
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tokoname%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Nanping%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sahiwal%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cuxhaven%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kilosa%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Wei%C3%9Fensee%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mathba%2CBangladesh
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=R_wah%2CIraq
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hongw_n%2CNorth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=S%C3%A3o+Pedro+da+Cova%2CPortugal
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Marcavelica%2CPeru
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Teoloyucan%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Igara%C3%A7u+do+Tiet%C3%AA%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_ndippatti%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Gajendragarh%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ngara%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ner%C3%B3polis%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Choa+Said_n+Sh_h%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ndib%C3%A8ne+Dahra%2CSenegal
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Fort+Walton+Beach%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=S%C3%A3o+Mateus+do+Sul%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Orosh%C3%A1za%2CHungary
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Khartsyz%E2%80%99k%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sunnybank%2CAustralia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ettlingen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kov_r%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Art%C3%ABmovskiy%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Floirac%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Villiers-le-Bel%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Brits%2CSouth+Africa
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Port-au-Prince%2CHaiti
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Guajar%C3%A1+Mirim%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Burla%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ikirun%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Saint-J%C3%A9r%C3%B4me%2CCanada
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kaulsdorf%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Zverevo%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Penja%2CCameroon
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Zgorzelec%2CPoland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ilebo%2CDemocratic+Republic+of+the+Congo
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Auxerre%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_mol%2CIran
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ak%C3%A7akoca%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=P_tan%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bentonville%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Glan%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Yegor%E2%80%99yevsk%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Acworth%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pandak%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ipinda%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Changping%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ipubi%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Stoke-on-Trent%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Belton%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lebedyn%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Aqaba%2CJordan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Viseu%2CPortugal
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Azrou%2CMorocco
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Berkeley%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Farrukhnagar%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_esk%C3%A9+Bud_jovice%2CCzech+Republic
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Banjar%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lower+Earley%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bautzen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Wakimachi%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bloomington%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Malazgirt%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Khal_bat%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ponte+Vedra+Beach%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Memmingen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Itatiba%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Centralia%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Giulianova%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=City+of+Isabela%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tom%C3%A9+A%C3%A7u%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Vanino%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bridgewater%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Marseille+14%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lumberton%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Binzhou%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Katsuura%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=K_thor%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Neyy_ttinkara%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shiqiao%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ciamis%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sahaspur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sosnovoborsk%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Acatl%C3%A1n+de+Osorio%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sioux+Falls%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hualmay%2CPeru
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sargodha%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Litv%C3%ADnov%2CCzech+Republic
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cassino%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bijapur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ulsan%2CSouth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Gunt_r%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Porta+Westfalica%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kilwinning%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Heze%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Dan+Khun+Thot%2CThailand
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hoffman+Estates%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rocca+di+Papa%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_hm_db_yli%2CAzerbaijan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Srinagar%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Aci+Castello%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=M_velikara%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=South+Laurel%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mal_yer%2CIran
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cochabamba%2CBolivia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Las+Animas%2CChile
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Malu%C3%B1gun%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Truro%2CCanada
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Brejo+da+Madre+de+Deus%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Iwade%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Essaouira%2CMorocco
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=La+Romana%2CDominican+Republic
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pindi+Bhatti_n%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Grove+City%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Michigan+City%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bensheim%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=N_dbai%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=S%C3%A3o+Jo%C3%A3o+Nepomuceno%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=H_t%2CIraq
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Saltillo%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Minas+de+Matahambre%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lahore%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Curridabat%2CCosta+Rica
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Netanya%2CIsrael
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Paducah%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Paphos%2CCyprus
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kashipur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Beaufort+West%2CSouth+Africa
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Prokuplje%2CSerbia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Montigny-l%C3%A8s-Cormeilles%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Vrangel%E2%80%99%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Blackburn%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lubu%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Celle%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Marina%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bharw_ri%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Clermont-Ferrand%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Port-de-Bouc%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jalai+Nur%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Yangquan%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Leonardo%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=B_l_gh_t%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rimini%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Sebasti%C3%A1n+de+los+Reyes%2CSpain
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Parque+Industrial+Ciudad+Mitras%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hicksville%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jhalida%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Longfeng%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bekobod%2CUzbekistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Collierville%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Chicago%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Stains%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tejar%2CCosta+Rica
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jammalamadugu%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Guihul%C3%B1gan%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ardon%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Guaran%C3%A9sia%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tienen%2CBelgium
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sarandi%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rho%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Irvine%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ceres%2CSouth+Africa
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=P%C3%A1jara%2CSpain
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Newburgh%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kharagpur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sibulan%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mutare%2CZimbabwe
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Reuleuet%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_______%2CMacedonia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Abreus%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ajra%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sinanju%2CNorth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Murray+Bridge%2CAustralia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Aghsu%2CAzerbaijan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ibi%C3%BAna%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cardenas%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Megarine%2CAlgeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kirishi%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Khadki%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tyumen%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Daura%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shakhtars%E2%80%99k%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Monrovia%2CLiberia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Guarulhos%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Neyagawa%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lianran%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jaragu%C3%A1%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sult_npur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Altea%2CSpain
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mthatha%2CSouth+Africa
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ksar+Chellala%2CAlgeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Crist%C3%B3bal%2CVenezuela
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ban+Mai%2CThailand
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Juan+Jose+Rios%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Teut%C3%B4nia%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Banfora%2CBurkina+Faso
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Gennevilliers%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Franklin+Square%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Edremit%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Yingkou%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=La+Libertad%2CEcuador
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Barking%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Buchholz+in+der+Nordheide%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Aubagne%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Augusta%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=R_zekne%2CLatvia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Atarra%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sungai+Udang%2CMalaysia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Y_nggwang-_p%2CNorth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Westerlo%2CBelgium
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Silao%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=_%C3%A0+L_t%2CVietnam
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mgandu%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Oued+Fodda%2CAlgeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Saskatoon%2CCanada
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ada%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jal_l_bad%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pameungpeuk%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hwacheon%2CSouth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mavoor%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ampara%2CSri+Lanka
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Severna+Park%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Asahikawa%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bogovinje%2CMacedonia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Al_garh%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Heusden%2CBelgium
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lincheng%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Terek%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Francisco%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Irec%C3%AA%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Afikpo%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Humait%C3%A1%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Erkelenz%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shahecheng%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tara%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Guatemala+City%2CGuatemala
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hwaseong-si%2CSouth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bellevue%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Dokuchayevs%E2%80%99k%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Murakami%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Barbosa%2CColombia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Griffin%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Chetumal%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ivrea%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Er_ttupetta%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bieru_%2CPoland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Northbrook%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Polavaram%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Howard%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Taozhuang%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Changwon%2CSouth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bagheria%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Izk_%2COman
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tosu%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Qom%2CIran
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pentecoste%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=N%E2%80%99dalatando%2CAngola
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lisakovsk%2CKazakhstan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Santyoku%2CSouth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Serpukhov%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Pedro%2CArgentina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Svalyava%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jal_l_%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Z%C3%BCrich+%28Kreis+10%29+%2F+H%C3%B6ngg%2CSwitzerland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Geyve%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Coelho+Neto%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ifakara%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Argentan%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Nanzhou%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Melipilla%2CChile
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Koup%C3%A9la%2CBurkina+Faso
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Amet%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ibaraki%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=S%C3%A3o+Jos%C3%A9+do+Egito%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ko_skie%2CPoland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hermosillo%2CMexico
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bal%C4%B1kesir%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Huicheng%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Westhoughton%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kumbo%2CCameroon
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Beykonak%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Krasnoufimsk%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=T%C3%B6r%C3%B6kszentmikl%C3%B3s%2CHungary
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bhand_ra%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Florissant%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Calatagan%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Gongdanglegi+Kulon%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Butw_l%2CNepal
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Korbach%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Apalit%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Al%C3%A8s%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Le+Hochet%2CMauritius
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Vratsa%2CBulgaria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Netphen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Harda+Kh_s%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sarny%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shams_b_d%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sovetskaya+Gavan%E2%80%99%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Palermo%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Holb%C3%A6k%2CDenmark
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=B_sudebpur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Vera+Cruz%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=A_dam%2CAzerbaijan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Manicaragua%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bibiani%2CGhana
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Vigneux-sur-Seine%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Modakeke%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Deer+Park%2CAustralia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hamme%2CBelgium
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Santa+Cruz+das+Palmeiras%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=g%C3%BCng%C3%B6ren+merter%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mount+Vernon%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Krestovskiy+ostrov%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bullhead+City%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mitte%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tata%2CHungary
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Linqu%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Halifax%2CCanada
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bornheim%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=West+Vancouver%2CCanada
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ijero-Ekiti%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=T_kamachi%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Banjar%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Felice+A+Cancello%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Prost_jov%2CCzech+Republic
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Qinhuangdao%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Searcy%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Yuen+Long+Kau+Hui%2CHong+Kong
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Central+Islip%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mount+Juliet%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kabarnet%2CKenya
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=A%C3%ADgio%2CGreece
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=P_nchla%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Semiluki%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Oss%2CNetherlands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Nijmegen%2CNetherlands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Middletown%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Westend%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rizhao%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Le%C3%B3n%2CSpain
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kabanovo%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Udg_r%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mandi%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Velenje%2CSlovenia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mikumi%2CTanzania
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Las+Tejer%C3%ADas%2CVenezuela
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=York%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Fort+Dauphin%2CMadagascar
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=N_han%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Es+Senia%2CAlgeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Dutsen+Wai%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sambri_l%2CPakistan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Plymouth%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Iwatsuki%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pita+Kotte%2CSri+Lanka
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Crema%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Manapparai%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Afzalpur%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Brusciano%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ferndale%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rumoi%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Jovellanos%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Slonim%2CBelarus
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=San+Ramon%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=East+Lake-Orient+Park%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Montcada+i+Reixac%2CSpain
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Abay%2CKazakhstan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sepang%2CMalaysia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Itako%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Al+Ain%2CUnited+Arab+Emirates
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Weston-super-Mare%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Marseille+03%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Diksmuide%2CBelgium
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Travnik%2CBosnia+and+Herzegovina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Long+Branch%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Santa+Clara%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shepetivka%2CUkraine
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Fraijanes%2CGuatemala
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Noto%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=%E2%80%98Abas_n+al+Kab_rah%2CPalestinian+Territory
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Owase%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Choloma%2CHonduras
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Saint+Croix%2CU.S.+Virgin+Islands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Esperanza%2CArgentina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sassari%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cirebon%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ujjain%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Belalcazar%2CColombia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bobingen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Derry+Village%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bergen+op+Zoom%2CNetherlands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Euclides+da+Cunha%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kishorganj%2CBangladesh
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cava+D%C3%A8+Tirreni%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lohja%2CFinland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ramon%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ives+Estates%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Z%C3%BCrich+%28Kreis+10%29%2CSwitzerland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Freiberg%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=%C3%93zd%2CHungary
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Vacaria%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Salamina%2CColombia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Iselin%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Morrinhos%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Battaramulla+South%2CSri+Lanka
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Przemy_l%2CPoland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Washington%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Caxias+do+Sul%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=R%C3%B8dovre%2CDenmark
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shakopee%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=%C3%96stersund%2CSweden
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ihiala%2CNigeria
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Zwickau%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pavlovsk%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Gusev%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Carrizal%2CVenezuela
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Orenburg%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ridderkerk%2CNetherlands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Yeniseysk%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Nagykanizsa%2CHungary
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Thanh+H%C3%B3a%2CVietnam
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Badulla%2CSri+Lanka
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Malkajgiri%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bang+Mun+Nak%2CThailand
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hakodate%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Roissy-en-Brie%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bellshill%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Datteln%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Pawtucket%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rotorua%2CNew+Zealand
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rijswijk%2CNetherlands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Zaxo%2CIraq
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Saint-Herblain%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Sestroretsk%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Renningen%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Lomonosov%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mouila%2CGabon
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ulan-Ude%2CRussia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Wabu%2CSouth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Erding%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Havl%C3%AD_k_v+Brod%2CCzech+Republic
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tarsus%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Huanggang%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Nipomo%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shivaji+Nagar%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=G_darw_ra%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=B_li%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Rimavsk%C3%A1+Sobota%2CSlovakia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Hemet%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Anju%2CNorth+Korea
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Newark%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Tokat%2CTurkey
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=G_do%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Kannad%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=La%C3%AF%2CChad
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Luohe%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mazara+del+Vallo%2CItaly
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Chengde%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=%E2%80%98Ar%E2%80%98ar%2CSaudi+Arabia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Venray%2CNetherlands
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Quesada%2CCosta+Rica
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Londrina%2CBrazil
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Mataram%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ghugus%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ulu+Tiram%2CMalaysia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Batang%2CIndonesia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shiraguppi%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Montauban%2CFrance
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Strzelce+Opolskie%2CPoland
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=J_etsu%2CJapan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Arcot%2CIndia
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Shangmei%2CChina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Balayan%2CPhilippines
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Villa+Ocampo%2CArgentina
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Bor%2CSouth+Sudan
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cleveland%2CUnited+States
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Duisburg%2CGermany
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Ramsbottom%2CUnited+Kingdom
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Cerro%2CCuba
    https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyBBapS7TvNlEgYNPZ9NYz01bBFwTuYEseE&address=Desenzano+del+Garda%2CItaly





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
      <th>country</th>
      <th>subcountry</th>
      <th>geonameid</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Lng</th>
      <th>Lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20756</th>
      <td>Deerfield</td>
      <td>United States</td>
      <td>Illinois</td>
      <td>4889668</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>-87.8445</td>
      <td>42.1711</td>
    </tr>
    <tr>
      <th>22160</th>
      <td>Kuna</td>
      <td>United States</td>
      <td>Idaho</td>
      <td>5597955</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>-116.42</td>
      <td>43.4918</td>
    </tr>
    <tr>
      <th>10490</th>
      <td>Erode</td>
      <td>India</td>
      <td>Tamil Nadu</td>
      <td>1272013</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>77.7172</td>
      <td>11.341</td>
    </tr>
    <tr>
      <th>19651</th>
      <td>Boryspil’</td>
      <td>Ukraine</td>
      <td>Kiev</td>
      <td>711660</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>30.9562</td>
      <td>50.3482</td>
    </tr>
    <tr>
      <th>20591</th>
      <td>Kingsport</td>
      <td>United States</td>
      <td>Tennessee</td>
      <td>4634662</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>-82.5618</td>
      <td>36.5484</td>
    </tr>
  </tbody>
</table>
</div>




```python
cities=city_sample["name"]
print(len(cities))
print(cities)
```

    550
    20756                Deerfield
    22160                     Kuna
    10490                    Erode
    19651                Boryspil’
    20591                Kingsport
    1305     Conceição do Araguaia
    6267                      Sama
    21837                 Hesperia
    19293                   Mungaa
    4174               Bahía Honda
    5234            Baumschulenweg
    13401                 Mi_r_tah
    12741                  Katsuta
    2717              Schaffhausen
    5368                  Stenløse
    21838                 Highland
    3450                 Hongjiang
    9206                  Sangari_
    20263                 Landover
    16368                Lubliniec
    15935               Mustaf_b_d
    18902                 Manavgat
    167                     La Paz
    18855                     Söke
    17442                   Kashin
    3109                  Tongshan
    19067                    Terme
    16086                    Daska
    700                   Chilm_ri
    12624                    Chino
                     ...          
    21248                   Newark
    19065                    Tokat
    12595                     G_do
    10115                   Kannad
    18444                      Laï
    3331                     Luohe
    11544         Mazara del Vallo
    3767                   Chengde
    18003                   ‘Ar‘ar
    14886                   Venray
    4040                   Quesada
    1814                  Londrina
    8504                   Mataram
    10420                   Ghugus
    14472                Ulu Tiram
    8640                    Batang
    11221               Shiraguppi
    6876                 Montauban
    16305        Strzelce Opolskie
    12837                   J_etsu
    11037                    Arcot
    3132                  Shangmei
    15788                  Balayan
    130               Villa Ocampo
    18314                      Bor
    21453                Cleveland
    5116                  Duisburg
    7419                Ramsbottom
    4157                     Cerro
    11906      Desenzano del Garda
    Name: name, Length: 550, dtype: object



```python
# Save config information.
import openweathermapy.core as owm
from api_key import api_key
```


```python

for index, row in city_sample.iterrows():
    url = "http://api.openweathermap.org/data/2.5/weather?"
    units = "imperial"

    # Build partial query URL
    query_url = f"{url}appid={api_key}&units={units}&q="
    #api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}
    city = row['name']
    try:
    # make request, print url
        cities_weather = requests.get(query_url + city)
        print(cities_weather.url)
    # convert to json
        cities_weather = cities_weather.json()

        city_sample.loc[index, "Max Temp"] = (cities_weather["main"]["temp_max"])
        city_sample.loc[index, "Humidity"] = (cities_weather["main"]["humidity"])
        city_sample.loc[index, "Cloudiness"] = (cities_weather["clouds"]["all"])
        city_sample.loc[index, "Wind Speed"] = (cities_weather["wind"]["speed"])

    except KeyError:
        print("City Not Found")
    
# Visualize to confirm lat lng appear
city_sample.head()

```

    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Deerfield
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kuna
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Erode
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Boryspil%E2%80%99
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kingsport
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Concei%C3%A7%C3%A3o%20do%20Araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sama
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hesperia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mungaa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bah%C3%ADa%20Honda
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Baumschulenweg
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mi_r_tah
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Katsuta
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Schaffhausen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Stenl%C3%B8se
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Highland
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hongjiang
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sangari_
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Landover
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lubliniec
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mustaf_b_d
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Manavgat
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=La%20Paz
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=S%C3%B6ke
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kashin
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tongshan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Terme
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Daska
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Chilm_ri
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Chino
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Levoberezhnyy
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hadyach
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Carolina
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Paungde
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_umperk
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jilib
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=T%C3%A4by
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Taquari
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=A%C3%AFn%20el%20Bya
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Overijse
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jacob_b_d
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Wollongong
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Divin%C3%B3polis
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Freguesia%20do%20Ribeirao%20da%20Ilha
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=G_d_r
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=El_r
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Amatitl%C3%A1n
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Piton%20Saint-Leu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tipasa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Huber%20Heights
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Helena
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lennestadt
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Satuba
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bulacan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_iauliai
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Gelsenkirchen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Dunkerque
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Santiago%20de%20las%20Vegas
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Madaoua
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sibs_gar
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Matozinhos
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cienfuegos
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Patz%C3%BAn
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Citrus%20Heights
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bordeaux
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Surrey
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tokoname
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Nanping
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sahiwal
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cuxhaven
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kilosa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Wei%C3%9Fensee
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mathba
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=R_wah
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hongw_n
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=S%C3%A3o%20Pedro%20da%20Cova
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Marcavelica
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Teoloyucan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Igara%C3%A7u%20do%20Tiet%C3%AA
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_ndippatti
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Gajendragarh
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ngara
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ner%C3%B3polis
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Choa%20Said_n%20Sh_h
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ndib%C3%A8ne%20Dahra
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Fort%20Walton%20Beach
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=S%C3%A3o%20Mateus%20do%20Sul
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Orosh%C3%A1za
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Khartsyz%E2%80%99k
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sunnybank
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ettlingen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kov_r
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Art%C3%ABmovskiy
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Floirac
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Villiers-le-Bel
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Brits
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Port-au-Prince
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Guajar%C3%A1%20Mirim
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Burla
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ikirun
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Saint-J%C3%A9r%C3%B4me
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kaulsdorf
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Zverevo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Penja
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Zgorzelec
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ilebo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Auxerre
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_mol
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ak%C3%A7akoca
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=P_tan
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bentonville
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Glan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Yegor%E2%80%99yevsk
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Acworth
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pandak
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ipinda
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Changping
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ipubi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Stoke-on-Trent
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Belton
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lebedyn
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Aqaba
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Viseu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Azrou
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Berkeley
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Farrukhnagar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_esk%C3%A9%20Bud_jovice
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Banjar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lower%20Earley
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bautzen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Wakimachi
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bloomington
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Malazgirt
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Khal_bat
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ponte%20Vedra%20Beach
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Memmingen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Itatiba
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Centralia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Giulianova
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=City%20of%20Isabela
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tom%C3%A9%20A%C3%A7u
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Vanino
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bridgewater
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Marseille%2014
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lumberton
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Binzhou
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=K_thor
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Neyy_ttinkara
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shiqiao
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ciamis
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sahaspur
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sosnovoborsk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Acatl%C3%A1n%20de%20Osorio
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sioux%20Falls
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sargodha
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Litv%C3%ADnov
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cassino
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bijapur
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ulsan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Gunt_r
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Porta%20Westfalica
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kilwinning
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Heze
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Dan%20Khun%20Thot
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hoffman%20Estates
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rocca%20di%20Papa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_hm_db_yli
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Srinagar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Aci%20Castello
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=M_velikara
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=South%20Laurel
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mal_yer
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cochabamba
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Las%20Animas
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Malu%C3%B1gun
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Truro
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Brejo%20da%20Madre%20de%20Deus
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Iwade
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Essaouira
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=La%20Romana
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pindi%20Bhatti_n
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Grove%20City
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Michigan%20City
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bensheim
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=N_dbai
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=S%C3%A3o%20Jo%C3%A3o%20Nepomuceno
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=H_t
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Saltillo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Minas%20de%20Matahambre
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lahore
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Curridabat
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Netanya
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Paducah
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Paphos
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kashipur
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Beaufort%20West
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Prokuplje
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Montigny-l%C3%A8s-Cormeilles
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Vrangel%E2%80%99
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Blackburn
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lubu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Celle
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Marina
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bharw_ri
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Clermont-Ferrand
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Port-de-Bouc
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jalai%20Nur
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Yangquan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Leonardo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=B_l_gh_t
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rimini
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Sebasti%C3%A1n%20de%20los%20Reyes
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Parque%20Industrial%20Ciudad%20Mitras
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hicksville
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jhalida
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Longfeng
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bekobod
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Collierville
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Chicago
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Stains
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tejar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jammalamadugu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Guihul%C3%B1gan
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ardon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Guaran%C3%A9sia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tienen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sarandi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rho
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Irvine
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ceres
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=P%C3%A1jara
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Newburgh
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kharagpur
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sibulan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mutare
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Reuleuet
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_______
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Abreus
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ajra
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sinanju
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Murray%20Bridge
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Aghsu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ibi%C3%BAna
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cardenas
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Megarine
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kirishi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Khadki
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tyumen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Daura
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shakhtars%E2%80%99k
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Monrovia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Guarulhos
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Neyagawa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lianran
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jaragu%C3%A1
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sult_npur
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Altea
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mthatha
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ksar%20Chellala
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Crist%C3%B3bal
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ban%20Mai
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Juan%20Jose%20Rios
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Teut%C3%B4nia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Banfora
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Gennevilliers
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Franklin%20Square
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Edremit
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Yingkou
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=La%20Libertad
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Barking
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Buchholz%20in%20der%20Nordheide
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Aubagne
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Augusta
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=R_zekne
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Atarra
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sungai%20Udang
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Y_nggwang-_p
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Westerlo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Silao
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=_%C3%A0%20L_t
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mgandu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Oued%20Fodda
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Saskatoon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ada
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jal_l_bad
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pameungpeuk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hwacheon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mavoor
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ampara
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Severna%20Park
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Asahikawa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bogovinje
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Al_garh
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Heusden
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lincheng
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Terek
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Francisco
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Irec%C3%AA
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Afikpo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Humait%C3%A1
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Erkelenz
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shahecheng
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tara
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Guatemala%20City
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hwaseong-si
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bellevue
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Dokuchayevs%E2%80%99k
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Murakami
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Barbosa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Griffin
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Chetumal
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ivrea
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Er_ttupetta
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bieru_
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Northbrook
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Polavaram
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Howard
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Taozhuang
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Changwon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bagheria
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Izk_
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tosu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Qom
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pentecoste
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=N%E2%80%99dalatando
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lisakovsk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Santyoku
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Serpukhov
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Pedro
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Svalyava
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jal_l_
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Z%C3%BCrich%20(Kreis%2010)%20/%20H%C3%B6ngg
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Geyve
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Coelho%20Neto
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ifakara
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Argentan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Nanzhou
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Melipilla
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Koup%C3%A9la
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Amet
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ibaraki
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=S%C3%A3o%20Jos%C3%A9%20do%20Egito
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ko_skie
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hermosillo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bal%C4%B1kesir
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Huicheng
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Westhoughton
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kumbo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Beykonak
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Krasnoufimsk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=T%C3%B6r%C3%B6kszentmikl%C3%B3s
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bhand_ra
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Florissant
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Calatagan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Gongdanglegi%20Kulon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Butw_l
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Korbach
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Apalit
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Al%C3%A8s
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Le%20Hochet
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Vratsa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Netphen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Harda%20Kh_s
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sarny
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shams_b_d
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sovetskaya%20Gavan%E2%80%99
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Palermo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Holb%C3%A6k
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=B_sudebpur
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Vera%20Cruz
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=A_dam
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Manicaragua
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bibiani
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Vigneux-sur-Seine
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Modakeke
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Deer%20Park
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hamme
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Santa%20Cruz%20das%20Palmeiras
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=g%C3%BCng%C3%B6ren%20merter
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mount%20Vernon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Krestovskiy%20ostrov
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bullhead%20City
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mitte
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tata
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Linqu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Halifax
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bornheim
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=West%20Vancouver
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ijero-Ekiti
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=T_kamachi
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Banjar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Felice%20A%20Cancello
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Prost_jov
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Qinhuangdao
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Searcy
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Yuen%20Long%20Kau%20Hui
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Central%20Islip
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mount%20Juliet
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kabarnet
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=A%C3%ADgio
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=P_nchla
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Semiluki
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Oss
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Nijmegen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Middletown
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Westend
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rizhao
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Le%C3%B3n
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kabanovo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Udg_r
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mandi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Velenje
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mikumi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Las%20Tejer%C3%ADas
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=York
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Fort%20Dauphin
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=N_han
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Es%20Senia
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Dutsen%20Wai
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sambri_l
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Plymouth
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Iwatsuki
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pita%20Kotte
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Crema
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Manapparai
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Afzalpur
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Brusciano
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ferndale
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rumoi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Jovellanos
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Slonim
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=San%20Ramon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=East%20Lake-Orient%20Park
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Montcada%20i%20Reixac
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Abay
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sepang
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Itako
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Al%20Ain
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Weston-super-Mare
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Marseille%2003
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Diksmuide
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Travnik
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Long%20Branch
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Santa%20Clara
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shepetivka
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Fraijanes
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Noto
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=%E2%80%98Abas_n%20al%20Kab_rah
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Owase
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Choloma
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Saint%20Croix
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Esperanza
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sassari
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cirebon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ujjain
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Belalcazar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bobingen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Derry%20Village
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bergen%20op%20Zoom
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Euclides%20da%20Cunha
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kishorganj
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cava%20D%C3%A8%20Tirreni
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lohja
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ramon
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ives%20Estates
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Z%C3%BCrich%20(Kreis%2010)
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Freiberg
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=%C3%93zd
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Vacaria
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Salamina
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Iselin
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Morrinhos
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Battaramulla%20South
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Przemy_l
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Washington
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Caxias%20do%20Sul
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=R%C3%B8dovre
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shakopee
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=%C3%96stersund
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ihiala
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Zwickau
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pavlovsk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Gusev
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Carrizal
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Orenburg
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ridderkerk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Yeniseysk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Nagykanizsa
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Thanh%20H%C3%B3a
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Badulla
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Malkajgiri
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bang%20Mun%20Nak
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hakodate
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Roissy-en-Brie
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bellshill
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Datteln
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Pawtucket
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rotorua
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rijswijk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Zaxo
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Saint-Herblain
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Sestroretsk
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Renningen
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Lomonosov
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mouila
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ulan-Ude
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Wabu
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Erding
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Havl%C3%AD_k_v%20Brod
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tarsus
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Huanggang
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Nipomo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shivaji%20Nagar
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=G_darw_ra
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=B_li
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Rimavsk%C3%A1%20Sobota
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Hemet
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Anju
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Newark
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Tokat
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=G_do
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Kannad
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=La%C3%AF
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Luohe
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mazara%20del%20Vallo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Chengde
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=%E2%80%98Ar%E2%80%98ar
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Venray
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Quesada
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Londrina
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Mataram
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ghugus
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ulu%20Tiram
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Batang
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shiraguppi
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Montauban
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Strzelce%20Opolskie
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=J_etsu
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Arcot
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Shangmei
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Balayan
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Villa%20Ocampo
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Bor
    City Not Found
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cleveland
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Duisburg
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Ramsbottom
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Cerro
    http://api.openweathermap.org/data/2.5/weather?appid=f2ae3a995d22eb9f243319df1b9b2057&units=imperial&q=Desenzano%20del%20Garda





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
      <th>country</th>
      <th>subcountry</th>
      <th>geonameid</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Lng</th>
      <th>Lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20756</th>
      <td>Deerfield</td>
      <td>United States</td>
      <td>Illinois</td>
      <td>4889668</td>
      <td>64.4</td>
      <td>68</td>
      <td>1</td>
      <td>6.93</td>
      <td>-87.8445</td>
      <td>42.1711</td>
    </tr>
    <tr>
      <th>22160</th>
      <td>Kuna</td>
      <td>United States</td>
      <td>Idaho</td>
      <td>5597955</td>
      <td>75.2</td>
      <td>46</td>
      <td>40</td>
      <td>11.41</td>
      <td>-116.42</td>
      <td>43.4918</td>
    </tr>
    <tr>
      <th>10490</th>
      <td>Erode</td>
      <td>India</td>
      <td>Tamil Nadu</td>
      <td>1272013</td>
      <td>70.84</td>
      <td>97</td>
      <td>24</td>
      <td>2.71</td>
      <td>77.7172</td>
      <td>11.341</td>
    </tr>
    <tr>
      <th>19651</th>
      <td>Boryspil’</td>
      <td>Ukraine</td>
      <td>Kiev</td>
      <td>711660</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>30.9562</td>
      <td>50.3482</td>
    </tr>
    <tr>
      <th>20591</th>
      <td>Kingsport</td>
      <td>United States</td>
      <td>Tennessee</td>
      <td>4634662</td>
      <td>78.8</td>
      <td>78</td>
      <td>90</td>
      <td>2.71</td>
      <td>-82.5618</td>
      <td>36.5484</td>
    </tr>
  </tbody>
</table>
</div>




```python
import numpy as np
city_sample = city_sample.replace(['\[\],','\[\[\]\]', ''],['','', np.nan], regex=True)
city_sample = city_sample.dropna()
city_sample.head(5)
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
      <th>country</th>
      <th>subcountry</th>
      <th>geonameid</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Lng</th>
      <th>Lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20756</th>
      <td>Deerfield</td>
      <td>United States</td>
      <td>Illinois</td>
      <td>4889668</td>
      <td>64.40</td>
      <td>68.0</td>
      <td>1.0</td>
      <td>6.93</td>
      <td>-87.844512</td>
      <td>42.171137</td>
    </tr>
    <tr>
      <th>22160</th>
      <td>Kuna</td>
      <td>United States</td>
      <td>Idaho</td>
      <td>5597955</td>
      <td>75.20</td>
      <td>46.0</td>
      <td>40.0</td>
      <td>11.41</td>
      <td>-116.420122</td>
      <td>43.491831</td>
    </tr>
    <tr>
      <th>10490</th>
      <td>Erode</td>
      <td>India</td>
      <td>Tamil Nadu</td>
      <td>1272013</td>
      <td>70.84</td>
      <td>97.0</td>
      <td>24.0</td>
      <td>2.71</td>
      <td>77.717164</td>
      <td>11.341036</td>
    </tr>
    <tr>
      <th>20591</th>
      <td>Kingsport</td>
      <td>United States</td>
      <td>Tennessee</td>
      <td>4634662</td>
      <td>78.80</td>
      <td>78.0</td>
      <td>90.0</td>
      <td>2.71</td>
      <td>-82.561819</td>
      <td>36.548434</td>
    </tr>
    <tr>
      <th>1305</th>
      <td>Conceição do Araguaia</td>
      <td>Brazil</td>
      <td>Pará</td>
      <td>3401845</td>
      <td>78.04</td>
      <td>70.0</td>
      <td>8.0</td>
      <td>3.94</td>
      <td>-49.446601</td>
      <td>-8.177433</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Max Temperature (F) vs. Latitude
y = city_sample["Max Temp"]
x = city_sample["Lat"]
```


```python
plt.scatter(x, y, linewidths=1, color='b', alpha=0.75, label="Max Temp vs Lat")
plt.title ('Max Temperature (F) vs. Latitude')
plt.xlabel('Latitude')
plt.ylabel('Max Temp')
plt.xlim([-80, 100])
plt.grid()
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_10_1.png)



```python
# Humidity (%) vs. Latitude
z = city_sample["Humidity"]
x = city_sample["Lat"]

plt.scatter(z, y, linewidths=1, color='b', alpha=0.75, label="Max Temp vs Lat")
plt.title ('Humidity" vs. Latitude')
plt.xlabel('Latitude')
plt.ylabel('Humidity')
plt.xlim([-80, 100])
plt.grid()
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_11_1.png)



```python
# Cloudiness (%) vs. Latitude
w = city_sample["Cloudiness"]
x = city_sample["Lat"]

plt.scatter(w, y, linewidths=1, color='b', alpha=0.75, label="Max Temp vs Lat")
plt.title ('Cloudiness (%) vs. Latitude')
plt.xlabel('Latitude')
plt.ylabel('Cloudiness')
plt.xlim([-80, 100])
plt.grid()
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_12_1.png)



```python
# Wind Speed (mph) vs. Latitude
w = city_sample["Wind Speed"]
x = city_sample["Lat"]

plt.scatter(w, y, linewidths=1, color='b', alpha=0.75, label="Max Temp vs Lat")
plt.title ('Wind Speed vs. Latitude')
plt.xlabel('Latitude')
plt.ylabel('Wind Speed (mph)')
plt.xlim([-10, 30])
plt.grid()
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_13_1.png)

