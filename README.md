# Api_Travelling_Salesmen_Problem








<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/blob/main/README.md">
  </a>

  <h3 align="center">Api_Travelling_Salesmen_Problem</h3>




<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
  * [Built With](#built-with)
* [Getting Started](#getting-started)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
* [Usage](#usage)

<!-- ABOUT THE PROJECT -->
## About The Project


This project is focused on Solving the one of the logistics problem of Travelling sales man, The following criteria are taken into considerations when it comes to solving this problem - "The list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city .

It is an NP-hard problem in combinatorial optimization, important in theoretical computer science and operations research.

The Purpose of the project is to travel to the all the cities in france consisting of population more than 75.000 with car and make sure to visit each city atleast one . The origin of the route should be from the north most of France to the southern end of the country . The end goal of this project is to -:

1) to record the amount of optimal distance travelled from north to south .
2) to record the optimal time.
3) to record optimal distance irrespective of the starting and ending point .




### Built With
Open source softwares that were used for this project -:

* [PowerBI Desktiop ](https://powerbi.microsoft.com)
* [Logisticslabs](http://logisticslab.org)




<!-- GETTING STARTED -->
## Getting Started



For this project we will generate real time coordinates (Latitude and Longitude) using Bingmaps Api and integrate as a custom made function on Powerdesktop Bi and use the function and invoke into our table created with country and cities which will generate coordinate based on the locations on the table . To solve the Travelling sales man problem we will be using logistics labs and use the coordinates that were previously generated on PowerDesktop Bi and set them manually on logistics labs and get the shortest route possible for multiple destinations .  The second part of the project is to generate Distance betweeen one city to another in the table created with Country and cities as a result give a difference in different units Km/Miles and also show different modes selected by the user (Drving, Bycling , Walking , Transit ) 


### Prerequisites

1. Get a free Bingmap API Key at [https://www.bingmapsportal.com](https://www.bingmapsportal.com)

2.  Get a free Googlemap API Key at [https://developers.google.com/maps/documentation/javascript/get-api-key](https://developers.google.com/maps/documentation/javascript/get-api-key)

3. Create a custom table on PowerDesktop Bi or download the existing one at [List of Cities in france with population >75000](https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem)

4. code to create a custom query called location that can be involked into the table 
```sh

let


FindLatLong= (location) =>

let
    Source = Xml.Tables(Web.Contents("http://dev.virtualearth.net/REST/v1/Locations/"&location&")?o=xml&key=Enter your Bingmap Api key")),
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Copyright", type text}, {"BrandLogoUri", type text}, {"StatusCode", Int64.Type}, {"StatusDescription", type text}, {"AuthenticationResultCode", type text}, {"TraceId", type text}}),
    ResourceSets = #"Changed Type"{0}[ResourceSets],
    ResourceSet = ResourceSets{0}[ResourceSet],
    #"Changed Type1" = Table.TransformColumnTypes(ResourceSet,{{"EstimatedTotal", Int64.Type}}),
    Resources = #"Changed Type1"{0}[Resources],
    #"Expanded Location" = Table.ExpandTableColumn(Resources, "Location", {"Name", "Point", "BoundingBox", "EntityType", "Address", "Confidence", "MatchCode", "GeocodePoint"}, {"Location.Name", "Location.Point", "Location.BoundingBox", "Location.EntityType", "Location.Address", "Location.Confidence", "Location.MatchCode", "Location.GeocodePoint"}),
    #"Location Point" = #"Expanded Location"{0}[Location.Point],
    #"Changed Type2" = Table.TransformColumnTypes(#"Location Point",{{"Latitude", type number}, {"Longitude", type number}})
in
    #"Changed Type2",
    Custom1 = FindLatLong
http://dev.virtualearth.net/REST/v1/Locations/?key=Jd5VDQizCgTIcXGVQxYr~3A9I5RCwiJhA-eOgDZYbiw~AuajSknY0A9ltbJZgzujBQuqwy0KerCRnZDdHGR9AdZ1GXULONc-TAWQunSHpc7N
  https://www.youtube.com/watch?v=6WafpWGvhao
```

5. Add New query for the google map api key 
```sh
"Your GOOGLE MAP API KEY" meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]
```

6. Add New Query to determine the distance between the origin of the city and the Distination of the cities . 
```sh
"driving" meta [IsParameterQuery=true, List={"driving", "walking", "bicycling", "transit"}, DefaultValue=..., Type="Text", IsParameterQueryRequired=true]
```
7. Add New Query to measure the unit between cities in Km/miles 
```sh
"metric" meta [IsParameterQuery=true, List={"metric", "imperial"}, DefaultValue=..., Type="Text", IsParameterQueryRequired=true]
```
8. Code to invoke the location , Google Map Api Key , Distance , Time query into custome pre-existed table
```sh
let
    Source = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("dVdLchs3EL2KSut2Ff6fZWzH8UJSVHIqG5cWIIUiJxlibJCjirzyHXyIlLLIJXiTnCQYcDj4zHClEuZ1oz8Pr5ufP19/8MatLdwb3+yv4RoToAwDwyr8M35737s/rf/a23T09vh62PbOXj/C5OLmpXMBQYBgBkioBfR0ctO0rf3v+4+Hrl+Z5q/cza3xezt8HqIBSSQgiS5bQrLrV7bfFCH91vVt1+9ProhEIAVasJhOfjetdevGOmf3RWqzSzFgIYBxcsE4Fa/rTRNMb6wrXL7t/JM1/eiLK2AUX7CaTn/aNZWXu2Z9Sg4hBhqzGXb6P4RuXWFq3CEGqrQEiWkNTRW3H82zL+r66eDNftX1fhNgUihQebvPBtPBO1Ne/XCukdAImJAVMnN0a8p8f/HWdavIDSF1uJbO0KkCbmN9YX1OTIRyE8VnUKgCrJkUbXFgEpUzbCK7t/tDQejOHb7Ytm2sDyguNGguanzKovOhcYWDigycIdAKz02mg0+mcYc3d+abaaq+DR+ufj5Ekg6uwnNXFF2yTDeYh269rZ9WrBhTFCSjc+x0cN814cKyE+9a63ehLm8+WB9OngZHQ2EIWbCD8s6Sw+uXwZYyYFLXSMhwh7Kfv/r2+HqiCyMUMKEzNMyweQInojIkQFC6gE1pRGXNTCeyMURBSF5DUxK+eymjvrWHb+E7VRokQjNkxslmVxi+b/6I7KVSAteoRiZH26Zzz01bKFVSbxqeepHuhIcixMy6UsXBA9KswkPZ0Fx57dX4qiljoDStsYm6oyaVQnPKjmIJFOMFcIqjb7dxVOSZj0+ThoGmGFsAp5NA5tXxtW1y+3vrvzQbZ2LlUaC3IEsWSQrs3rjj310hlpOQEz3QBS/BoexzWb+zGJMgtirv/ZkUZ221zj6bIn8TRnpsPFEKKNczcKagzpady2pEFANJ9QydZH9rdqEWvmx9s+s2kTREEdB55gmfAjpPhlwejv/uTg64DmuNWkAnNg2TNH8x2dJDeJiQklbgXGzasuqZdBHOg8TiGTrbQk77TlH4rLOEiUA+vGBQUGmYMEUIpo/G6EpxuQCFqkiFQI3dISSsJvmzmQq6OE9yecwahBUGzuklqzQ4XsoiThTDgoenQytkIsM4S2aDLptjmCnAeREnI6jpls+4s6xiSsPCIObgfIMIfT3+s5trX7w/rLE83xoSPj3n80aYRzDq62kbpgR4vrZkO2TW9fwJmas4jEd7EjZWyks01MHm5tNigYMCCiTn6JLWlX5mCzYeflgIvYBP2YxqU47KU5WiCxT2zPx3SZKnSm/r7SvKrPFP0YkEHUXwro+vIP55fPwf", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [Location = _t, Population = _t, Origin = _t, Destination = _t]),
    #"Invoked Custom Function" = Table.AddColumn(Source, "geocode", each geocode([Location])),
    #"Expanded geocode" = Table.ExpandTableColumn(#"Invoked Custom Function", "geocode", {"Latitude", "Longitude"}, {"geocode.Latitude", "geocode.Longitude"}),
    #"Invoked Custom Function1" = Table.AddColumn(#"Expanded geocode", "Time and Distance", each #"Time and Distance"([Origin], [Destination], "Metrics", "Driving")),
    #"Expanded Time and Distance" = Table.ExpandTableColumn(#"Invoked Custom Function1", "Time and Distance", {"Distance", "Duration"}, {"Time and Distance.Distance", "Time and Distance.Duration"})
in
    #"Expanded Time and Distance"
```


### Installation

Create and Generate Bingmap API
1. Get a free Bingmap API Key at [https://www.bingmapsportal.com](https://www.bingmapsportal.com)

### create custom function in PowerBi and integrate it to the custom table and map.
1. Download and Install PowerDesktop Bi
2. Create a custom table in PowerDesktop Bi 
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
<p align="center">
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Power-Bi-UI.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Power-Bi-UI.png" alt="Logo" width="1000" height="700">
  </a>
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
   <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Power-BI-CustomData.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Power-BI-CustomData.png" alt="Logo" width="1000" height="700">
  </a>
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
  </p>
3. Create a new query on PowerDesktop Bi with a websource 
4. Enter your Bing API 
```sh
'ENTER YOUR API';
```
4. Paste the URL in websource and press ok and Keep clicking on the Table link and drilling through the record set until you reach where you can see the address and the attributes. Click Table under Point. 

<p align="center">
  
   <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Webquery-option.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Webquery-option.png" alt="Logo" width="1000" height="700">
  </a>
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Webquery-bingapi.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Webquery-bingapi.png" alt="Logo" width="1000" height="700">
  </a>
  
  
  </p>

5. Now we need to use this query to create a dynamic function that we can use to look up the point information for each address. Click the Advanced Editor button in the Home ribbon. 
6. When you click Done you should see your function in the Query Editor window.
7. Press right click on the Query Editor Window add the queries  -  Location , Google Map Api key (GMAPIKey) , Distance , Unit Query as show in the codes.

<p align="center">
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Geo-code-function-query.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Geo-code-function-query.png" alt="Logo" width="1000" height="700">
  </a>
  
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/GMAPIKey-Parameter-Query.png">
    <img src="TSP-IMG/Microsoft-PowerBI/GMAPIKey-Parameter-Query.png" alt="Logo" width="1000" height="700">
  </a>
  
   <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Mode-Parameter-Query.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Mode-Parameter-Query.png" alt="Logo" width="1000" height="700">
  </a>
  
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Units-Parameter-query.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Units-Parameter-query.png" alt="Logo" width="1000" height="700">
  </a>
  
  </p>


8. Invoke custom query Location to generate the Latitude and longitude of the cities in location coloumn.

<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/InvokeCustomFunction-Location.png">
    <img src="TSP-IMG/Microsoft-PowerBI/InvokeCustomFunction-Location.png" alt="Logo" width="1000" height="700">
  </a>
  
  </p>
  
9. Save the table in Excel Format with latitude and longitude generated as we require them in Logistics Labs.


<p align="center">
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Copy-table.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Copy-table.png" alt="Logo" width="1000" height="700">
  </a>
  
  </p>


### Use the coordinates to find the optimal distance and optimal route in Logistics labs 

10. Download and Install Logistics labs at [Logisticslabs](http://logisticslab.org)
11. Enter the coordinates manually as shown in the figure below or download the TSP file and open in the logistics labs [Coordinates of cities ](https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem)

<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logistics-labs-openproblem.png">
    <img src="TSP-IMG/Logistics-labs/Logistics-labs-openproblem.png" alt="Logo" width="1000" height="700">
  </a>
  
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logistics-labs-nodesdefined.png">
    <img src="TSP-IMG/Logistics-labs/Logistics-labs-nodesdefined.png" alt="Logo" width="1000" height="700">
  </a>
  
  </p>
  
12. Once the file is open click on optimisation tab calculate distance matrix and choose 1.15 Detour factor in Great Distance circle and click on Start.

<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logisticslabs-optimisationtab.png">
    <img src="TSP-IMG/Logistics-labs/Logisticslabs-optimisationtab.png" alt="Logo" width="1000" height="700">
  </a>
  
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logistics-labs-Distance-matrix-UI.png">
    <img src="TSP-IMG/Logistics-labs/Logistics-labs-Distance-matrix-UI.png" alt="Logo" width="1000" height="700">
  </a>
  
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logistics-labs-DistanceMatrix-Table.png">
    <img src="TSP-IMG/Logistics-labs/Logistics-labs-DistanceMatrix-Table.png" alt="Logo" width="1000" height="700">
  </a>
  
  
  </p>

13. Click on Optimisation tab again and choose * Start optimisation * , Click on Problem TSP according to Start and Ending point and click on Start .

<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logisticslabs-North%26South-Optimisation-Ui.png">
    <img src="TSP-IMG/Logistics-labs/Logisticslabs-North&South-Optimisation-Ui.png" alt="Logo" width="1000" height="700">
  </a>
  
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logisticslabs-North%26South-Optimal-Solved.png">
    <img src="TSP-IMG/Logistics-labs/Logisticslabs-North&South-Optimal-Solved.png" alt="Logo" width="1000" height="700">
  </a>
  
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Logistics-labs/Logistislabs-North%26South-Solved-Map.png">
    <img src="TSP-IMG/Logistics-labs/Logistislabs-North&South-Solved-Map.png" alt="Logo" width="1000" height="700">
  </a>
  
  
  </p>

14. Go To Existing Excel Table and add Origin and Destination as Starting and Ending points from Logistics labs .  


<p align="center">
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Origindes-excel.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Origindes-excel.png" alt="Logo" width="1000" height="700">
  </a>
  
  </p>

15. Go to Microsft Power BI open the existing file or Copy paste the Origin and Destional coloumn from Excel table into the existing file . 
<p align="center">
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/Copied-Data-ExceltoBI.png">
    <img src="TSP-IMG/Microsoft-PowerBI/Copied-Data-ExceltoBI.png" alt="Logo" width="1000" height="700">
  </a>
  
  </p>

16. Once the queries are added next step is to to invoke all the custom queries in to the existing queries . 

  <p align="center">
  <a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/InvokeCustomfunction-TimeandDistance.png">
    <img src="TSP-IMG/Microsoft-PowerBI/InvokeCustomfunction-TimeandDistance.png" alt="Logo" width="1000" height="700">
  </a>
  
  
  </p>

17. The  Google Map Api key (GMAPI) , Distance , Unit Query must be invoked in to the coloumn Origin and Destination to generate Distance required to travel between origin and Destination and time taken between them .

<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/France-North%26South-Query.png">
    <img src="TSP-IMG/Microsoft-PowerBI/France-North&South-Query.png" alt="Logo" width="1000" height="700">
  </a>
  </p>


18. The new table will have coloumns Latitude , Longitutude , Distance between origin and Destination based on the mode choosen and unit in Kilometer or miles .
<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/France-North%26South-Table.png">
    <img src="TSP-IMG/Microsoft-PowerBI/France-North&South-Table.png" alt="Logo" width="1000" height="700">
  </a>
  </p>

19.This is an extra step but to display the cities in flow map , Go to visualization and select Flow map and choose the filter Origin and Destination in the respective filter to show location from north to south as in [Coordinates of cities ](https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem)                                                        

<p align="center">
  
<a href="https://github.com/arsal-subhan/Api_Travelling_Salesmen_Problem/raw/main/TSP-IMG/Microsoft-PowerBI/France-North%26South-FlowMap.png">
    <img src="TSP-IMG/Microsoft-PowerBI/France-North&South-FlowMap.png" alt="Logo" width="1000" height="700">
  </a>
  </p>


<!-- USAGE EXAMPLES -->
## Usage

Will be updated soon 


