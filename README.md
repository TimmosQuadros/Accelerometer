# Accelerometer

In kotlin there are some build in objects/classes that you can use to get acces to all the sensors of the smartphone. These classes are:

1. Sensor
2. SensorManager
```java
import android.hardware.Sensor;
import android.hardware.SensorManager;
```
Also we need to extend from AppCompatActivity() and we need to implement the SensorEventListener:

3. AppCompatActivity()
4. SensorEventListener

## Sensor

Class representing a sensor. Use getSensorList(int) to get the list of available Sensors.

```kotlin
sm = getSystemService(Context.SENSOR_SERVICE) as SensorManager
sm.getSensorList(Sensor.TYPE_ALL)
```

## SesnorManager

SensorManager lets you access the device's sensors. Get an instance of this class by calling Context.getSystemService() with the argument SENSOR_SERVICE.

```kotlin
sm = getSystemService(Context.SENSOR_SERVICE) as SensorManager
accelerometer = sm.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
```
## AppCompatActivity()

When extending from this class we get a lot of functions among others the getSystemService which can be used to retreive SensorManager.

```kotlin
sm = getSystemService(Context.SENSOR_SERVICE) as SensorManager
```

## SensorEventListener

When implementing this interface we get two event functions that listens for any change in the selected Sensor type.

```kotlin
class MainActivity() : SensorEventListener{

  override fun onSensorChanged(event: SensorEvent) {

  }

  override fun onAccuracyChanged(sensor: Sensor, accuracy: Int) {

  }

}
```

In order for these events to work corectly and fire whenever a sensor changes its state we need to register the listener:

```kotlin
sm.registerListener(this, accelerometer, SensorManager.SENSOR_DELAY_NORMAL)
```

Since the class itself is the SensorEventListener we give this as first argument for the listener. The seccond argument is the Sensor itself which in our case is the accelerometer. The last argument specifies how often we need to fire an event (i think).

## The app

```kotlin
class MainActivity(internal var accelerometer: Sensor,internal var sm: SensorManager,internal var acceleration: TextView) : AppCompatActivity(), SensorEventListener {

override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        sm = getSystemService(Context.SENSOR_SERVICE) as SensorManager
        accelerometer = sm.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
        sm.registerListener(this, accelerometer, SensorManager.SENSOR_DELAY_NORMAL)

        acceleration = findViewById(R.id.acceleration) as TextView
        decimalFormat.applyPattern("#0.##")
    }
    var counter = 0
    var sum_x = 0f
    var sum_y = 0f
    var sum_z = 0f
    var value = 1000000
    val decimalFormat = DecimalFormat()
    override fun onSensorChanged(event: SensorEvent) {

        if(counter!=value){
            sum_x += event.values[0]
            sum_y += event.values[1]
            sum_z += event.values[2]
            counter++;
        }else {
            acceleration.text = "X: " + decimalFormat.format(sum_x/value) + " Y: " + decimalFormat.format(sum_y/value) + " Z: " + decimalFormat.format(sum_z/value)
            counter = 0
            sum_x = 0f
            sum_y = 0f
            sum_z = 0f
        }

    }

    override fun onAccuracyChanged(sensor: Sensor, accuracy: Int) {

    }

}
```
