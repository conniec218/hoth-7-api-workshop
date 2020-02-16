# HOTH 7 API workshop

# Link to the slides
https://tinyurl.com/apislides

In this demo, you will learn how to use a JSON object and integrate it into a simple webpage that uses Google Maps API. 

Let's begin :)

# JSON 
Now what exactly is JSON? JSON stands for JavaScript Object Notation. What this does is allow us to send and receive data in a way that both the server and user can use. JSON works for any programming language, making it super versatile and useful. 

The way JSON works is by storing an object's data in a string. In our JSON file, the images are stored in an array of JSON objects. As we can see in the code sample below, JSON can store whatever data we give it. 

```
[
{ 
  "title":"UCLA",
  "icon":"http://www.free-icons-download.net/images/cute-small-star-icon-34614.png",
  "position": {"lat":34.068921,"lng":-118.4473698},
  "image":"https://www.wallpaperup.com/uploads/wallpapers/2013/10/19/162678/a76e1c05f55d7df97fa1c5d4d218951a-700.jpg",
  "link":"http://my.ucla.edu"
},
{ 
  "title":"The Getty",
  "icon":"http://cdn.shopify.com/s/files/1/1061/1924/products/Heart_Eyes_Emoji_grande.png?v=1480481053",
  "position":{"lat":34.0677051,"lng":-118.4670609},
  "image":"https://www.cesarsway.com/sites/newcesarsway/files/styles/large_article_preview/public/Natural-Dog-Law-2-To-dogs%2C-energy-  is-everything.jpg?itok=Z-ujUOUrg",
  "link":"http://www.getty.edu/"
 },

... more images loaded in ...
]

```

As we can see above, the JSON contains a string version of a list of parameters that each store a value. For those familiar with JavaScript, you'll notice that the format of this is similar to the way Objects are declared in JavaScript. Later on, we'll be accessing these values when using the Google Maps API. 

BUT FIRST....How do we even access this JSON data??

# Retrieving our JSON
First we will write a function that loads in our JSON. Here is the code:

``` js
async function getIcons() {
  const response = await fetch('https://api.myjson.com/bins/bd6u2'); // <-- here we are retrieving data the server that stores our json
  const icons = await response.json();
  console.log(icons);
  return icons;
}
```

Now we must put put this function in our project so that we can use it later to load these images and their data. 

To do this, we simply insert this function within the <script></script> html elements.

```js
<script>
async function getIcons() {
  const response = await fetch('https://api.myjson.com/bins/bd6u2'); // <-- here we are retrieving data from our link
  const icons = await response.json();
  console.log(icons);
  return icons;
}
</script>
```
Now this is ready to be used in our project :)

# (Optional) Create your own JSON and upload it in a server

To create your own JSON object and store it in a server for free, you can go to the link here: http://myjson.com/

# Get a Google Maps API Key

In order to use Google's API, you must have an API Key, which grants you access to the API. You can get one for free by going to this link: https://developers.google.com/maps/documentation/javascript/get-api-key

You do need to register with a credit card (Google just wants to know you're not a robot), but it is free (Google lists the details of billing when you register). 

However, for now, just use the key we will be providing you with. 

# Google Maps API Demo
Now let's get to the real content: API!

In this demo we will be using the Google Maps API to load Google Maps into our personal website and add personalized icons and popups using our JSON. 

First let's add some basic code to our HTML file. 

``` html
<!DOCTYPE html>
<html>
<style>
h1, h2 {
  font-family: "Comic Sans MS";
  text-align: center;
}

p {
  font-size: 70%;
}

</style>
<body>

<h1>Places to Visit in LA! :)</h1>
<h2> Using <em>Google Maps API</em> woohoo </h2>

<script>
//this function will load in our json that stores our images and the information about them
async function getIcons() {
  const response = await fetch('https://api.myjson.com/bins/bd6u2');
  const icons = await response.json();
  console.log(icons);
  return icons;
}
</script>

  <!--This code loads in our map -->
<div id="map" style="width:100%;height:400px;"></div>

<script>
// this function will create our map and center it at myLatLng
function myMap() {

  //Location of Map
  const myLatLng = {lat: 34.068921, lng: -118.4473698};

//Map
  const map = new google.maps.Map(document.getElementById('map'), {
    zoom: 12,
    center: myLatLng
  });
}
  
<script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>
</body>
</html>
```
In the code above, we load in Google Maps' API using the line, replacing OUR_API_KEY with our personal API key. 

```html
<script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>
```

The function ```myMap()``` loads our map by creating a new Map Object, which is centered at the latitude and longitude stored in myLatLng and has a zoom: 12, which zooms us in at that point.  

Now, when we open this file in the browser, we can see Google Maps embedded in our webpage! 


Now let's add in some personal icons.

Remember our JSON object? We'll be using that to load in some icons. Here's our code: 

```js
async function setMarkers(map) {
  
  //this loads in our icons yeet
  const icons = await getIcons();

  for (const myicon of icons){
    const marker = new google.maps.Marker({
      //assign the json data to each marker
      position: myicon.position,
      map: map,
      title: myicon.title,
      icon: {
        url: myicon.icon,
        scaledSize: {
          width: 50,
          height: 50
        }
      }
    });
  }
}
```
Remember how in our JSON we had elements called 'position', 'title', and 'icon'? Well we are accessing those elements here in the code with ```myicon.position```, ```myicon.title```, and ```myicon.icon```.

To display our icons in our map, we must call the ```setMarkers(map)``` function in our ```myMap()``` function. 

```html
<!DOCTYPE html>
<html>
<style>
h1, h2 {
  font-family: "Comic Sans MS";
  text-align: center;
}

p {
  font-size: 70%;
}

</style>
<body>

<h1>Places to Visit in LA! :)</h1>
<h2> Using <em>Google Maps API</em> woohoo </h2>


<script>
//this function will load in our json that stores our images and the information about them
async function getIcons() {
  const response = await fetch('https://api.myjson.com/bins/bd6u2');
  const icons = await response.json();
  console.log(icons);
  return icons;
}
</script>

<div id="map" style="width:100%;height:400px;"></div>

<script>
function myMap() {

  const myLatLng = {lat: 34.068921, lng: -118.4473698};

  const map = new google.maps.Map(document.getElementById('map'), {
    zoom: 12,
    center: myLatLng
  });
  
//we call our setMarkers(map) function here
  setMarkers(map);
}


//This function is reponsible for drawing our markers and making their infoWindows
async function setMarkers(map) {

  const icons = await getIcons();

  for (const myicon of icons){
    const marker = new google.maps.Marker({
      //assign the json data to each marker
      position: myicon.position,
      map: map,
      title: myicon.title,
      icon: {
        url: myicon.icon,
        scaledSize: {
          width: 50,
          height: 50
        }
      }
    });
  }

}
</script>
<script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>
</body>
</html>
```

Ya yeet :) Now we have personalized icons in our map! As we can see, we have have an icon that is centered at a specified longitude and latitude, which we stored in our JSON. 

Now let's add more functionality to our icons. Let's add a features so that when we click it, we get a pop up box which will give us a link to another website for more information about that place. 

To do this, we will want to use another object from Google Maps' API: an ```InfoWindow```.

So what is an info window? An info window is what pops on when you click on an icon to get more information about it. Normally info windows display the name and address of a location. But what if we want to personalize info windows for our personalized icons? The API allows us to do this! 

To load this into our project, we will store this a new variable we'll call ```infowindow``` . Our call looks like this:

``` const infowindow = new google.maps.InfoWindow(); ```
    
We will also need to add a function that will detect whenever we click on an icon. We will add this code inside our icon for-loop: 
```js
    const popUpBox = `
      <div>
        <div>HELLO BONJOUR NIHAO ANYEONG FROM ${myicon.title}</div>
        <p> (click image for more info) </p>
        <a href="${myicon.link}">
          <img width=70% height=70% src="${myicon.image}">
        </a>
      </div>
    `;

    infowindow.setContent(popUpBox);
    infowindow.setOptions({maxWidth: 150});
      
    google.maps.event.addListener (marker, 'click', function () {

       infowindow.open(map, this);
    });
```
What this does is store the contents of our InfoWindow in ```popUpBox```, which we pass to the function ```infowindow.setContent(popUpBox)```. This formats our InfoWindow to display our desired content. 

Our final product should now match the code displayed below. When we test this, a user should be able to click an icon and see an InfoWindow pop up. When they click on the image in the InfoWindow, they will be redirected to a site where they can learn more about that location. 


```html 
<!DOCTYPE html>
<html>
<style>
h1, h2 {
  font-family: "Comic Sans MS";
  text-align: center;
}

p {
  font-size: 70%;
}

</style>
<body>

<h1>Places to Visit in LA! :)</h1>
<h2> Using <em>Google Maps API</em> woohoo </h2>


<script>
//this function will load in our json that stores our images and the information about them
async function getIcons() {
  const response = await fetch('https://api.myjson.com/bins/bd6u2');
  const icons = await response.json();
  console.log(icons);
  return icons;
}
</script>

<div id="map" style="width:100%;height:400px;"></div>

<script>

// this function will create our map and center it at myLatLng
function myMap() {

  //Location of Map
  const myLatLng = {lat: 34.068921, lng: -118.4473698};

//Map
  const map = new google.maps.Map(document.getElementById('map'), {
    zoom: 12,
    center: myLatLng
  });

//this calls our function which is responsible for drawing our markers

  setMarkers(map);
}


//This function is reponsible for drawing our markers and making their infoWindows
async function setMarkers(map) {

  const icons = await getIcons();

  for (const myicon of icons){
    const marker = new google.maps.Marker({
      //assign the json data to each marker
      position: myicon.position,
      map: map,
      title: myicon.title,
      icon: {
        url: myicon.icon,
        scaledSize: {
          width: 50,
          height: 50
        }
      }
    });

    // This creates a pop up for when we click an icon
    const infowindow = new google.maps.InfoWindow();
    console.log(myicon);
    const popUpBox = `
      <div>
        <div>HELLO BONJOUR NIHAO ANYEONG FROM ${myicon.title}</div>
        <p> (click image for more info) </p>
        <a href="${myicon.link}">
          <img width=70% height=70% src="${myicon.image}">
        </a>
      </div>
    `;
  
      infowindow.setContent(popUpBox);
      infowindow.setOptions({maxWidth: 150});

      //This adds a listener, which allows the website to know when an icon is being clicked
    google.maps.event.addListener (marker, 'click', function () {

       infowindow.open(map, this);
    });
  }

}
</script>

<script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>


</body>
</html>
```

And that's all :) As we can see, our job is made A LOT easier by using Google Maps' API. Instead of hardcoding everything ourselves, by using Google's API, we were able to include all the functionality of Google Maps into simple, readable code. 


