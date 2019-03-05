import android.Manifest
import android.app.Activity
import android.arch.lifecycle.Lifecycle
import android.arch.lifecycle.LifecycleObserver
import android.arch.lifecycle.OnLifecycleEvent
import android.content.Context
import android.content.pm.PackageManager
import android.location.LocationManager
import android.support.v4.app.ActivityCompat
import android.util.Log
import android.widget.Toast
import com.google.android.gms.location.*


/**
 *
 */
class GPSLocationManager constructor(var context: Activity, var callback: GPSLocationCallback) : LifecycleObserver {


    lateinit var mFusedLocationProviderClient: FusedLocationProviderClient
    lateinit var mLocationCallback: LocationCallback

    private val REQUEST_LOCATION_PERMISSION = 1
    private var mTrackingLocation: Boolean = false

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun onStart() {
        mFusedLocationProviderClient = LocationServices.getFusedLocationProviderClient(context)


        mLocationCallback = object : LocationCallback() {
            override fun onLocationResult(locationResult: LocationResult?) {
                Log.d("locationUpdate", "${locationResult?.lastLocation?.latitude}")
                callback.getLocationUpdate(locationResult?.lastLocation?.latitude.toString(), locationResult?.lastLocation?.longitude.toString())
            }
        }
        if (checkGpsStatus())
            startTrackingLocation()
    }

    private fun checkGpsStatus(): Boolean {

        val locationManager = context.getSystemService(Context.LOCATION_SERVICE) as LocationManager

        return locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER)
    }

    fun startTrackingLocation() {
        if (ActivityCompat.checkSelfPermission(context,
                        Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(context, arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
                    REQUEST_LOCATION_PERMISSION)
        } else {
            if (checkGpsStatus()) {
                mTrackingLocation = true
                mFusedLocationProviderClient.requestLocationUpdates(getLocationRequest(),
                        mLocationCallback,
                        null /* Looper */)
            } else {
                Toast.makeText(context, "Enable your gps", Toast.LENGTH_SHORT).show()
            }
        }
    }

    private fun getLocationRequest(): LocationRequest? {
        var locationRequest = LocationRequest.create()
        locationRequest.interval = 10000
        locationRequest.fastestInterval = 5000
        locationRequest.priority = LocationRequest.PRIORITY_HIGH_ACCURACY
        return locationRequest
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    fun onPause() {
        if (mTrackingLocation) {
            stopTrackingLocation()
            mTrackingLocation = true
            mFusedLocationProviderClient.removeLocationUpdates(mLocationCallback);
        }

    }


    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    fun onResume() {
        if (mTrackingLocation) {
            startTrackingLocation()
        }

    }

    private fun stopTrackingLocation() {
        if (mTrackingLocation) {
            mTrackingLocation = false
        }
    }


    interface GPSLocationCallback {

        fun getLocationUpdate(latitude: String?, longitude: String?)

    }

