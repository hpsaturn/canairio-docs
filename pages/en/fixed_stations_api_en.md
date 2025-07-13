---
title: Fixed stations API
tags:
  - api
keywords: api, data, fixed stations
last_updated: "March 12, 2021"
summary: "CanAirIO fixed stations API for fetch published data"
series: "api"
sidebar: english_sidebar
permalink: fixed_stations_api_en.html
folder: en
---

## CanAirIO Unified API

We have two alternatives to retrieve stations data, using two output protocols:

## Basic Output

We have a basic REST API for our CanAirIO fixed stations, for now it only has two endpoints, the first one for get all current stations that are online right now with the last publication, `stations` and the second one for retrieve the complete data of each station in the last hour:

### Stations list

endpoint: [http://api.canair.io:8080/stations](http://api.canair.io:8080/stations)

Sample request for retreive all stations IDs and its last data:

```bash
curl -G http://api.canair.io:8080/stations
```

[sample output](http://influxdb.canair.io:8080/data/api_stations_list_output_sample.json)

Here, you should have a list of all stations and the last data of each one. The important thing here is the IDs, for in the next step, retrieve the all data of this station.

### Station data

endpoint: [http://api.canair.io:8080/stations/station_id](http://api.canair.io:8080/stations/station_id)

Request sample for station with id: U33TTGOTDA585E:  

```bash
curl -G http://api.canair.io:8080/stations/U33TTGOTDA585E
```

[sample output](http://influxdb.canair.io:8080/data/api_station_output_sample.json)

## DarwinCore Output

Similar than the basic output protocol endpoints, you are able to retrieve these endpoints using `dwc` API controller, to retrieve the data formatted unsing the [DarwinCore](https://dwc.tdwg.org/terms/) standard protocol, like this:

### Stations list

endpoint: [http://api.canair.io:8080/dwc/stations](http://api.canair.io:8080/dwc/stations)

Sample request for retreive all stations IDs and its last data:

```bash
curl -G http://api.canair.io:8080/dwc/stations
```

here you should have a list of all stations and the last data of each one. The important thing here is the IDs, for in the next step, retrieve the all data of this station. Here a reduced sample of this list:

```json
[{
	"id": "EZTTTGOTD78432",
	"station_name": "EZTTTGOTD78432",
	"scientificName": "CanAirIO Air quality Station",
	"ownerInstitutionCodeProperty": "CanAirIO",
	"type": "FixedStation",
	"license": "CC BY-NC-SA",
	"measurements": [
		[{
			"measurementID": "2022-03-15T16:29:07.913361Z",
			"measurementType": "PM2.5",
			"measurementUnit": "ug/m3",
			"measurementDeterminedDate ": "2022-03-15T16:29:07.913361Z",
			"measurementDeterminedBy": "CanAirIO station EZTTTGOTD78432",
			"measurementValue": 18
		}, {
			"measurementID": "2022-03-15T16:29:07.913361Z",
			"measurementType": "Geohash",
			"measurementUnit": "",
			"measurementDeterminedDate ": "2022-03-15T16:29:07.913361Z",
			"measurementDeterminedBy": "CanAirIO station EZTTTGOTD78432",
			"measurementValue": "ezty5zs"
		}, {
			"measurementID": "2022-03-15T16:29:07.913361Z",
			"measurementType": "Version",
			"measurementUnit": "",
			"measurementDeterminedDate ": "2022-03-15T16:29:07.913361Z",
			"measurementDeterminedBy": "CanAirIO station EZTTTGOTD78432",
			"measurementValue": "v0.5.2r907"
		}]
	],
	"locationID": "2022-03-15T16:29:07.913361Z",
	"georeferencedBy": "CanAirIO firmware v0.5.2r907",
	"georeferencedDate": "2022-03-15T16:29:07.913361Z",
	"decimalLatitude ": 43.28,
	"decimalLongitude ": -2.99,
	"geohash": "ezty5zs",
	"observedOn": "2022-03-15T16:29:07.913361Z"
}]
```

### Station data

endpoint: [http://api.canair.io:8080/dwc/stations/station_id](http://api.canair.io:8080/dwc/stations/station_id)

Request sample for station with id: U33TTGOTDA585E:  

```bash
curl -G http://api.canair.io:8080/dwc/stations/U33TTGOTDA585E
```

Sample output for this station:

```json
{
	"id": "U33TTGOTDA585E",
	"station_name": "U33TTGOTDA585E",
	"scientificName": "CanAirIO Air quality Station",
	"ownerInstitutionCodeProperty": "CanAirIO",
	"type": "FixedStation",
	"license": "CC BY-NC-SA",
	"measurements": [
		[{
			"measurementID": "2022-03-15T16:45:34.835070Z",
			"measurementType": "PM2.5",
			"measurementUnit": "ug/m3",
			"measurementDeterminedDate ": "2022-03-15T16:45:34.835070Z",
			"measurementDeterminedBy": "CanAirIO station U33TTGOTDA585E",
			"measurementValue": 5
		}, {
			"measurementID": "2022-03-15T16:45:34.835070Z",
			"measurementType": "Geohash",
			"measurementUnit": "",
			"measurementDeterminedDate ": "2022-03-15T16:45:34.835070Z",
			"measurementDeterminedBy": "CanAirIO station U33TTGOTDA585E",
			"measurementValue": "u33dcu0"
		}, {
			"measurementID": "2022-03-15T16:45:34.835070Z",
			"measurementType": "Version",
			"measurementUnit": "",
			"measurementDeterminedDate ": "2022-03-15T16:45:34.835070Z",
			"measurementDeterminedBy": "CanAirIO station U33TTGOTDA585E",
			"measurementValue": "v0.5.3r908"
		}],
		[{
			"measurementID": "2022-03-15T16:47:34.767315Z",
			"measurementType": "PM2.5",
			"measurementUnit": "ug/m3",
			"measurementDeterminedDate ": "2022-03-15T16:47:34.767315Z",
			"measurementDeterminedBy": "CanAirIO station U33TTGOTDA585E",
			"measurementValue": 5
		}, {
			"measurementID": "2022-03-15T16:47:34.767315Z",
			"measurementType": "Geohash",
			"measurementUnit": "",
			"measurementDeterminedDate ": "2022-03-15T16:47:34.767315Z",
			"measurementDeterminedBy": "CanAirIO station U33TTGOTDA585E",
			"measurementValue": "u33dcu0"
		}, {
			"measurementID": "2022-03-15T16:47:34.767315Z",
			"measurementType": "Version",
			"measurementUnit": "",
			"measurementDeterminedDate ": "2022-03-15T16:47:34.767315Z",
			"measurementDeterminedBy": "CanAirIO station U33TTGOTDA585E",
			"measurementValue": "v0.5.3r908"
		}]
	],
	"locationID": "2022-03-15T16:47:34.767315Z",
	"georeferencedBy": "CanAirIO firmware v0.5.3r908",
	"georeferencedDate": "2022-03-15T16:47:34.767315Z",
	"decimalLatitude ": 52.54,
	"decimalLongitude ": 13.44,
	"geohash": "u33dcu0",
	"observedOn": "2022-03-15T16:47:34.767315Z"
}
```

## Data visualization

The shared time series of each stations could be listed in our Grafana dashboard:

[CanAirIO grafana](https://grafana.canair.io)

[![Mobile track visualization](/docs/images/grafana_sample.jpg)](https://grafana.canair.io)

{% include links.html %}

