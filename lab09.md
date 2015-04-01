# Lab 9: Google Maps

## Embedding Google Maps into our application

Google Maps is a popular application on the Android platform. It allows the
users to view the map as well as find directions on the map. Google also
provides an SDK to embed a map into our Android application.

To embed a map into the application, we can easily create a new **Google Maps
Activity**. Then, we need to modify the setting file called
`google_maps_api.xml` and set the API key.

The API key can be obtained from the **Google Developer Console**. This can be
easily done by following the link in `google_maps_api.xml` file, and click
**Create** to generate an API key. Then, we can put the key in the
`google_maps_api.xml` file. After that, we can run the application to test its
feature.

## Handling Long Clicks

The activity comes with a variable called `mMap` of type `GoogleMap`. We can
make the `GoogleMap` object respond the user's long clicks by

1. Make the activity implement `GoogleMap.OnMapLongClickListener`.

2. Set the activity as the listener of `mMap` by adding the following statement
   in `onCreate` method

  ```java
  mMap.setOnMapLongClickListener(this);
  ```

3. Implement `onMapLongClick` method which will be called when the user makes a
   long-click on the map. This method is called with the coordinate that the
   user has selected.

  ```java
  @Override
  public void onMapLongClick(LatLng latLng) {

  }
  ```

## Adding Markers on the Map

We can add a marker on the map by calling the `addMarker` method. Here, the
method needs an object of type `MarkerOptions`. 

```java
@Override
public void onMapLongClick(LatLng latLng) {
    mMap.addMarker(new MarkerOptions().position(latLng).title("My Marker"));
}
```

## Drawing Lines on the Map

Similar to the markers, we can draw a line on the map by called `addPolyLine`
method. It needs an object of type `PolyLineOptions`.

```java
@Override
public void onMapLongClick(LatLng latLng) {
    mMap.addMarker(new MarkerOptions().position(latLng).title("My Marker"));
    l = new LatLng(0, 0);
    mMap.addPolyline(new PolylineOptions().width(1).add().add(latLng));
}
```

This code draws a line from (0,0) to the touch position.

## Moving the Map Center

We can set the center of the map, and the zoom level by calling `moveCamera`
and `animateCamera` methods, respectively.

```java
@Override
public void onMapLongClick(LatLng latLng) {
    CameraUpdate center = CameraUpdateFactory.newLatLng(latLng);
    CameraUpdate zoom = CameraUpdateFactory.zoomTo(5);

    mMap.moveCamera(center);
    mMap.animateCamera(zoom);
}
```

## Getting the current location

`LocationManager` and `LocationListener` can be used to obtain the current location
using the GPS component equipped with the device. 

See `its333y2014/Lab9ExampleLocation` for the details on how to use the
`LocationManager`.

Note that we need to add a `use-premission` tag in the `AndroidManifest.xml`
when we want to use the location service.

If you use the emulator to test your application, you can set the current
location by `telnet localhost 5554`, and type

```
geo fix <longitude> <latitude>
```

## Exercise

1. Create a new Android application project with a Google Maps Activity.

2. Use `LocationManager` to obtain the current location and set the map center
   to the obtained location. Set the zoom level to 7.

3. Add a marker to the map when the user makes a long click on the map.

4. Draw lines to trace the user movement.
