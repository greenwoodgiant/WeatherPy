
# WeatherPy

ANALYSIS:

- There is a clear concentration of high temperatures around the equator, but what's interesting is you don't see the same drop in temperature going south as you do going north. -40 degree south only shows a slight drop where at the 40 mark you see a drastic drop in temperature. This is likely due to the tilt in the Earth's axis, which must be pointing the southern pole towards the Sun.

- There does not appear to be a strong correlation with latitude and humidity, as cities of any latitude currently have 100% humidity, but there is a slight indication that the southern hemisphere is more likely to be humid as there are fewer cities reporting low humidities.

- There does not appear to be a strong correlation for Cloudiness or Wind Speed, either, as the marks for cloudiness are fairly evenly distributed across the map (clearly the data is recorded in increments of five, causing the straight lines across the graph), and aside from a dozen or so outliers with high Wind Speeds, the cities all occupy the same general range.


```python
import matplotlib.pyplot as plt
import numpy as np
from citipy import citipy
import requests as req
from random import shuffle
import pandas as pd
import json
```


```python
# Create empty lists for appending cities and their country codes
cities = []
countries = []

# Create a list of 850 evenly-spaced latitudes & longitudes
# using approximate boundaries of landmasses (-40:75 deg lat, -120:150 deg lng)
# (otherwise, results skew towards coastal areas)
lat = np.arange(-40,75,.135)
lng = np.arange(-120,150,.317)

# Shuffle the lists to randomize order
shuffle(lat)
shuffle(lng)

# For each index in the list
for x in range(850):

    # Find the nearest city to those points
    city = citipy.nearest_city(lat[x-1],lng[x-1])
    # Add the city and country to the empty lists
    cities.append(city.city_name)
    countries.append(city.country_code)

# Create a dataframe from the result
city_df = pd.DataFrame({"City":cities,"Country":countries})

# Check Top 5 countries to see if it makes sense per geographical area
print(city_df["Country"].value_counts().head())
```

    ru    80
    ca    61
    br    52
    au    44
    cn    32
    Name: Country, dtype: int64
    


```python
key = "&APPID=aa4d872bb2292428aae8893e04007714"
url = "http://api.openweathermap.org/data/2.5/weather?q="
```


```python
# Create empty lists to append called weather data
found_cities = []
city_ids = []
found_countries = []
found_lats = []
temps = []
humids = []
clouds = []
winds = []

# For each index in Cities / Countries:
for x in range(850):
    
    try:

        # Get the current weather data
        city_url = "{0}{1},{2}{3}".format(url,cities[x-1],countries[x-1],key)
        weather = req.get(city_url,timeout=5).json()
        
        # Append the relevant data to the created lists
        found_cities.append(weather["name"])
        city_ids.append(weather["id"])
        found_countries.append(weather["sys"]["country"])
        found_lats.append(weather["coord"]["lat"])
        temps.append(weather["main"]["temp"] * (9/5) - 459.67)
        humids.append(weather["main"]["humidity"])
        clouds.append(weather["clouds"]["all"])
        winds.append((weather["wind"]["speed"] * 3600)/1609.34)
        
        # Print out the city, ID, and url for a record
        print(weather["name"]," ",weather["id"])
        print(url)
        
    except KeyError:
        pass
```

    Kavaratti   1267390
    http://api.openweathermap.org/data/2.5/weather?q=
    Xuddur   49747
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Georgetown   2411397
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Okhotsk   2122605
    http://api.openweathermap.org/data/2.5/weather?q=
    Olinda   3456160
    http://api.openweathermap.org/data/2.5/weather?q=
    Fomboni   921889
    http://api.openweathermap.org/data/2.5/weather?q=
    Shimoda   1852357
    http://api.openweathermap.org/data/2.5/weather?q=
    Constitucion   3893726
    http://api.openweathermap.org/data/2.5/weather?q=
    Alyangula   2079582
    http://api.openweathermap.org/data/2.5/weather?q=
    Banda Aceh   1215502
    http://api.openweathermap.org/data/2.5/weather?q=
    Tasiilaq   3424607
    http://api.openweathermap.org/data/2.5/weather?q=
    Namibe   3347019
    http://api.openweathermap.org/data/2.5/weather?q=
    Shklo   693995
    http://api.openweathermap.org/data/2.5/weather?q=
    Grindavik   3416888
    http://api.openweathermap.org/data/2.5/weather?q=
    Aripuana   3665202
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    San Jose   3590069
    http://api.openweathermap.org/data/2.5/weather?q=
    Isla Mujeres   3526756
    http://api.openweathermap.org/data/2.5/weather?q=
    Ribeira Grande   3372707
    http://api.openweathermap.org/data/2.5/weather?q=
    Pinheiro   3392054
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Filipe   3374210
    http://api.openweathermap.org/data/2.5/weather?q=
    Linhares   3458498
    http://api.openweathermap.org/data/2.5/weather?q=
    Beya   1510131
    http://api.openweathermap.org/data/2.5/weather?q=
    Morondava   1058381
    http://api.openweathermap.org/data/2.5/weather?q=
    Beroroha   1066831
    http://api.openweathermap.org/data/2.5/weather?q=
    Rikitea   4030556
    http://api.openweathermap.org/data/2.5/weather?q=
    Kutum   371745
    http://api.openweathermap.org/data/2.5/weather?q=
    Rikitea   4030556
    http://api.openweathermap.org/data/2.5/weather?q=
    Nabire   1634614
    http://api.openweathermap.org/data/2.5/weather?q=
    Lebu   3883457
    http://api.openweathermap.org/data/2.5/weather?q=
    San Pedro   3919886
    http://api.openweathermap.org/data/2.5/weather?q=
    Ivangorod   555624
    http://api.openweathermap.org/data/2.5/weather?q=
    Hami   1529484
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Kutum   371745
    http://api.openweathermap.org/data/2.5/weather?q=
    Beaupre   6325479
    http://api.openweathermap.org/data/2.5/weather?q=
    Praia   3374333
    http://api.openweathermap.org/data/2.5/weather?q=
    Georgetown   2411397
    http://api.openweathermap.org/data/2.5/weather?q=
    Sandwick   2640416
    http://api.openweathermap.org/data/2.5/weather?q=
    Dehloran   136702
    http://api.openweathermap.org/data/2.5/weather?q=
    Latehar   1265025
    http://api.openweathermap.org/data/2.5/weather?q=
    Suntar   2015913
    http://api.openweathermap.org/data/2.5/weather?q=
    Cabinda   2243271
    http://api.openweathermap.org/data/2.5/weather?q=
    Colac   2171069
    http://api.openweathermap.org/data/2.5/weather?q=
    Olinda   3456160
    http://api.openweathermap.org/data/2.5/weather?q=
    Talnakh   1490256
    http://api.openweathermap.org/data/2.5/weather?q=
    East London   1006984
    http://api.openweathermap.org/data/2.5/weather?q=
    Krasnyy Yar   541344
    http://api.openweathermap.org/data/2.5/weather?q=
    Ribeira Grande   3372707
    http://api.openweathermap.org/data/2.5/weather?q=
    Mahebourg   934322
    http://api.openweathermap.org/data/2.5/weather?q=
    George   1002145
    http://api.openweathermap.org/data/2.5/weather?q=
    Binga   219057
    http://api.openweathermap.org/data/2.5/weather?q=
    Pangnirtung   6096551
    http://api.openweathermap.org/data/2.5/weather?q=
    Khilchipur   1266731
    http://api.openweathermap.org/data/2.5/weather?q=
    Bandarbeyla   64814
    http://api.openweathermap.org/data/2.5/weather?q=
    Bafra   751335
    http://api.openweathermap.org/data/2.5/weather?q=
    Nikel   522260
    http://api.openweathermap.org/data/2.5/weather?q=
    Protvino   504576
    http://api.openweathermap.org/data/2.5/weather?q=
    Ambilobe   1082243
    http://api.openweathermap.org/data/2.5/weather?q=
    Mae Chan   1152227
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Arraial do Cabo   3471451
    http://api.openweathermap.org/data/2.5/weather?q=
    Wichita Falls   4741752
    http://api.openweathermap.org/data/2.5/weather?q=
    Kuandian   2036283
    http://api.openweathermap.org/data/2.5/weather?q=
    Touros   3386213
    http://api.openweathermap.org/data/2.5/weather?q=
    Vanimo   2084442
    http://api.openweathermap.org/data/2.5/weather?q=
    Flinders   6255012
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Kontagora   2334008
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Cidreira   3466165
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint-Philippe   935215
    http://api.openweathermap.org/data/2.5/weather?q=
    Bardiyah   80509
    http://api.openweathermap.org/data/2.5/weather?q=
    Chimbote   3698304
    http://api.openweathermap.org/data/2.5/weather?q=
    Georgetown   2411397
    http://api.openweathermap.org/data/2.5/weather?q=
    Arcachon   3037253
    http://api.openweathermap.org/data/2.5/weather?q=
    Atar   2381334
    http://api.openweathermap.org/data/2.5/weather?q=
    Lafia   2332515
    http://api.openweathermap.org/data/2.5/weather?q=
    Itarema   3393692
    http://api.openweathermap.org/data/2.5/weather?q=
    Saskylakh   2017155
    http://api.openweathermap.org/data/2.5/weather?q=
    Namibe   3347019
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Vanimo   2084442
    http://api.openweathermap.org/data/2.5/weather?q=
    Port Lincoln   2063036
    http://api.openweathermap.org/data/2.5/weather?q=
    Kodinsk   1503037
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Shishou   1788268
    http://api.openweathermap.org/data/2.5/weather?q=
    Lorengau   2092164
    http://api.openweathermap.org/data/2.5/weather?q=
    Hamilton   3573197
    http://api.openweathermap.org/data/2.5/weather?q=
    Hamilton   3573197
    http://api.openweathermap.org/data/2.5/weather?q=
    Victor Harbor   2059470
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Ust-Barguzin   2013986
    http://api.openweathermap.org/data/2.5/weather?q=
    Fougamou   2400578
    http://api.openweathermap.org/data/2.5/weather?q=
    Pangnirtung   6096551
    http://api.openweathermap.org/data/2.5/weather?q=
    Spas-Demensk   489868
    http://api.openweathermap.org/data/2.5/weather?q=
    Qaqortoq   3420846
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    Tasiilaq   3424607
    http://api.openweathermap.org/data/2.5/weather?q=
    Clyde River   5924351
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Santo Domingo   3653015
    http://api.openweathermap.org/data/2.5/weather?q=
    Lebu   3883457
    http://api.openweathermap.org/data/2.5/weather?q=
    Beloha   1067565
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Kutum   371745
    http://api.openweathermap.org/data/2.5/weather?q=
    Sulangan   1685422
    http://api.openweathermap.org/data/2.5/weather?q=
    Kalmunai   1242110
    http://api.openweathermap.org/data/2.5/weather?q=
    Kaduna   2335727
    http://api.openweathermap.org/data/2.5/weather?q=
    Fulton   4387496
    http://api.openweathermap.org/data/2.5/weather?q=
    Havre-Saint-Pierre   5972291
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Ponta do Sol   3374346
    http://api.openweathermap.org/data/2.5/weather?q=
    Marienburg   3383570
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Baiyin   1817240
    http://api.openweathermap.org/data/2.5/weather?q=
    Saldanha   3361934
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Limbang   1737714
    http://api.openweathermap.org/data/2.5/weather?q=
    Bloemfontein   1018725
    http://api.openweathermap.org/data/2.5/weather?q=
    Camara de Lobos   2270380
    http://api.openweathermap.org/data/2.5/weather?q=
    Eyl   60019
    http://api.openweathermap.org/data/2.5/weather?q=
    Kandara   400742
    http://api.openweathermap.org/data/2.5/weather?q=
    Monzon   3116224
    http://api.openweathermap.org/data/2.5/weather?q=
    Chunoyar   1507638
    http://api.openweathermap.org/data/2.5/weather?q=
    Nanortalik   3421765
    http://api.openweathermap.org/data/2.5/weather?q=
    Sangar   2017215
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Shubarshi   608270
    http://api.openweathermap.org/data/2.5/weather?q=
    Mahebourg   934322
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Luiz Gonzaga   3448552
    http://api.openweathermap.org/data/2.5/weather?q=
    Kirkenaer   3150261
    http://api.openweathermap.org/data/2.5/weather?q=
    Ponta do Sol   3374346
    http://api.openweathermap.org/data/2.5/weather?q=
    Killybegs   2963295
    http://api.openweathermap.org/data/2.5/weather?q=
    Shingu   1847947
    http://api.openweathermap.org/data/2.5/weather?q=
    Shaki   2337765
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Camacha   2270385
    http://api.openweathermap.org/data/2.5/weather?q=
    Mahebourg   934322
    http://api.openweathermap.org/data/2.5/weather?q=
    Karratha   6620339
    http://api.openweathermap.org/data/2.5/weather?q=
    Thunder Bay   6166142
    http://api.openweathermap.org/data/2.5/weather?q=
    San Luis   3837056
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Filipe   3374210
    http://api.openweathermap.org/data/2.5/weather?q=
    Barra do Garcas   3470709
    http://api.openweathermap.org/data/2.5/weather?q=
    Mildura   2157698
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Zhenlai   2033128
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    Acapulco   3533462
    http://api.openweathermap.org/data/2.5/weather?q=
    Ypsonas   145952
    http://api.openweathermap.org/data/2.5/weather?q=
    Castrovillari   6540785
    http://api.openweathermap.org/data/2.5/weather?q=
    Pangoa   3933104
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Clyde River   5924351
    http://api.openweathermap.org/data/2.5/weather?q=
    Katsuura   1865309
    http://api.openweathermap.org/data/2.5/weather?q=
    Fairview   5951036
    http://api.openweathermap.org/data/2.5/weather?q=
    Flin Flon   5954718
    http://api.openweathermap.org/data/2.5/weather?q=
    Yuksekova   296173
    http://api.openweathermap.org/data/2.5/weather?q=
    Ponta Delgada   3372783
    http://api.openweathermap.org/data/2.5/weather?q=
    Arraial do Cabo   3471451
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Fort Frances   5955826
    http://api.openweathermap.org/data/2.5/weather?q=
    Husavik   2629833
    http://api.openweathermap.org/data/2.5/weather?q=
    Cabo San Lucas   3985710
    http://api.openweathermap.org/data/2.5/weather?q=
    Kavaratti   1267390
    http://api.openweathermap.org/data/2.5/weather?q=
    Muzambinho   3456412
    http://api.openweathermap.org/data/2.5/weather?q=
    Tocopilla   3869716
    http://api.openweathermap.org/data/2.5/weather?q=
    Jumla   1283285
    http://api.openweathermap.org/data/2.5/weather?q=
    Muisne   3653967
    http://api.openweathermap.org/data/2.5/weather?q=
    Zarubino   2012646
    http://api.openweathermap.org/data/2.5/weather?q=
    Pauini   3662927
    http://api.openweathermap.org/data/2.5/weather?q=
    Kampot   1831112
    http://api.openweathermap.org/data/2.5/weather?q=
    Boa Vista   3664980
    http://api.openweathermap.org/data/2.5/weather?q=
    Obo   236950
    http://api.openweathermap.org/data/2.5/weather?q=
    Khatanga   2022572
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Luderitz   3355672
    http://api.openweathermap.org/data/2.5/weather?q=
    Khudumelapye   933562
    http://api.openweathermap.org/data/2.5/weather?q=
    Cahokia   4234969
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Dingzhou   1812728
    http://api.openweathermap.org/data/2.5/weather?q=
    Mana   3381041
    http://api.openweathermap.org/data/2.5/weather?q=
    Babina   1278058
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Yakeshi   2033536
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Maria   3374218
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Gobabis   3357247
    http://api.openweathermap.org/data/2.5/weather?q=
    Hirna   334700
    http://api.openweathermap.org/data/2.5/weather?q=
    Mareeba   2158767
    http://api.openweathermap.org/data/2.5/weather?q=
    Yichang   1786764
    http://api.openweathermap.org/data/2.5/weather?q=
    Shaunavon   6145425
    http://api.openweathermap.org/data/2.5/weather?q=
    Salalah   286621
    http://api.openweathermap.org/data/2.5/weather?q=
    Lagoa   2267254
    http://api.openweathermap.org/data/2.5/weather?q=
    Ponta do Sol   3374346
    http://api.openweathermap.org/data/2.5/weather?q=
    Methoni   257122
    http://api.openweathermap.org/data/2.5/weather?q=
    Naryan-Mar   523392
    http://api.openweathermap.org/data/2.5/weather?q=
    Touros   3386213
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint-Joseph   6690296
    http://api.openweathermap.org/data/2.5/weather?q=
    Kijang   1640044
    http://api.openweathermap.org/data/2.5/weather?q=
    Bosaso   64013
    http://api.openweathermap.org/data/2.5/weather?q=
    Rikitea   4030556
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Edson   5946820
    http://api.openweathermap.org/data/2.5/weather?q=
    Fort-Shevchenko   609906
    http://api.openweathermap.org/data/2.5/weather?q=
    Pato Branco   3454818
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Wolow   3081430
    http://api.openweathermap.org/data/2.5/weather?q=
    Saldanha   3361934
    http://api.openweathermap.org/data/2.5/weather?q=
    Chandbali   1274761
    http://api.openweathermap.org/data/2.5/weather?q=
    Mirnyy   502265
    http://api.openweathermap.org/data/2.5/weather?q=
    Chapais   5919850
    http://api.openweathermap.org/data/2.5/weather?q=
    Pochutla   3517970
    http://api.openweathermap.org/data/2.5/weather?q=
    Le Havre   3003796
    http://api.openweathermap.org/data/2.5/weather?q=
    Richards Bay   962367
    http://api.openweathermap.org/data/2.5/weather?q=
    Ugoofaaru   1337619
    http://api.openweathermap.org/data/2.5/weather?q=
    Mezen   527321
    http://api.openweathermap.org/data/2.5/weather?q=
    Valparaiso   3868626
    http://api.openweathermap.org/data/2.5/weather?q=
    Menongue   3347353
    http://api.openweathermap.org/data/2.5/weather?q=
    Griffith   2164422
    http://api.openweathermap.org/data/2.5/weather?q=
    Pljevlja   3193161
    http://api.openweathermap.org/data/2.5/weather?q=
    Batagay-Alyta   2027042
    http://api.openweathermap.org/data/2.5/weather?q=
    Beloha   1067565
    http://api.openweathermap.org/data/2.5/weather?q=
    Vanimo   2084442
    http://api.openweathermap.org/data/2.5/weather?q=
    Clifton   5096699
    http://api.openweathermap.org/data/2.5/weather?q=
    Sambava   1056899
    http://api.openweathermap.org/data/2.5/weather?q=
    Vila Velha   6320062
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Lingyuan   2036075
    http://api.openweathermap.org/data/2.5/weather?q=
    Iqaluit   5983720
    http://api.openweathermap.org/data/2.5/weather?q=
    Qasigiannguit   3420768
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Lagoa   2267254
    http://api.openweathermap.org/data/2.5/weather?q=
    Ancud   3899695
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Rikitea   4030556
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Luzia   3450144
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   5222866
    http://api.openweathermap.org/data/2.5/weather?q=
    Matara   1235846
    http://api.openweathermap.org/data/2.5/weather?q=
    Iqaluit   5983720
    http://api.openweathermap.org/data/2.5/weather?q=
    Porto Novo   3374336
    http://api.openweathermap.org/data/2.5/weather?q=
    Klaksvik   2618795
    http://api.openweathermap.org/data/2.5/weather?q=
    Havoysund   779622
    http://api.openweathermap.org/data/2.5/weather?q=
    Viedma   3832899
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Yumen   1528998
    http://api.openweathermap.org/data/2.5/weather?q=
    Moose Jaw   6078112
    http://api.openweathermap.org/data/2.5/weather?q=
    Torbay   6167817
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Karratha   6620339
    http://api.openweathermap.org/data/2.5/weather?q=
    Ereymentau   1524302
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Esperance   2071860
    http://api.openweathermap.org/data/2.5/weather?q=
    Kenora   5991055
    http://api.openweathermap.org/data/2.5/weather?q=
    Arraial do Cabo   3471451
    http://api.openweathermap.org/data/2.5/weather?q=
    Mpika   905846
    http://api.openweathermap.org/data/2.5/weather?q=
    Marostica   3173878
    http://api.openweathermap.org/data/2.5/weather?q=
    Vanavara   2013727
    http://api.openweathermap.org/data/2.5/weather?q=
    Panaba   3521972
    http://api.openweathermap.org/data/2.5/weather?q=
    Mezhdurechensk   1498920
    http://api.openweathermap.org/data/2.5/weather?q=
    Yulara   6355222
    http://api.openweathermap.org/data/2.5/weather?q=
    Hovd   1516048
    http://api.openweathermap.org/data/2.5/weather?q=
    Atmakur   1278201
    http://api.openweathermap.org/data/2.5/weather?q=
    Caucaia   3402429
    http://api.openweathermap.org/data/2.5/weather?q=
    Oranjemund   3354071
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint George   3573061
    http://api.openweathermap.org/data/2.5/weather?q=
    Lourical   2266974
    http://api.openweathermap.org/data/2.5/weather?q=
    Chambersburg   4557109
    http://api.openweathermap.org/data/2.5/weather?q=
    Polaniec   761652
    http://api.openweathermap.org/data/2.5/weather?q=
    Ust-Kulom   478050
    http://api.openweathermap.org/data/2.5/weather?q=
    Port Keats   2063039
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Sinnamary   3380290
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    Semnan   116402
    http://api.openweathermap.org/data/2.5/weather?q=
    Hays   4272782
    http://api.openweathermap.org/data/2.5/weather?q=
    Leshukonskoye   535839
    http://api.openweathermap.org/data/2.5/weather?q=
    Nyurba   2018735
    http://api.openweathermap.org/data/2.5/weather?q=
    Nenjiang   2035601
    http://api.openweathermap.org/data/2.5/weather?q=
    Dong Hoi   1582886
    http://api.openweathermap.org/data/2.5/weather?q=
    Teya   1489656
    http://api.openweathermap.org/data/2.5/weather?q=
    Prudentopolis   3452216
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Cruz de la Palma   2511180
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Kloulklubed   7671223
    http://api.openweathermap.org/data/2.5/weather?q=
    Georgetown   2411397
    http://api.openweathermap.org/data/2.5/weather?q=
    Hirara   1862505
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint George   3573061
    http://api.openweathermap.org/data/2.5/weather?q=
    Sangar   2017215
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Yining   1786538
    http://api.openweathermap.org/data/2.5/weather?q=
    Vanimo   2084442
    http://api.openweathermap.org/data/2.5/weather?q=
    Muros   3115824
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Francisco de Paula   3449121
    http://api.openweathermap.org/data/2.5/weather?q=
    Arraial do Cabo   3471451
    http://api.openweathermap.org/data/2.5/weather?q=
    Adrar   2508813
    http://api.openweathermap.org/data/2.5/weather?q=
    Yerbogachen   2012956
    http://api.openweathermap.org/data/2.5/weather?q=
    Lebu   3883457
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   1014034
    http://api.openweathermap.org/data/2.5/weather?q=
    Okha   2122614
    http://api.openweathermap.org/data/2.5/weather?q=
    Babushkin   2027276
    http://api.openweathermap.org/data/2.5/weather?q=
    Khvalynsk   548982
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Juegang   1804979
    http://api.openweathermap.org/data/2.5/weather?q=
    Duvan   564022
    http://api.openweathermap.org/data/2.5/weather?q=
    Mahibadhoo   1337605
    http://api.openweathermap.org/data/2.5/weather?q=
    Nyimba   900056
    http://api.openweathermap.org/data/2.5/weather?q=
    Georgetown   2411397
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Cruz   3621607
    http://api.openweathermap.org/data/2.5/weather?q=
    Ixtapa   4004293
    http://api.openweathermap.org/data/2.5/weather?q=
    Chokurdakh   2126123
    http://api.openweathermap.org/data/2.5/weather?q=
    Dwarka   1273294
    http://api.openweathermap.org/data/2.5/weather?q=
    Verkhniy Landekh   474931
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Moss Point   4436743
    http://api.openweathermap.org/data/2.5/weather?q=
    Paamiut   3421193
    http://api.openweathermap.org/data/2.5/weather?q=
    Yatou   1783956
    http://api.openweathermap.org/data/2.5/weather?q=
    Ribeira Grande   3372707
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Maria   3374218
    http://api.openweathermap.org/data/2.5/weather?q=
    Tallahassee   4174715
    http://api.openweathermap.org/data/2.5/weather?q=
    Barcelona   3128760
    http://api.openweathermap.org/data/2.5/weather?q=
    Zhezkazgan   1516589
    http://api.openweathermap.org/data/2.5/weather?q=
    Gallup   5468773
    http://api.openweathermap.org/data/2.5/weather?q=
    Vostok   2013279
    http://api.openweathermap.org/data/2.5/weather?q=
    Vila Franca do Campo   3372472
    http://api.openweathermap.org/data/2.5/weather?q=
    Sri Aman   1735799
    http://api.openweathermap.org/data/2.5/weather?q=
    Kincardine   5992144
    http://api.openweathermap.org/data/2.5/weather?q=
    Georgetown   2411397
    http://api.openweathermap.org/data/2.5/weather?q=
    Omboue   2396853
    http://api.openweathermap.org/data/2.5/weather?q=
    Hellvik   3133472
    http://api.openweathermap.org/data/2.5/weather?q=
    Doctor Arroyo   4011873
    http://api.openweathermap.org/data/2.5/weather?q=
    Sebina   933117
    http://api.openweathermap.org/data/2.5/weather?q=
    Katsuura   1865309
    http://api.openweathermap.org/data/2.5/weather?q=
    Shache   1280037
    http://api.openweathermap.org/data/2.5/weather?q=
    Zubtsov   462008
    http://api.openweathermap.org/data/2.5/weather?q=
    Linhares   3458498
    http://api.openweathermap.org/data/2.5/weather?q=
    Itupeva   3460516
    http://api.openweathermap.org/data/2.5/weather?q=
    Kashiwazaki   1859908
    http://api.openweathermap.org/data/2.5/weather?q=
    Tahoua   2439376
    http://api.openweathermap.org/data/2.5/weather?q=
    Songea   877401
    http://api.openweathermap.org/data/2.5/weather?q=
    Nanortalik   3421765
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Rosa del Sur   3792379
    http://api.openweathermap.org/data/2.5/weather?q=
    Lagoa   2267254
    http://api.openweathermap.org/data/2.5/weather?q=
    Piacabucu   3454005
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Jumla   1283285
    http://api.openweathermap.org/data/2.5/weather?q=
    Bacolod   1729564
    http://api.openweathermap.org/data/2.5/weather?q=
    Karratha   6620339
    http://api.openweathermap.org/data/2.5/weather?q=
    Qaqortoq   3420846
    http://api.openweathermap.org/data/2.5/weather?q=
    Zambezi   895953
    http://api.openweathermap.org/data/2.5/weather?q=
    Upernavik   3418910
    http://api.openweathermap.org/data/2.5/weather?q=
    Miri   1738050
    http://api.openweathermap.org/data/2.5/weather?q=
    Nuevo Progreso   3522013
    http://api.openweathermap.org/data/2.5/weather?q=
    Arlit   2447513
    http://api.openweathermap.org/data/2.5/weather?q=
    Isla Aguada   3526766
    http://api.openweathermap.org/data/2.5/weather?q=
    Banepa   1283679
    http://api.openweathermap.org/data/2.5/weather?q=
    Cunha   3465010
    http://api.openweathermap.org/data/2.5/weather?q=
    Awbari   2219235
    http://api.openweathermap.org/data/2.5/weather?q=
    Bilma   2446796
    http://api.openweathermap.org/data/2.5/weather?q=
    Kudymkar   539817
    http://api.openweathermap.org/data/2.5/weather?q=
    Saldanha   3361934
    http://api.openweathermap.org/data/2.5/weather?q=
    Yulara   6355222
    http://api.openweathermap.org/data/2.5/weather?q=
    Quatre Cocos   1106643
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint-Francois   3578441
    http://api.openweathermap.org/data/2.5/weather?q=
    Chuy   3443061
    http://api.openweathermap.org/data/2.5/weather?q=
    Tiksi   2015306
    http://api.openweathermap.org/data/2.5/weather?q=
    Quatre Cocos   1106643
    http://api.openweathermap.org/data/2.5/weather?q=
    Sadsadan   1691194
    http://api.openweathermap.org/data/2.5/weather?q=
    Raudeberg   3146487
    http://api.openweathermap.org/data/2.5/weather?q=
    Tawang   1254832
    http://api.openweathermap.org/data/2.5/weather?q=
    Tiksi   2015306
    http://api.openweathermap.org/data/2.5/weather?q=
    Presidencia Roque Saenz Pena   3840300
    http://api.openweathermap.org/data/2.5/weather?q=
    Korostyshiv   704885
    http://api.openweathermap.org/data/2.5/weather?q=
    Dalbandin   1180729
    http://api.openweathermap.org/data/2.5/weather?q=
    Hirara   1862505
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Port Lincoln   2063036
    http://api.openweathermap.org/data/2.5/weather?q=
    Itarema   3393692
    http://api.openweathermap.org/data/2.5/weather?q=
    High Rock   3572189
    http://api.openweathermap.org/data/2.5/weather?q=
    Paranga   512618
    http://api.openweathermap.org/data/2.5/weather?q=
    Rapid Valley   5768244
    http://api.openweathermap.org/data/2.5/weather?q=
    Vila Franca do Campo   3372472
    http://api.openweathermap.org/data/2.5/weather?q=
    Mount Gambier   2156643
    http://api.openweathermap.org/data/2.5/weather?q=
    Taraz   1516905
    http://api.openweathermap.org/data/2.5/weather?q=
    Gigmoto   1712961
    http://api.openweathermap.org/data/2.5/weather?q=
    Galle   1246294
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint-Augustin   6138501
    http://api.openweathermap.org/data/2.5/weather?q=
    Hauterive   5889745
    http://api.openweathermap.org/data/2.5/weather?q=
    Weinan   1791636
    http://api.openweathermap.org/data/2.5/weather?q=
    La Romana   3500957
    http://api.openweathermap.org/data/2.5/weather?q=
    Seoul   1835848
    http://api.openweathermap.org/data/2.5/weather?q=
    Navirai   3456368
    http://api.openweathermap.org/data/2.5/weather?q=
    Two Hills   6171332
    http://api.openweathermap.org/data/2.5/weather?q=
    Kaz   1489779
    http://api.openweathermap.org/data/2.5/weather?q=
    Sampit   1628884
    http://api.openweathermap.org/data/2.5/weather?q=
    Nadym   1498087
    http://api.openweathermap.org/data/2.5/weather?q=
    Luderitz   3355672
    http://api.openweathermap.org/data/2.5/weather?q=
    Salta   3838233
    http://api.openweathermap.org/data/2.5/weather?q=
    Lorengau   2092164
    http://api.openweathermap.org/data/2.5/weather?q=
    Bathsheba   3374083
    http://api.openweathermap.org/data/2.5/weather?q=
    Banjar   1650233
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Miguel do Araguaia   3448455
    http://api.openweathermap.org/data/2.5/weather?q=
    Lagoa   2267254
    http://api.openweathermap.org/data/2.5/weather?q=
    Kloulklubed   7671223
    http://api.openweathermap.org/data/2.5/weather?q=
    Coahuayana   3981460
    http://api.openweathermap.org/data/2.5/weather?q=
    Nanortalik   3421765
    http://api.openweathermap.org/data/2.5/weather?q=
    Rabo de Peixe   3372745
    http://api.openweathermap.org/data/2.5/weather?q=
    Sembakung   1627877
    http://api.openweathermap.org/data/2.5/weather?q=
    Bowling Green   4285268
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Saskylakh   2017155
    http://api.openweathermap.org/data/2.5/weather?q=
    Sorland   3137469
    http://api.openweathermap.org/data/2.5/weather?q=
    Belyy Yar   1510377
    http://api.openweathermap.org/data/2.5/weather?q=
    Cockburn Town   3576994
    http://api.openweathermap.org/data/2.5/weather?q=
    Leh   1264976
    http://api.openweathermap.org/data/2.5/weather?q=
    Kavaratti   1267390
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Yumen   1528998
    http://api.openweathermap.org/data/2.5/weather?q=
    Luau   876177
    http://api.openweathermap.org/data/2.5/weather?q=
    Hearst   5973108
    http://api.openweathermap.org/data/2.5/weather?q=
    Beira   1052373
    http://api.openweathermap.org/data/2.5/weather?q=
    Bathsheba   3374083
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Renfrew   6119448
    http://api.openweathermap.org/data/2.5/weather?q=
    Tiznit   2527089
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Cururupu   3401148
    http://api.openweathermap.org/data/2.5/weather?q=
    Oktyabrskoye   1511846
    http://api.openweathermap.org/data/2.5/weather?q=
    Syntul   485206
    http://api.openweathermap.org/data/2.5/weather?q=
    Lebu   3883457
    http://api.openweathermap.org/data/2.5/weather?q=
    Atuona   4020109
    http://api.openweathermap.org/data/2.5/weather?q=
    Bikin   2026696
    http://api.openweathermap.org/data/2.5/weather?q=
    Katobu   1640972
    http://api.openweathermap.org/data/2.5/weather?q=
    Padang   1633419
    http://api.openweathermap.org/data/2.5/weather?q=
    Guarapari   3461888
    http://api.openweathermap.org/data/2.5/weather?q=
    Clyde River   5924351
    http://api.openweathermap.org/data/2.5/weather?q=
    Albany   2077963
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint George   3573061
    http://api.openweathermap.org/data/2.5/weather?q=
    Kawalu   1640902
    http://api.openweathermap.org/data/2.5/weather?q=
    Gerash   133595
    http://api.openweathermap.org/data/2.5/weather?q=
    Jinchang   1805733
    http://api.openweathermap.org/data/2.5/weather?q=
    Longonjo   3347853
    http://api.openweathermap.org/data/2.5/weather?q=
    Tshela   2311127
    http://api.openweathermap.org/data/2.5/weather?q=
    Sibu   1735902
    http://api.openweathermap.org/data/2.5/weather?q=
    Margate   978895
    http://api.openweathermap.org/data/2.5/weather?q=
    Maningrida   2067089
    http://api.openweathermap.org/data/2.5/weather?q=
    Sisimiut   3419842
    http://api.openweathermap.org/data/2.5/weather?q=
    Ust-Kuyga   2013921
    http://api.openweathermap.org/data/2.5/weather?q=
    Martapura   1636022
    http://api.openweathermap.org/data/2.5/weather?q=
    Kalmunai   1242110
    http://api.openweathermap.org/data/2.5/weather?q=
    Oistins   3373652
    http://api.openweathermap.org/data/2.5/weather?q=
    Rtishchevo   500886
    http://api.openweathermap.org/data/2.5/weather?q=
    Basco   1726449
    http://api.openweathermap.org/data/2.5/weather?q=
    Cap-aux-Meules   5915327
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Qaanaaq   3831208
    http://api.openweathermap.org/data/2.5/weather?q=
    Dhankuta   1283465
    http://api.openweathermap.org/data/2.5/weather?q=
    Kostomuksha   543899
    http://api.openweathermap.org/data/2.5/weather?q=
    Acapulco   3533462
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Cruz   3871616
    http://api.openweathermap.org/data/2.5/weather?q=
    Marawi   370481
    http://api.openweathermap.org/data/2.5/weather?q=
    Alyangula   2079582
    http://api.openweathermap.org/data/2.5/weather?q=
    Kangaatsiaq   3422683
    http://api.openweathermap.org/data/2.5/weather?q=
    Celestun   3531368
    http://api.openweathermap.org/data/2.5/weather?q=
    Atuona   4020109
    http://api.openweathermap.org/data/2.5/weather?q=
    Changzhou   1815456
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Aripuana   3665202
    http://api.openweathermap.org/data/2.5/weather?q=
    Luwuk   1637001
    http://api.openweathermap.org/data/2.5/weather?q=
    Deputatskiy   2028164
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Houma   4328010
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Moron   2029947
    http://api.openweathermap.org/data/2.5/weather?q=
    Mutoko   884927
    http://api.openweathermap.org/data/2.5/weather?q=
    Alyangula   2079582
    http://api.openweathermap.org/data/2.5/weather?q=
    Dingle   2964782
    http://api.openweathermap.org/data/2.5/weather?q=
    Shenzhen   1795565
    http://api.openweathermap.org/data/2.5/weather?q=
    Ussel   2971298
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    Lebu   3883457
    http://api.openweathermap.org/data/2.5/weather?q=
    Nouakchott   2377450
    http://api.openweathermap.org/data/2.5/weather?q=
    Pisco   3932145
    http://api.openweathermap.org/data/2.5/weather?q=
    Ust-Omchug   2120047
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Touros   3386213
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Flin Flon   5954718
    http://api.openweathermap.org/data/2.5/weather?q=
    Ponta do Sol   3374346
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Fujinomiya   1864105
    http://api.openweathermap.org/data/2.5/weather?q=
    Arlit   2447513
    http://api.openweathermap.org/data/2.5/weather?q=
    Naron   3115739
    http://api.openweathermap.org/data/2.5/weather?q=
    Geraldton   2070998
    http://api.openweathermap.org/data/2.5/weather?q=
    Koungou   1090225
    http://api.openweathermap.org/data/2.5/weather?q=
    Ionia   4997191
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambey   2252309
    http://api.openweathermap.org/data/2.5/weather?q=
    Valparaiso   3868626
    http://api.openweathermap.org/data/2.5/weather?q=
    Qaanaaq   3831208
    http://api.openweathermap.org/data/2.5/weather?q=
    Bar Harbor   4957320
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Jaszalsoszentgyorgy   719656
    http://api.openweathermap.org/data/2.5/weather?q=
    Turayf   101312
    http://api.openweathermap.org/data/2.5/weather?q=
    Hofn   2630299
    http://api.openweathermap.org/data/2.5/weather?q=
    Marsh Harbour   3571913
    http://api.openweathermap.org/data/2.5/weather?q=
    Zarumilla   3818398
    http://api.openweathermap.org/data/2.5/weather?q=
    Smolenka   552312
    http://api.openweathermap.org/data/2.5/weather?q=
    Coracora   3942259
    http://api.openweathermap.org/data/2.5/weather?q=
    Havre-Saint-Pierre   5972291
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Cairns   2172797
    http://api.openweathermap.org/data/2.5/weather?q=
    Port Alfred   964432
    http://api.openweathermap.org/data/2.5/weather?q=
    East London   1006984
    http://api.openweathermap.org/data/2.5/weather?q=
    Mao   2428394
    http://api.openweathermap.org/data/2.5/weather?q=
    Kysyl-Syr   2021017
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint George   3573061
    http://api.openweathermap.org/data/2.5/weather?q=
    Klaksvik   2618795
    http://api.openweathermap.org/data/2.5/weather?q=
    Miri   1738050
    http://api.openweathermap.org/data/2.5/weather?q=
    Coruripe   3465329
    http://api.openweathermap.org/data/2.5/weather?q=
    Batticaloa   1250161
    http://api.openweathermap.org/data/2.5/weather?q=
    Venice   4176380
    http://api.openweathermap.org/data/2.5/weather?q=
    Yarim   69559
    http://api.openweathermap.org/data/2.5/weather?q=
    Ambon   1651531
    http://api.openweathermap.org/data/2.5/weather?q=
    Atar   2381334
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint-Philippe   935215
    http://api.openweathermap.org/data/2.5/weather?q=
    Rabat   2538474
    http://api.openweathermap.org/data/2.5/weather?q=
    Sassandra   2281951
    http://api.openweathermap.org/data/2.5/weather?q=
    Azangaro   3946937
    http://api.openweathermap.org/data/2.5/weather?q=
    Luderitz   3355672
    http://api.openweathermap.org/data/2.5/weather?q=
    San Nicolas   3855666
    http://api.openweathermap.org/data/2.5/weather?q=
    Bud   7626370
    http://api.openweathermap.org/data/2.5/weather?q=
    Aykhal   2027296
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Lebu   3883457
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Jose da Coroa Grande   3388456
    http://api.openweathermap.org/data/2.5/weather?q=
    Khakhea   933649
    http://api.openweathermap.org/data/2.5/weather?q=
    Cayenne   3382160
    http://api.openweathermap.org/data/2.5/weather?q=
    Chumikan   2025256
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    Saint George   3573061
    http://api.openweathermap.org/data/2.5/weather?q=
    Pangnirtung   6096551
    http://api.openweathermap.org/data/2.5/weather?q=
    Severo-Yeniseyskiy   1492566
    http://api.openweathermap.org/data/2.5/weather?q=
    Hella   3415720
    http://api.openweathermap.org/data/2.5/weather?q=
    Kyrylivka   689198
    http://api.openweathermap.org/data/2.5/weather?q=
    Peniche   2264923
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Iberia   3938531
    http://api.openweathermap.org/data/2.5/weather?q=
    Qaanaaq   3831208
    http://api.openweathermap.org/data/2.5/weather?q=
    Ponta do Sol   3374346
    http://api.openweathermap.org/data/2.5/weather?q=
    Camabatela   2242885
    http://api.openweathermap.org/data/2.5/weather?q=
    Ust-Ilimsk   2013952
    http://api.openweathermap.org/data/2.5/weather?q=
    Biltine   244878
    http://api.openweathermap.org/data/2.5/weather?q=
    Puerto Ayora   3652764
    http://api.openweathermap.org/data/2.5/weather?q=
    Nicoya   3622716
    http://api.openweathermap.org/data/2.5/weather?q=
    Curuca   3663145
    http://api.openweathermap.org/data/2.5/weather?q=
    Arraial do Cabo   3471451
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Catu   3466641
    http://api.openweathermap.org/data/2.5/weather?q=
    Iranshahr   1160939
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambanglipuro   1650434
    http://api.openweathermap.org/data/2.5/weather?q=
    Atar   2381334
    http://api.openweathermap.org/data/2.5/weather?q=
    Naze   1855540
    http://api.openweathermap.org/data/2.5/weather?q=
    Kushima   1855476
    http://api.openweathermap.org/data/2.5/weather?q=
    Presidente Dutra   3391220
    http://api.openweathermap.org/data/2.5/weather?q=
    Maniitsoq   3421982
    http://api.openweathermap.org/data/2.5/weather?q=
    Conceicao do Araguaia   3401845
    http://api.openweathermap.org/data/2.5/weather?q=
    Aswan   359792
    http://api.openweathermap.org/data/2.5/weather?q=
    Manta   3654410
    http://api.openweathermap.org/data/2.5/weather?q=
    Matay   352628
    http://api.openweathermap.org/data/2.5/weather?q=
    Nanortalik   3421765
    http://api.openweathermap.org/data/2.5/weather?q=
    Kisanga   149929
    http://api.openweathermap.org/data/2.5/weather?q=
    Arrecife   2521570
    http://api.openweathermap.org/data/2.5/weather?q=
    Nadym   1498087
    http://api.openweathermap.org/data/2.5/weather?q=
    Athabasca   5887916
    http://api.openweathermap.org/data/2.5/weather?q=
    Biak   1637001
    http://api.openweathermap.org/data/2.5/weather?q=
    Bathsheba   3374083
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Atuona   4020109
    http://api.openweathermap.org/data/2.5/weather?q=
    Harbour Breton   5970478
    http://api.openweathermap.org/data/2.5/weather?q=
    Taungdwingyi   1294041
    http://api.openweathermap.org/data/2.5/weather?q=
    San Patricio   3985168
    http://api.openweathermap.org/data/2.5/weather?q=
    Hambantota   1244926
    http://api.openweathermap.org/data/2.5/weather?q=
    Dingle   2964782
    http://api.openweathermap.org/data/2.5/weather?q=
    Naze   1855540
    http://api.openweathermap.org/data/2.5/weather?q=
    Korla   1529376
    http://api.openweathermap.org/data/2.5/weather?q=
    Lorengau   2092164
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Shelekhov   2016764
    http://api.openweathermap.org/data/2.5/weather?q=
    Talcahuano   3870282
    http://api.openweathermap.org/data/2.5/weather?q=
    Rio Bravo   4722831
    http://api.openweathermap.org/data/2.5/weather?q=
    La Ronge   6050066
    http://api.openweathermap.org/data/2.5/weather?q=
    Busselton   2075265
    http://api.openweathermap.org/data/2.5/weather?q=
    Camacha   2270385
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Filipe   3374210
    http://api.openweathermap.org/data/2.5/weather?q=
    Ribeira Grande   3372707
    http://api.openweathermap.org/data/2.5/weather?q=
    Tecoanapa   3532499
    http://api.openweathermap.org/data/2.5/weather?q=
    Kochi   1273874
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Korla   1529376
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Bela Vista   3470177
    http://api.openweathermap.org/data/2.5/weather?q=
    Dingle   2964782
    http://api.openweathermap.org/data/2.5/weather?q=
    Gali   614613
    http://api.openweathermap.org/data/2.5/weather?q=
    Bardsir   124647
    http://api.openweathermap.org/data/2.5/weather?q=
    Tessalit   2449893
    http://api.openweathermap.org/data/2.5/weather?q=
    Klaksvik   2618795
    http://api.openweathermap.org/data/2.5/weather?q=
    Burlington   5234372
    http://api.openweathermap.org/data/2.5/weather?q=
    Hami   1529484
    http://api.openweathermap.org/data/2.5/weather?q=
    Atuona   4020109
    http://api.openweathermap.org/data/2.5/weather?q=
    Cartagena   3675443
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Gabriel da Cachoeira   3662342
    http://api.openweathermap.org/data/2.5/weather?q=
    Ratnagiri   1258338
    http://api.openweathermap.org/data/2.5/weather?q=
    San Juan   4726440
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Broome   2075720
    http://api.openweathermap.org/data/2.5/weather?q=
    Chifeng   2038067
    http://api.openweathermap.org/data/2.5/weather?q=
    Grindavik   3416888
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Juybar   129933
    http://api.openweathermap.org/data/2.5/weather?q=
    Torbay   6167817
    http://api.openweathermap.org/data/2.5/weather?q=
    Kupang   2057087
    http://api.openweathermap.org/data/2.5/weather?q=
    Adrar   2508813
    http://api.openweathermap.org/data/2.5/weather?q=
    Brumado   3468893
    http://api.openweathermap.org/data/2.5/weather?q=
    Carnarvon   2074865
    http://api.openweathermap.org/data/2.5/weather?q=
    La Ligua   3885456
    http://api.openweathermap.org/data/2.5/weather?q=
    Bathsheba   3374083
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Pak Chong   1608057
    http://api.openweathermap.org/data/2.5/weather?q=
    Toro   3107886
    http://api.openweathermap.org/data/2.5/weather?q=
    El Coyote   3524348
    http://api.openweathermap.org/data/2.5/weather?q=
    Neijiang   1799491
    http://api.openweathermap.org/data/2.5/weather?q=
    Saldanha   3361934
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Teya   1489656
    http://api.openweathermap.org/data/2.5/weather?q=
    Cabo San Lucas   3985710
    http://api.openweathermap.org/data/2.5/weather?q=
    Hobyo   57000
    http://api.openweathermap.org/data/2.5/weather?q=
    Trofors   3133983
    http://api.openweathermap.org/data/2.5/weather?q=
    Vila do Maio   3374120
    http://api.openweathermap.org/data/2.5/weather?q=
    Port Alfred   964432
    http://api.openweathermap.org/data/2.5/weather?q=
    Sistranda   3139597
    http://api.openweathermap.org/data/2.5/weather?q=
    Coquimbo   3893629
    http://api.openweathermap.org/data/2.5/weather?q=
    Sept-Iles   6144312
    http://api.openweathermap.org/data/2.5/weather?q=
    Den Helder   2757220
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Shaowu   1795857
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Surt   2210554
    http://api.openweathermap.org/data/2.5/weather?q=
    Vagur   2610806
    http://api.openweathermap.org/data/2.5/weather?q=
    Gewane   336745
    http://api.openweathermap.org/data/2.5/weather?q=
    Jumla   1283285
    http://api.openweathermap.org/data/2.5/weather?q=
    Esperance   2071860
    http://api.openweathermap.org/data/2.5/weather?q=
    Lagoa   2267254
    http://api.openweathermap.org/data/2.5/weather?q=
    Amazar   2027806
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Port-Gentil   2396518
    http://api.openweathermap.org/data/2.5/weather?q=
    Ostrovnoy   556268
    http://api.openweathermap.org/data/2.5/weather?q=
    Katsuura   1865309
    http://api.openweathermap.org/data/2.5/weather?q=
    Yellowknife   6185377
    http://api.openweathermap.org/data/2.5/weather?q=
    Victoria   241131
    http://api.openweathermap.org/data/2.5/weather?q=
    Vestmannaeyjar   3412093
    http://api.openweathermap.org/data/2.5/weather?q=
    Coquimbo   3893629
    http://api.openweathermap.org/data/2.5/weather?q=
    Deniliquin   2169068
    http://api.openweathermap.org/data/2.5/weather?q=
    Aban   1512218
    http://api.openweathermap.org/data/2.5/weather?q=
    Torbay   6167817
    http://api.openweathermap.org/data/2.5/weather?q=
    Santa Maria   3374218
    http://api.openweathermap.org/data/2.5/weather?q=
    Forbes   2166368
    http://api.openweathermap.org/data/2.5/weather?q=
    Rikitea   4030556
    http://api.openweathermap.org/data/2.5/weather?q=
    Marsabit   187585
    http://api.openweathermap.org/data/2.5/weather?q=
    Kwinana   2068079
    http://api.openweathermap.org/data/2.5/weather?q=
    Yinchuan   1786657
    http://api.openweathermap.org/data/2.5/weather?q=
    Nemuro   2128975
    http://api.openweathermap.org/data/2.5/weather?q=
    Marathon   5113792
    http://api.openweathermap.org/data/2.5/weather?q=
    Vidalia   4344599
    http://api.openweathermap.org/data/2.5/weather?q=
    New Port Richey   4165869
    http://api.openweathermap.org/data/2.5/weather?q=
    Mount Darwin   885800
    http://api.openweathermap.org/data/2.5/weather?q=
    Pellerd   3046499
    http://api.openweathermap.org/data/2.5/weather?q=
    Atuona   4020109
    http://api.openweathermap.org/data/2.5/weather?q=
    Babayurt   579999
    http://api.openweathermap.org/data/2.5/weather?q=
    Miloslavskoye   526543
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Filipe   3374210
    http://api.openweathermap.org/data/2.5/weather?q=
    Kerchevskiy   551047
    http://api.openweathermap.org/data/2.5/weather?q=
    Coquimbo   3893629
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Thenzawl   1254609
    http://api.openweathermap.org/data/2.5/weather?q=
    Tura   2014833
    http://api.openweathermap.org/data/2.5/weather?q=
    Labytnangi   1500933
    http://api.openweathermap.org/data/2.5/weather?q=
    Xichang   1789647
    http://api.openweathermap.org/data/2.5/weather?q=
    Freeport   4893171
    http://api.openweathermap.org/data/2.5/weather?q=
    Vardo   777019
    http://api.openweathermap.org/data/2.5/weather?q=
    Arraial do Cabo   3471451
    http://api.openweathermap.org/data/2.5/weather?q=
    Salalah   286621
    http://api.openweathermap.org/data/2.5/weather?q=
    Hirara   1862505
    http://api.openweathermap.org/data/2.5/weather?q=
    Sao Filipe   3374210
    http://api.openweathermap.org/data/2.5/weather?q=
    Mao   2428394
    http://api.openweathermap.org/data/2.5/weather?q=
    Jamestown   3370903
    http://api.openweathermap.org/data/2.5/weather?q=
    Hithadhoo   1282256
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Nantucket   4944903
    http://api.openweathermap.org/data/2.5/weather?q=
    Weyburn   6179652
    http://api.openweathermap.org/data/2.5/weather?q=
    Mufumbwe   905382
    http://api.openweathermap.org/data/2.5/weather?q=
    Shingu   1847947
    http://api.openweathermap.org/data/2.5/weather?q=
    Barcelos   3665098
    http://api.openweathermap.org/data/2.5/weather?q=
    Mutis   3689325
    http://api.openweathermap.org/data/2.5/weather?q=
    Batouri   2234663
    http://api.openweathermap.org/data/2.5/weather?q=
    Mailsi   1171502
    http://api.openweathermap.org/data/2.5/weather?q=
    Ilulissat   3423146
    http://api.openweathermap.org/data/2.5/weather?q=
    Trairi   3386177
    http://api.openweathermap.org/data/2.5/weather?q=
    Pangnirtung   6096551
    http://api.openweathermap.org/data/2.5/weather?q=
    Sumenep   1626099
    http://api.openweathermap.org/data/2.5/weather?q=
    Rundu   3353383
    http://api.openweathermap.org/data/2.5/weather?q=
    Benghazi   88319
    http://api.openweathermap.org/data/2.5/weather?q=
    Ribeira Grande   3372707
    http://api.openweathermap.org/data/2.5/weather?q=
    Kijang   1640044
    http://api.openweathermap.org/data/2.5/weather?q=
    Thompson   6165406
    http://api.openweathermap.org/data/2.5/weather?q=
    Trinidad   3534915
    http://api.openweathermap.org/data/2.5/weather?q=
    Mana   3381041
    http://api.openweathermap.org/data/2.5/weather?q=
    Belmonte   3448454
    http://api.openweathermap.org/data/2.5/weather?q=
    Berlevag   780687
    http://api.openweathermap.org/data/2.5/weather?q=
    Tabou   2281120
    http://api.openweathermap.org/data/2.5/weather?q=
    Belle Fourche   5762718
    http://api.openweathermap.org/data/2.5/weather?q=
    Bambous Virieux   1106677
    http://api.openweathermap.org/data/2.5/weather?q=
    Ust-Tsilma   477940
    http://api.openweathermap.org/data/2.5/weather?q=
    Tymovskoye   2120334
    http://api.openweathermap.org/data/2.5/weather?q=
    Bethlehem   1019704
    http://api.openweathermap.org/data/2.5/weather?q=
    Cap Malheureux   934649
    http://api.openweathermap.org/data/2.5/weather?q=
    Bonavista   5905393
    http://api.openweathermap.org/data/2.5/weather?q=
    


```python
# Create DataFrame from API-built lists
weather_df = pd.DataFrame({"ID":city_ids,
                           "City":found_cities,
                           "Country":found_countries,
                           "Latitude":found_lats,
                           "Temperature (F)":temps,
                           "Humidity (%)":humids,
                           "Cloudiness (%)":clouds,
                           "Wind Speed (MPH)":winds})

# Re-order columns for readability
weather_df = weather_df[["ID","City","Country","Latitude","Temperature (F)","Cloudiness (%)","Humidity (%)","Wind Speed (MPH)"]]
weather_df.head()
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
      <th>ID</th>
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Temperature (F)</th>
      <th>Cloudiness (%)</th>
      <th>Humidity (%)</th>
      <th>Wind Speed (MPH)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1267390</td>
      <td>Kavaratti</td>
      <td>IN</td>
      <td>10.57</td>
      <td>83.2316</td>
      <td>0</td>
      <td>100</td>
      <td>2.147464</td>
    </tr>
    <tr>
      <th>1</th>
      <td>49747</td>
      <td>Xuddur</td>
      <td>SO</td>
      <td>4.12</td>
      <td>71.9816</td>
      <td>0</td>
      <td>54</td>
      <td>10.647843</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2075265</td>
      <td>Busselton</td>
      <td>AU</td>
      <td>-33.64</td>
      <td>62.8916</td>
      <td>0</td>
      <td>100</td>
      <td>11.318926</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2411397</td>
      <td>Georgetown</td>
      <td>SH</td>
      <td>-7.93</td>
      <td>78.7766</td>
      <td>24</td>
      <td>100</td>
      <td>10.983384</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3370903</td>
      <td>Jamestown</td>
      <td>SH</td>
      <td>-15.94</td>
      <td>73.5116</td>
      <td>92</td>
      <td>100</td>
      <td>10.759690</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find the number of unique cities pulled
len(weather_df["ID"].unique())
```




    514




```python
# Remove duplicate entries and save to CSV
weather_df.drop_duplicates("ID",inplace=True)
weather_df.to_csv("Weather by Latitude.csv")
```

# Temperature (F) vs. Latitude


```python
plt.scatter(weather_df["Latitude"],weather_df["Temperature (F)"])
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.title("Current Temperature (F) vs. Latitude")
plt.savefig("Temp v Latitude.png")
plt.show()
```


![png](output_10_0.png)


# Humidity (%) vs. Latitude


```python
plt.scatter(weather_df["Latitude"],weather_df["Humidity (%)"])
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.title("Current Humidity (%) vs. Latitude")
plt.savefig("Humidity v Latitude.png")
plt.show()
```


![png](output_12_0.png)


# Cloudiness (%) vs. Latitude


```python
plt.scatter(weather_df["Latitude"],weather_df["Cloudiness (%)"])
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.title("Cloudiness (%) vs. Latitude")
plt.savefig("Clouds v Latitude.png")
plt.show()
```


![png](output_14_0.png)


# Wind Speed (mph) vs. Latitude


```python
plt.scatter(weather_df["Latitude"],weather_df["Wind Speed (MPH)"])
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (MPH)")
plt.title("Wind Speed (MPH) vs. Latitude")
plt.savefig("Wind v Latitude.png")
plt.show()
```


![png](output_16_0.png)

