# Highway Lite FAQs

* [How do I get started?](#how-do-I-get-started) 
* [Which techhnology is used?](#technology-used) 
* [How do I solve Geolocation issues? ](#solve-geolocation-issues) 
* [How much does it cost?](#How-much-does-it-cost) 
* [How do I create a multiday route?](#How-do-I-create-a-multiday-route) 
* [How do I balance multiple vehicles?](#How-do-I-balance-multiple-vehicles) 

Do you have more questions? Raise your doubts/improvements at [support@smartmonkey.io ](mailto:support@smartmonkey.io)

## How do I get started
To get started using Highway Lite, our Google Spreadsheet plugin, watch the following video in English and with spanish Subtitles (Español)
<iframe width="560" height="315" src="https://www.youtube.com/embed/vb5sQwxtLmg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>  


## Technology Used
Highway Lite - Route optimizer uses the mighty technology [SmartMonkey API](developers/README) to optimize the routes.<br/> 
Highway Lite uses Google maps aswell to geolocate the stops.  

## Solve Geolocation issues 

Highway Life uses Google maps for geolocation. If Google maps finds more matches for one stop, it will select the first one but it does not know which is closest to the user. 

Watch a video on how to deal with Stops which could not be geolocated.

It explains as well how to manually select the latitude and longitude from the URL for an optimal geolocation. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/TCn2uVJchuc" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>  

## How much does it cost
See [pricing section](products/highway_lite/pricing.md) for more details.

## How do I create a multiday route
There is a manual way to create routes for multiple days (multiday route). 

This video explains it:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Sap-Dj5Ch74" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

On this examnple we follow these instructions. 

1) First of all, we enter the time windows of the destinations (sheet "Stops") and name the vehicle "joe_monday" (sheet Fleet). Since we know that it's impossible to visit all places in one day. 

2) Then we optimize the route and the system will assign the vehicle "joe_monday" to the stops (sheet "Stops") that can be visited on the first day. 

3) Then we can clean the "Stops" sheet from the monday stops. We filter them and delete them. 

4) Rename the "Results" seet to "Joe Monday". 

5) Then we change the ID of the vehicle in the "Fleet" Sheet and optimize again with the remaining Stops. 

6) A new sheet called "Results" appears with the Tuesday route optimized we can rename to "Joe Tuesday"

## How do I balance multiple vehicles?
You can add the Capacity restriction to the vehicles to balance the deliveries between vehicles. 

This video explains how to balance routes for several vehicles?: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/Ozsgi7dwmcI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Video available in Spanish (español):
¿Cómo balancear entregas para varios vehículos?
<iframe width="560" height="315" src="https://www.youtube.com/embed/jIRYGUFk7vs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>