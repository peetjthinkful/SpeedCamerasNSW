## SpeedCamerasNSW

Uses the Google Store Locator Api, more specifically the jQuery plugin by Bjorn Holine to create a useful experience.

###jQuery Store Locator Plugin

####How it works
User searches for a location and the following code is executed in the jquery.storelocator.js file.

	function get_form_values(e)
		- Handles form submission and calls begin_mapping();

		- Note that if you have a <select> control with id='maxdistance' on
		  your page, you can set the maxdistance allowed from a search.
	
	function begin_mapping()
		- Get user submitted value out of the #address element	
		- If no value entered goto start()

		function start()
			- Check for a default location which you can pass in the options
			- Call mapping() based on a default or fullmap 
			- OR do HTML5 geocoding
	
		- Do the geocoding to find the lat/lng and call mapping()
		- mapping()
			- 60% of the code - does everything here.
			- Results are sorted numerically by distance
			- If you are using 'featuredLocations' they are processed here
			- Distance unit is set to 'miles' which is a little weird
			
			- Now the map is created...
			- Slide in the map container based on the option
			- Open a modal window as per the options
			- Set up the map options: zoom, center, mapType
			- Create the map:
			
			var map = new google.maps.Map(document.getElementById(settings.mapDiv),myOptions);

			- Adds an origin marker (a blue map icon at the point of origin) as per the options.

			674 - Adds the markers!

			function create_location_variables()
				- Get the object from the locationset array
				- Create a locationData object which contains all the info for the particualar location.
				- Extend the locationData object with:
					- markerId, marker, length and origin eg. 0, 'A', 'miles', 'Edina,MN'
					- return 'locations' which is the extended object in an array (see my example below):

						{
						    "location": [{
						        "id": "8",
						        "name": "Chipotle Edina",
						        "lat": "44.879826",
						        "lng": "-93.321280",
						        "address": "6801 York Avenue South",
						        "address2": "",
						        "city": "Edina",
						        "state": "MN",
						        "postal": "55435",
						        "phone": "952-926-6651",
						        "web": "www.chipotle.com",
						        "hours1": "Mon-Sun 11am-10pm",
						        "hours2": "",
						        "hours3": "",
						        "distance": 1.56,
						        "markerid": 0,
						        "marker": "A",
						        "length": "miles",
						        "origin": "Edina,MN"
						    }]
						}
			
			- Get the Mustache template HTML for the location object eg:

				<div class="loc-name">
				    Chipotle Edina
				</div>
				<div>
				    6801 York Avenue South
				</div>
				<div>
				    Edina, MN 55435
				</div>
				<div>
				    Mon-Sun 11am-10pm
				</div>
				<div>
				    952-926-6651
				</div>
				<div>
				    <a href="http://www.chipotle.com" target="_blank">www.chipotle.com</a>
				</div>

			- Create and append the location data HTML to the lefthand panel
