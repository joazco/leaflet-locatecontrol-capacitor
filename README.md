# leaflet-locatecontrol-capacitor
This code makes the Leaflet LocateControl plugin compatible with Capacitor or Ionic. It adjusts the plugin's geolocation functionalities to use @capacitor/geolocation, ensuring better integration and performance on iOS and Android platforms.

### Compatibilité
leaflet-locatecontrol-capacitor is compatible with iOS, Android, and Web.

## Leaflet

[Leaflet](https://leafletjs.com/) is the leading open-source JavaScript library for mobile-friendly interactive maps. Weighing just about 42 KB of JS, it has all the mapping features most developers ever need.

## Leaflet.Locate

[Leaflet.Locate](https://github.com/domoritz/leaflet-locatecontrol) is a useful control to geolocate the user with many options. Official Leaflet and MapBox plugin.

## Capacitorjs

[Capacitor](https://capacitorjs.com/) is an open source native runtime for building Web Native apps. Create cross-platform iOS, Android, and Progressive Web Apps with JavaScript, HTML, and CSS.

## Installation

## Installation

1. Copy the [L.Control.Locate.js](https://github.com/joazco/leaflet-locatecontrol-capacitor/blob/main/L.Control.Locate.js) file into your project's `src` folder.
2. Import the script in your project by adding the following line to your code:
   ```javascript
   import "src/L.Control.Locate.js";
   ```
3. Install the @capacitor/geolocation plugin by running:
   ```bash
   npm install @capacitor/geolocation
   npx cap sync  # or use `ionic capacitor sync` if you're using Ionic
   ```

## Implementation in Your Source Code

```typescript
import L from "leaflet";
import { WatchPositionCallback } from "@capacitor/geolocation";
import "leaflet.locatecontrol/dist/L.Control.Locate.min.css"; // Import styles
import "leaflet/dist/leaflet.css";

// Import the custom plugin
import "src/L.Control.Locate.js";


// Initialize the map
const m = L.map('map', {
  center: L.latLng(currentPosition.lat, currentPosition.lng),
  zoom: 15.4,
  zoomControl: false,
});
L.tileLayer(
  mapUrl,
  {
    attribution:
      'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
    maxZoom: 20,
    id: "mapbox.streets",
  }
).addTo(m);

// Configure and add the locate control to the map
const controlLocate = L.control
  .locate({
    keepCurrentZoomLevel: true,
    showPopup: false,
    setView: "untilPanOrZoom",
    showCompass: true,
    drawCircle: true,
    position: "bottomright",
  })
  .addTo(m);

// Optionally start with a default position
// Optionally callback for watch positon
controlLocate.start({
  lat: currentPosition?.coords.latitude,
  lng: currentPosition?.coords.longitude,
  accuracy: currentPosition?.coords.accuracy,
}, (position: WatchPositionCallback) => {});

// HTML container for the map
<div style="width: '100%'; height: '100%'" id="map" />;
```

## Warning

The `watch` function of the `@capacitor/geolocation` plugin is utilized within the `leaflet-locatecontrol-capacitor` plugin. If the plugin does not function properly, it might be due to a conflict arising if your code is already watching the position. In such cases, you can resolve the issue by using the `clearWatch` function to stop previous watch processes.

***If you want to listen to the real-time position while displaying the map with the Leaflet.Locate plugin, you have the option to add a callback to the start function:***
```typescript
start(defaultPosition?: Position, callbackWatchPosition?: (position: WatchPositionCallback) => void) => void
``` 


