# weather app

## description

### weather app featured by sugestions search and descriptive icon and the most important thing is details

## API services

### \*fetching weather informatinsusing [darksky](https://darksky.net/dev)

### \*fetching latitude and longtude using [google maps API](https://maps.googleapis.com/maps/api/)

### \*adding suggestions features using [google places API](https://cloud.google.com/maps-platform/places/)

## main two files wraps the logic of the project

# server.js contains the set up for the back-end of the environment using express and the most important function inside it app.post

'''javascript
app.post('/weather', (req, res) => {
const url = `https://api.darksky.net/forecast/${DARKSKY_API_KEY}/${req.body.latitude},${req.body.longitude}?units=auto`
axios({
url: url,
responseType: 'json'
}).then(data => res.json(data.data.currently))
})
'''
it makes post request and get weather information in JSON format

# script.js and most stepes of getting weather information process is inside it

##main stepes getting the information

#getting the name of city with help of google places api

'''javascript
const searchElement = document.querySelector("[data-city-search]");
const searchBox = new google.maps.places.SearchBox(searchElement);
'''

#getting the longitude and latitude
'''javascript
const place = searchBox.getPlaces()[0];
if (place == null) return;
const latitude = place.geometry.location.lat();
const longitude = place.geometry.location.lng();
'''
#passing longitude and latitude in the body of fetch request
'''javascript
fetch("/weather", {
method: "POST",
headers: {
"Content-Type": "application/json",
Accept: "application/json"
},
body: JSON.stringify({
latitude: latitude,
longitude: longitude
})
})

'''
#fetch requests via app.post in server.js file
'''javascript
//server.js
app.post('/weather', (req, res) => {
const url = `https://api.darksky.net/forecast/${DARKSKY_API_KEY}/${req.body.latitude},${req.body.longitude}?units=auto`
axios({
url: url,
responseType: 'json'
}).then(data => res.json(data.data.currently))
})

'''
#getting the data in the returning response
'''javascript
.then(res => res.json())
'''
#passing data throw place.formatted_address);
'''javascript
.then(data => {
setWeatherData(data, place.formatted_address);
});
//
'''
#populate data in the DOM including the icon
'''javascript

const icon = new Skycons({ color: "#222" });
const locationElement = document.querySelector("[data-location]");
const statusElement = document.querySelector("[data-status]");
const temperatureElement = document.querySelector("[data-temperature]");
const precipitationElement = document.querySelector("[data-precipitation]");
const windElement = document.querySelector("[data-wind]");
icon.set("icon", "clear-day");
icon.play();

function setWeatherData(data, place) {
locationElement.textContent = place;
statusElement.textContent = data.summary;
temperatureElement.textContent = data.temperature;
precipitationElement.textContent = `${data.precipProbability * 100}%`;
windElement.textContent = data.windSpeed;
icon.set("icon", data.icon);
icon.play();
}
'''
