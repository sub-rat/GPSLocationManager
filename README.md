# GPSLocationManager
Lifecycle observer class on android than can manage GPS Location

## Location Manager ##

The class will provide the location update . Permission check are included in here.

## What is Used? ##

- LifeCycleObserver
- FusedLocationProviderClient
- LocationCallback
- LocationService

## Permission Required ##

- ACCESS_FINE_LOCATION 
- ACCESS_COARSE_LOCATION

## What you have to do To get location?? ##
Define the variable on top

```  
  lateinit var locationManager: GPSLocationManager
  var location: Location? = null
```

you have to add life cycleObserver in your activity 

```
    override fun onCreate(savedInstanceState: Bundle?) {
    location = GPSLocationManager(this,this)
          lifecycle.addObserver(location)
      }
```

you have to overide and pass the permission result 

```
    override fun onRequestPermissionsResult(
                requestCode: Int,
                permissions: Array<String>,
                grantResults: IntArray) {
                super.onRequestPermissionsResult(requestCode, permissions, grantResults)
            locationManager.onRequestPermissionsResult(requestCode, permissions, grantResults)
        }
 ```
 
 you have to override onActivityResult and pass to location manager
 
 ```
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
          super.onActivityResult(requestCode, resultCode, data)
          locationManager.onActivityResult(requestCode, resultCode, data)
      }
 ```

Finally you will get the Data in 

```
    override fun getLocationUpdate(location: Location?) {
            Log.d("latitudeandlongitude", "$latitude $longitude")
        }
 ```
