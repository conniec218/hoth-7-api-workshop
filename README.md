# HOTH 7 API workshop

**Date** Feb 23rd, 2020

**Teachers:** Connie Chen and Eugene Lo

**Slides:** https://tinyurl.com/apislides

**Link to the Google Maps API:** https://developers.google.com/maps/documentation/

# Let's Begin :)
Today, you will learn how to use a JSON object and integrate it into a simple webpage that uses Google Maps API. Today, we'll be learning how to use the Google Maps API specifically for JavaScript (documentation here: https://developers.google.com/maps/documentation/javascript/tutorial). Documentation will be your best friend when using APIs!

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
Let's see if we can spice up our webpage *even more* by adding another Maps object: StreetView!

Let's say we want to embed a StreetView object in our webpage, right underneath the map we already have. We want to have it so that clicking on an icon on our original Map shifts the StreetView to the location that we just clicked. 

First things first, how do we embed a StreetView object into our webpage? Exactly how we did it for our Map object!

We create another div that will hold our StreetView object, as follows:

```html
<div id="pano" style="width:100%;height:400px;"></div>
 ```

Then, inside our `myMap` function, we call the constructor for the StreetView object, similar to the way we did it for the Map object.

```js
function myMap() {
     const myLatLng = {lat: 34.068921, lng: -118.4473698};
     const map = new google.maps.Map(document.getElementById('map'), {
       zoom: 12,
       center: myLatLng
     });
     const pano = new google.maps.StreetViewPanorama(document.getElementById('pano'), {
        position: myLatLng
    });

    setMarkers(map);
}
```

Now that we have our StreetView object, how can we manipulate it so that its position will change when you click on an icon?

The documentation shows us that StreetView objects defined a function called `setPosition` which, as you might have guessed, changes the position of the StreetView. And remember how we already created an eventListener for our markers? (Hint: it opens an infoWindow). We can piggyback off that eventListener and use it to set the position of our StreetView for each 'click' event as well!

That should look like this (in our `setMarkers` function):

```js
google.maps.event.addListener (marker, 'click', function(){
    infowindow.open(map, this);
    pano.setPosition(marker.position);
});
```

Finally, we want to change the arguments for `setMarkers` so that it takes in the StreetView object, called `pano` in this case, as well as the map. This way, we can use it in our eventListener later in the function.

Congrats! Hopefully now you have a better understanding of what APIs are, and the amazing features you can build with them :) If you want to make sure you have everything right, or ever need to reference the code, peep the completed code in `example.html`!
