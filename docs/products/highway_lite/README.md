# Highway Lite

<span style="font-size:18px; color:#43494D">Looking for a nice and easy way to optimize your daily routes? **SmartMonkey Highway Lite - Route Optimizer** for Google Spreadsheet will save you lots of time calculating the routes. 
It’s easy, fast and reliable. </span>

|    Product  | Description     |
|-------------|-----------------|
|    Type  | Extension for Google Spreadsheet [Try it for free](https://gsuite.google.com/marketplace/app/smartmonkey_optimizador_de_rutas_para_go/80526361570) |
|    Pricing  | Freemium model. Free up to 10 stops and 1 vehicle. <br/>Check below for [Upgrade license options](#How-much-does-it-cost)     |
|    Last update  | Oct 4 2018     |
|    Contact  | [support@smartmonkey.io](mailto:support@smartmonkey.io)     |



## Free plan ##
The freemium service allow you to optimize routes for **10 stops and 1 vehicle**. 
In order to increase this limits, you can upgrade your account directly on the add-on, or contact us at: sales@smartmonkey.io

## Last Updates ##
* Optimized routes with different depot locations and destinations. Even with just one of them. You can define each start/ending point per each vehicle.
- Google Maps integration supported. Share Google maps link for following the route on your mobile phone.


** Try Highway Lite for free **. Free to use up to 10 vehicles.
Get it on Google Suite Marketplace.<br/>
<a href="https://gsuite.google.com/marketplace/app/smartmonkey_optimizador_de_rutas_para_go/80526361570" target="_blank" style="font: bold 18px Arial; color:white; padding: 4px 8px 4px 8px;background-color: #FF8200;font-decoration:none; margin:4px">Try it now</a>  

## Features ##
* **Address geocoder**: Add any stop address and Highway detects magically its latitude and longitude.
* **Multiple set of origins and destination** routes on each vehicle.
* **Map view**: Delivery stops are displayed on a map.
* **Time windows**: define the opening and closing times of each delivery stop to avoid visits during the closig times. 
* **Order capacity**: Define capacity of each client in weight and volume.
* **Multiple vehicle fleet**: define multiple vehicles in order to spread the visits. 
* **Fleet time windows**: Define the operating schedule of each vehicle.
* **Fleet capacity**: Define different capacities for each vehicle.
* **Real time route optimizer**:  Our technology will process all data and will show you the best route in seconds.
* **Google Maps route integration**

# User Guide #
* [Quick overview](#quick-overview) 
* [Initial setup](#initial-setup) 
* [Stops](#stops)
* [Fleet](#fleet)
* [Stop time](#stop-time)
* [Results](#results)


## Quick overview ##
This quick video demonstrates how easy it is to optimize your routes with SmartMonkey Highway Lite. Video available on [Youtube](https://www.youtube.com/watch?v=vb5sQwxtLmg).
<iframe width="560" height="315" src="https://www.youtube.com/embed/vb5sQwxtLmg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Initial Setup ##
To start using the Add-on please  install it first. Visit the [G Suite Marketplace](https://gsuite.google.com/marketplace) and look for SmartMonkey. Select the Add-on and press the button "LAUNCH". 

A new Spread Sheet opens with Highway Lite installed. 

Open the menu "Add-ons" to confirm that Highway is installed. If succesfully added, the SmartMonkey Add-on should appear on the list. 

**Granting rights for current Sheet**<br/>
If the menu item does have only one subitem "Help", it means that the Add-on has not enough rights to start.  
Press the button "Manage Add-ons" and on the popup press the button "Manage" which will expand a menu. Select "Use in this document".
This is needed for every new Sheet you start. 

Now you should be able to see the following menu. If not, please reload the page. 
![SmartMonkey Highway Lite Add-on menu](./images/add_on_menu.png)

## Stops ##
Press "Optimize route" on the submenu of Highway Lite. 
![SmartMonkey Highway Lite Add-on menu](./images/add_on_menu_filled.png)

When the Addon initiates it creates two tabs on the bottom: Stops and Fleet. 

The Stops Sheet is filled with sample data. Here you should fill in your stops addresses. 

On the right panel you'll find the button "Geocode" that will generate latitudes and longitudes for all the stops. 

If a Stop can not be located Highway Lite generates a new Sheet titled "Stops not found". See FAQs below for information on how to solve Geolocation issues. 

## Fleet ##
Jumping to the next Sheet "Fleet" you are asked to fill in the location of your route origin and destination. Usually it is the warehouse or office address. 

## Stop time ##
On the next step you can configure the average time needed for the stops. Default value is set to 5 minutes. 

## Results ##
After the optimization a new Sheet "Results" is created. It contains the optimized route based on all previous parameters. 

With the button "Visualize the routes on a map" you can view the route on a Google map. 



# Frequent Asked Questions (FAQs)
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
Highway Lite is marketed as a freemium model. You can use it anytime for free up to 10 stops and 1 vehicle.

If you need an advanced license press the button **Upgrade** to open the pricing models dialogue. Upgrade license includes 100 stops and 3 vehicles. You can decide between three options for the duration of the license:
* 1 day for just 4,99€ 
* 1 month for just 29,90€
* 1 year for just 299€

![pricing modalities](./images/pricing_payment.png)

If you have any questions or requests please contact us. 

If you add more than one vehicle or more than 100 stops, the system will display following dialogue:
![Limit achieved](./images/limit_achieved.png)

Press Yes to upgrade your license. 

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
