# HOTH 7 API workshop

**Date** Feb 23rd, 2020

**Teachers:** Connie Chen and Eugene Lo

**Slides:** https://tinyurl.com/hoth7-api-slides

# Let's Begin :)
Today, you will learn how to use a JSON object and integrate it into a simple webpage that uses Google Maps API. 

# JSON 
Now what exactly is JSON? JSON stands for JavaScript Object Notation. What this does is allow us to send and receive data in a way that both the server and user can use and understand. JSON works for any programming language, making it super versatile and useful. 

JSON is structured in key-value pairs. In our JSON file, the images are stored in an array of JSON objects. As we can see in the code sample below, JSON can store whatever data we give it. 

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

As we can see above, each JSON object contains a set of parameters that each store a value. For those familiar with JavaScript, you'll notice that the format of this is similar to the way Objects are structures in JavaScript. Later on, we'll be accessing these values when using the Google Maps API. 

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

Now we must put this function in our project so that we can use it later to load these images and their data. 

To do this, we simply insert this function within the `<script></script>` html elements.

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

For now, just use the API key we will be providing you with. When you generate your own API key, avoid sharing it publicly (i.e. embedding the it directly in your code in a public Github repo) because this could compromise your account!

# Demo [Google Maps API]
Now let's get to what you came here for: using the Google Maps API!

In this demo we will be using the Google Maps API to load Google Maps and StreetView into our personal website and add personalized icons and popups using our JSON. 

We'll start out with a basic HTML page: 

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
    <h2>Using the <em>~Google Maps API</em> woohoo </h2>
    <!-- This div will house our map, and we want it to span the width of the entire page -->
    <div id="map" style="width:100%;height:400px;"></div>
</body>
</html>
```

To add a Map object to our webpage, we simply call the Map constructor, which looks like this: 

```js
const map =  new google.maps.Map();
```

How do we use the constructor? Like this:

```js
function myMap() {
  //Location of Map
  const myLatLng = {lat: 34.068921, lng: -118.4473698};
  //Creating the Map inside the HTML element with an id of 'map'
  const map = new google.maps.Map(document.getElementById('map'), {
    zoom: 12,
    center: myLatLng
  });
}
```
In the code above, we want to create the Map object inside a div on the HTML page, and set some options for it, like `zoom` and `center`. We pass in a `getElementById` function as the first argument to the constructor, which will return the div with id `map`. The second argument is an object in which we specify that we want the `zoom` to be 12, and the `center` to be `myLatLng`.

Adding this new function, along with the `getIcons` function created earlier inside `<script>` tags so that the browser can read it as JavaScript, our code should now look like this:

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
    <h2>Using the <em>~Google Maps API</em> woohoo </h2>
    <!-- This div will house our map, and we want it to span the width of the entire page -->
    <div id="map" style="width:100%;height:400px;"></div>
</body>

<script>
  function myMap() {
    //Location of Map
    const myLatLng = {lat: 34.068921, lng: -118.4473698};
    //Creating the Map inside the HTML element with an id of 'map'
    const map = new google.maps.Map(document.getElementById('map'), {
      zoom: 12,
      center: myLatLng
    });
  }

  async function getIcons() {
    // here we are retrieving data from our link
    const response = await fetch('https://api.myjson.com/bins/bd6u2'); 
    const icons = await response.json();
    return icons;
  }
</script>
<script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>
</html>
```
We also load in Google Maps' API in the last `<script>` tag, replacing OUR_API_KEY with our personal API key generated earlier, shown below:

```html
<script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>
```  

Now, when we open this file in the browser, we can see Google Maps embedded in our webpage! 

## Personalizing our Google Maps

Now let's add in some personal icons.

Remember our JSON object? We'll be using that to load in some icons. Here's our code: 

```js
async function setMarkers(map) {
  // This loads our array of JSON objects into the constant variable 'icons'
  const icons = await getIcons();

  // We loop through each JSON object, and create a new Marker for each
  for (const myicon of icons){
    const marker = new google.maps.Marker({
      // Assign the data to properties of the Marker
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
Remember how in our JSON we had elements called 'position', 'title', and 'icon'? Well we are accessing those elements here in the code with `myicon.position`, `myicon.title`, and `myicon.icon`.

Now that we have can create our Markers, let's call the `setMarkers(map)` function in our `myMap()` function so that we can use it! This is what your code should look like after that:

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

    <div id="map" style="width:100%;height:400px;"></div>
</body>
    <script>
    function myMap() {
        //Location of Map
        const myLatLng = {lat: 34.068921, lng: -118.4473698};
        //Creating the Map inside the HTML element with an id of 'map'
        const map = new google.maps.Map(document.getElementById('map'), {
            zoom: 12,
            center: myLatLng
        });
        setMarkers(map);
    }
    
    async function getIcons() {
        // here we are retrieving data from our link
        const response = await fetch('https://api.myjson.com/bins/bd6u2'); 
        const icons = await response.json();
        return icons;
    }

    async function setMarkers(map) {
        // This loads our array of JSON objects into the constant variable 'icons'
        const icons = await getIcons();

        // We loop through each JSON object, and create a new Marker for each
        for (const myicon of icons){
            const marker = new google.maps.Marker({
            // Assign the data to properties of the Marker
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
</html>
```

Ya yeet :) Now we have personalized icons in our map! As we can see, we have have an icon that is centered at a specified longitude and latitude, which we stored in our JSON. 

Now let's add more functionality to our icons. Let's say we want to spice up our map, so that when we click on a marker, we get a pop up box which will give us a link to another website for more information about that place. 

To do this, we will want to use another object from Google Maps' API: an ```InfoWindow```.

## Infowindows

So what is an info window? An info window is what pops up when you click on an icon to get more information about it. Normally, info windows display the name and address of a location. But what if we want to personalize info windows to display different information? The API allows us to do this! 

To load this into our project, we will store this a new variable we'll call ```infowindow``` . Our call looks like this:

```js 
const infowindow = new google.maps.InfoWindow(); 
```
    
We will also need to add a function that will detect whenever we click on an icon. We will add this code inside our icon for-loop, in the `setMarker(map)` function call: 
```js
    // This is what will be displayed on our infoWindow!
    const popUpBox = `
      <div>
        <div>HELLOOooOOo FROM ${myicon.title}</div>
        <p> (click image for more info) </p> 
        <a href="${myicon.link}">
          <img width=70% height=70% src="${myicon.image}">
        </a>
      </div>
    `;

    /* Formatting our InfoWindow to display our desired content */
    infowindow.setContent(popUpBox);
    infowindow.setOptions({maxWidth: 150});
      
    /* This event listener will call the given function whenever a 'click' event happens to a marker */
    google.maps.event.addListener (marker, 'click', function(){
       infowindow.open(map, this);
    });
```
Notice that we pull information fetched from the JSON earlier through `myicon.title`, `myicon.link`, and `myicon.image`.

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

<div id="map" style="width:100%;height:400px;"></div>

 <script>
   function myMap() {
     //Location of Map
     const myLatLng = {lat: 34.068921, lng: -118.4473698};
     //Creating the Map inside the HTML element with an id of 'map'
     const map = new google.maps.Map(document.getElementById('map'), {
       zoom: 12,
       center: myLatLng
     });
     setMarkers(map);
   }
 
   async function getIcons() {
     // here we are retrieving data from our link
     const response = await fetch('https://api.myjson.com/bins/bd6u2'); 
     const icons = await response.json();
     return icons;
   }

   async function setMarkers(map) {
    // This loads our array of JSON objects into the constant variable 'icons'
    const icons = await getIcons();

    // We loop through each JSON object, and create a new Marker for each
    for (const myicon of icons){
        const marker = new google.maps.Marker({
        // Assign the data to properties of the Marker
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
        const popUpBox = `
        <div>
            <div>HELLOOooOOo FROM ${myicon.title}</div>
            <p> (click image for more info) </p> 
            <a href="${myicon.link}">
            <img width=70% height=70% src="${myicon.image}">
            </a>
        </div>
        `;

        /* Formatting our InfoWindow to display our desired content */
        infowindow.setContent(popUpBox);
        infowindow.setOptions({maxWidth: 150});
        
        /* This event listener will call the given function whenever a 'click' event happens to a marker */
        google.maps.event.addListener (marker, 'click', function(){
            infowindow.open(map, this);
        });
    }
   }
  </script>

  <script src="https://maps.googleapis.com/maps/api/js?key=OUR_API_KEY&callback=myMap"></script>
</body>
</html>
```

And that's all :) As we can see, our job is made A LOT easier by using Google Maps' API. Instead of building a map or hardcoding everything ourselves, by using Google's API, we were able to include all the functionality of Google Maps into simple, readable code. 

## Something extra :)



