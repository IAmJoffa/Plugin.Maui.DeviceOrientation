## Device Orientation Plugin for .Net Maui and Windows

Simple cross-platform plugin to work with screen orientation of mobile device.

Based on [Plugin.DeviceOrientation](https://github.com/wcoder/Xamarin.Plugin.DeviceOrientation) by [Yauheni Pakala](https://github.com/wcoder/)

[iOS demo](https://youtu.be/3eQDrHMPmE4)

[Maui sample](https://github.com/compusport/Plugin.Maui.DeviceOrientation/tree/master/samples/Plugin.Maui.DeviceOrientation.Sample)

#### Setup

* Available on NuGet: [![NuGet Badge](https://buildstats.info/nuget/Plugin.Maui.DeviceOrientation)](https://www.nuget.org/packages/Plugin.Maui.DeviceOrientation/)
* Install into your PCL/NetStandard project and Platform Specific projects

#### Platform Support

|Platform|Version|
| ------------------- | :------------------: |
|iOS|iOS 14.2+|
|Android|API 21+|
|Windows 10 UWP|10.0.16299+|

## API Usage

Call `DeviceOrientation.Default` from any project to gain access to APIs.

```csharp
/// <summary>
/// Gets current device orientation
/// </summary>
DeviceOrientations CurrentOrientation { get; }
```

When device orientation is changed you can register for an event to fire:

```csharp
/// <summary>
/// Event handler when orientation changes
/// </summary>
event OrientationChangedEventHandler OrientationChanged;
```

You will get a `OrientationChangedEventArgs` with the orientation type:

```csharp
public class OrientationChangedEventArgs : EventArgs
{
	public DeviceOrientations Orientation { get; set; }
}

public delegate void OrientationChangedEventHandler(object sender, OrientationChangedEventArgs e);
```

The **DeviceOrientations** enumeration has these members.

|Member|Value|Description|
| :----------------: | :-----------: | :------------------ |
|**Undefined**|0|No display orientation is specified.|
|**Landscape**|1|Specifies that the monitor is oriented in landscape mode where the width of the display viewing area is greater than the height.|
|**Portrait**|2|Specifies that the monitor rotated 90 degrees in the clockwise direction to orient the display in portrait mode where the height of the display viewing area is greater than the width.|
|**LandscapeFlipped**|4|Specifies that the monitor rotated another 90 degrees in the clockwise direction (to equal 180 degrees) to orient the display in landscape mode where the width of the display viewing area is greater than the height. This landscape mode is flipped 180 degrees from the **Landscape** mode.|
|**PortraitFlipped**|8|Specifies that the monitor rotated another 90 degrees in the clockwise direction (to equal 270 degrees) to orient the display in portrait mode where the height of the display viewing area is greater than the width. This portrait mode is flipped 180 degrees from the **Portrait** mode.|

Call `LockOrientation` for set orientation and lock with disabling device auto-rotation:
```csharp
/// <summary>
///     Lock orientation in the specified position
/// </summary>
/// <param name="orientation">Position for lock.</param>
void LockOrientation(DeviceOrientations orientation);
```
For disable the lock, call `UnlockOrientation` method:
```csharp
/// <summary>
///     Unlock orientation
/// </summary>
void UnlockOrientation();
```

### iOS Specific Support

Add this code for your ViewController where you want locking orientation:
```csharp
public override bool ShouldAutorotate()
{
	// set plugin for handle of this method
	return DeviceOrientationImplementation.ShouldAutorotate;
}

public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations()
{
	// allow all orientations
	return UIInterfaceOrientationMask.AllButUpsideDown;
}
```
In your `Info.plist` need set all device orientations.

#### Xamarin.Forms iOS

If you want to lock orientation for the entire iOS application.

Add this code in your `AppDelegate.cs`:
```csharp
[Export("application:supportedInterfaceOrientationsForWindow:")]
public UIInterfaceOrientationMask GetSupportedInterfaceOrientations(UIApplication application, IntPtr forWindow)
{
    return DeviceOrientationImplementation.SupportedInterfaceOrientations;
}
```
**Note:** In this case, to lock/unlock orientation on the one screen, you must use the `LockOrientation`/`UnlockOrientation` methods.

#### Xamarin.Forms Android

In your `MainActivity.cs`, add overriding for changing orientation as here:

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);

    DeviceOrientationImplementation.NotifyOrientationChange(newConfig.Orientation);
}
```
**Note:** This solution has only two state **Portrait** and **Landscape**.

## Additional information
* [Android - Handling Rotation](https://developer.xamarin.com/guides/android/application_fundamentals/handling_rotation/)
* [Xamarin.Forms - Device Orientation](https://developer.xamarin.com/guides/xamarin-forms/user-interface/layouts/device-orientation/)

## Troubleshooting
* [See in the issues](https://github.com/wcoder/Xamarin.Plugin.DeviceOrientation/issues?q=label%3Afaq)



## Contributors
* [Yauheni Pakala](https://github.com/wcoder)
* [Christian Lavallee](https://github.com/compusport)

---
&copy; 2016-2024 MIT License
