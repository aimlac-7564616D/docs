# API Recon

<!--BEGIN TOC-->
## Table of Contents
1. [MetOffice](#metoffice)
    1. [Spot weather forecast](#spot-weather-forecast)
2. [AIMLAC Endpoint](#aimlac-endpoint)

<!--END TOC-->

## MetOffice <a id="toc-tag-mdtoc" name="metoffice"></a>

Overview of the MetOffice API. Full docs are
- [Atmospheric Model Data API](https://metoffice.apiconnect.ibmcloud.com/metoffice/production/product/17502/api/16908)
- [Spot Weather Forecast](https://metoffice.apiconnect.ibmcloud.com/metoffice/production/product/16935)

### Spot weather forecast <a id="toc-tag-mdtoc" name="spot-weather-forecast"></a>

Retrieve immediate and forecast weather data.

Request:
```py
headers = {
    "X-IBM-Client-Id": MET_API_KEY,
    "X-IBM-Client-Secret": MET_SECRET_KEY,
    "accept": "application/json"
}
request_example = requests.get(
    (   
        "https://rgw.5878-e94b1c46.eu-gb.apiconnect.appdomain.cloud"
        "/metoffice/production/v0/forecasts/point/hourly"
        f"?excludeParameterMetadata={false}"
        f"&includeLocationName={true}"
        f"&latitude={LAT}&longitude={LON}"
    ), headers=headers
).json()

request_example.keys()
# dict_keys(['type', 'features', 'parameters'])

request_example["type"]
# 'FeatureCollection'

request_example["features"][0].keys()
# dict_keys(['type', 'geometry', 'properties'])

request_example["features"][0]["properties"].keys()
# dict_keys(['location', 'requestPointDistance', 'modelRunDate', 'timeSeries'])
```

The `timeSeries` gives *weather predictions*:
```
{'time': '2022-01-23T20:00Z',
'screenTemperature': 1.56,
'maxScreenAirTemp': 1.77,
'minScreenAirTemp': 1.56,
'screenDewPointTemperature': 0.62,
'feelsLikeTemperature': 0.08,
'windSpeed10m': 1.2,
'windDirectionFrom10m': 182,
'windGustSpeed10m': 2.46,
'max10mWindGust': 2.46,
'visibility': 10288,
'screenRelativeHumidity': 94.24,
'mslp': 103370,
'uvIndex': 0,
'significantWeatherCode': 8,
'precipitationRate': 0,
'totalPrecipAmount': 0,
'totalSnowAmount': 0,
'probOfPrecipitation': 13}
```

Description of each parameter given as a list of dictionaries in
```py
request_example["parameters"]
```
For example
```
'mslp': {'type': 'Parameter',
   'description': 'Mean Sea Level Pressure',
   'unit': {'label': 'pascals',
    'symbol': {'value': 'http://www.opengis.net/def/uom/UCUM/',
     'type': 'Pa'}}},
```

Note that including `excludeParameterMetadata=true` in the request omits the `parameters` field.

## AIMLAC Endpoint <a id="toc-tag-mdtoc" name="aimlac-endpoint"></a>

Request:
```py
requests.get(f"http://{AIMLAC_RSE_ADDR}/sim/llanwrtyd-wells").json()
```
Return:
```
{'elements': {'AIMLAC HQ Llanwrtyd Wells': {'power': -20000.0,
   'temperature': 12.13},
  'Llanwrtyd Wells - Computing Centre': {'power': -200000.0},
  'Llanwrtyd Wells - Solar Generator': {'power': 0.0},
  'Llanwrtyd Wells - Wind Generator 1': {'power': 92.0},
  'Llanwrtyd Wells - Wind Generator 2': {'power': 40.0},
  'Llanwrtyd Wells - Wind Generator 3': {'power': 92.0},
  'Llanwrtyd Wells - Wind Generator 4': {'power': 92.0},
  'Llanwrtyd Wells - Wind Generator A': {'power': 92.0},
  'Llanwrtyd Wells - Wind Generator B': {'power': 92.0}},
 'timedate': 'Fri, 21 Jan 2022 19:29:22 GMT'}
```

Request:
```py
parameters = {"year":2022, "month":1, "day":1, "hour":13, "minute":00}

requests.get(
    f"http://{AIMLAC_RSE_ADDR}/sim/llanwrtyd-wells/30m-average-power", 
    params=parameters
).json()
```
Return:
```
{'average power': 114378.39}
```
