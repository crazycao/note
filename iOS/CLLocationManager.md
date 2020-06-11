# CLLocationManager

The object that you use to start and stop the delivery of location-related events to your app.

用于启动和停止向应用程序发送位置相关的事件。

## Overview 概述

You use instances of this class to configure, start, and stop the Core Location services. A location manager object supports the following location-related activities:

您可以使用这个类的实例来配置、启动和停止 Core Location 服务。位置管理器对象支持以下与位置相关的活动：

- Tracking large or small changes in the user’s current location with a configurable degree of accuracy.
- 以可配置的精度跟踪用户当前位置的大或小的变化。

- Reporting heading changes from the onboard compass. (iOS only)
- 报告来自手机上的指南针的方向变化。（仅限iOS）

- Monitoring distinct regions of interest and generating location events when the user enters or leaves those regions.
- 监视不同的感兴趣区域，并在用户进入或离开这些区域时生成位置事件。

- Deferring the delivery of location updates while the app is in the background. (iOS only)
- 在应用程序处于后台时延迟位置更新的发送。（仅限iOS）

- Reporting the range to nearby beacons.
- 报告到附近标记点的距离。

When you are ready to use location services, follow these steps:

当你准备使用定位服务时，遵从这些步骤：

1. Check to see if your app is authorized to use location services and request permission if your app's authorization status is not yet determined, as described in Requesting Permission to Use Location Services.

	检查您的应用是否被授权使用位置服务，如果尚未确定您的应用的授权状态，则按《[请求位置服务的授权](#1)》中所述请求权限。

2. Check to see if the appropriate location services are available for you to use, as described in Determining the Availability of Location Services.

	检查是否有适当的位置服务可供您使用，如《[确定位置服务的可用性](#2)》中所述。

3. Create an instance of the CLLocationManager class and store a strong reference to it somewhere in your app.

	创建 [CLLocationManager](https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc) 类的实例，并在应用程序中的某个位置存储对它的强引用。

	Keeping a strong reference to the location manager object is required until all tasks involving that object are complete. Because most location manager tasks run asynchronously, storing your location manager in a local variable is insufficient.

	在涉及到位置管理器对象的所有任务完成之前，必须保持对该对象的强引用。因为大多数位置管理器任务是异步运行的，所以将位置管理器存储在本地变量中是不够的。

4. Assign a custom object to the delegate property. This object must conform to the CLLocationManagerDelegate protocol.

	将自定义对象指定给 [delegate](https://developer.apple.com/documentation/corelocation/cllocationmanager/1423792-delegate?language=objc) 属性。此对象必须遵从 [CLLocationManagerDelegate](https://developer.apple.com/documentation/corelocation/cllocationmanagerdelegate?language=objc) 协议。

5. Configure the properties related to the service you intend to use. For example, when getting location updates, always configure the distanceFilter and desiredAccuracy properties.

	配置与要使用的服务相关的属性。例如，获取位置更新时，始终配置 [distanceFilter](https://developer.apple.com/documentation/corelocation/cllocationmanager/1423500-distancefilter?language=objc) 和 [desiredAccuracy](https://developer.apple.com/documentation/corelocation/cllocationmanager/1423836-desiredaccuracy?language=objc) 属性。

6. Call the appropriate method to start the delivery of events.

	调用适当的方法开始事件的发送。

For the services you use, configure any properties associated with that service accurately. Core Location manages power aggressively by turning off hardware when it is not needed. For example, setting the desired accuracy for location events to one kilometer gives the location manager the flexibility to turn off GPS hardware and rely solely on the WiFi or cell radios, which can lead to significant power savings.

对于您使用的服务，请准确配置与该服务关联的所有属性。Core Location 通过在不需要时关闭硬件来有效地管理电能。例如，将定位事件所需的精度设置为1公里，可使位置管理器灵活地关闭 GPS 硬件，并仅依赖 WiFi 或蜂窝无线电，这可显著节省电力。

All location- and heading-related updates are delivered to the associated delegate object, which is a custom object that you provide. For information about the delegate methods you use to receive events, see CLLocationManagerDelegate.

所有与位置和方向相关的更新都将发送到关联的 delegate 对象，该对象是你提供的一个自定义对象。有关用于接收事件的 delegate 方法的信息，请参阅 [CLLocationManagerDelegate](https://developer.apple.com/documentation/corelocation/cllocationmanagerdelegate?language=objc)。

## Topics 主题

<span id='1'/>
### Requesting Authorization for Location Services 
### 请求定位服务的授权

- [- requestWhenInUseAuthorization]()

	Requests permission to use location services while the app is in the foreground.
	
	请求在应用程序位于前台时使用位置服务的权限。

- [- requestAlwaysAuthorization]()

	Requests permission to use location services whenever the app is running.
	
	请求允许在应用运行时使用位置服务。
	
- [CLAuthorizationStatus]()

	Constants indicating the app's authorization to use location services.

	指示应用程序使用位置服务的授权状态的常量。

<span id='2'/>
### Determining the Availability of Services
### 确定服务的可用性

- [+ authorizationStatus]()

	Returns the app’s authorization status for using location services.
	
	返回应用程序使用位置服务的授权状态。

- [+ locationServicesEnabled]()

	Returns a Boolean value indicating whether location services are enabled on the device.
	
	返回一个布尔值，指示设备上是否启用了位置服务。
	
- [+ significantLocationChangeMonitoringAvailable]()

	Returns a Boolean value indicating whether the significant-change location service is available.
	
	返回一个布尔值，指示显著变更位置服务是否可用。
	
- [+ headingAvailable]()

	Returns a Boolean value indicating whether the location manager is able to generate heading-related events.
	
	返回一个布尔值，指示位置管理器是否能够生成与方向相关的事件。
	
- [+ isMonitoringAvailableForClass:]()

	Returns a Boolean value indicating whether the device supports region monitoring using the specified class.
	
	返回一个布尔值，指示设备是否支持使用指定类进行区域监视。
	
- [+ isRangingAvailable]()

	Returns a Boolean value indicating whether the device supports ranging of Bluetooth beacons.
	
	返回一个布尔值，指示设备是否支持蓝牙标记点的范围。
	
### Receiving Data from Location Services

### 从定位服务接收数据

- [delegate]()

	The delegate object to receive update events.
	
	接收更新事件的委托对象。
	
- [CLLocationManagerDelegate]()

	The methods that you use to receive events from an associated location manager object.
	
	用于从关联的位置管理器对象接收事件的方法。
	
### Initiating Standard Location Updates

### 启动标准位置更新

- [- startUpdatingLocation]()

	Starts the generation of updates that report the user’s current location.
	
	开始生成报告用户当前位置的更新。
	
- [- stopUpdatingLocation]()

	Stops the generation of location updates.
	
	停止生成位置更新。

- [- requestLocation]()

	Requests the one-time delivery of the user’s current location.
	
	请求发送一次用户的当前位置。
	
- [pausesLocationUpdatesAutomatically]()

	A Boolean value indicating whether the location manager object may pause location updates.
	
	一个布尔值，指示位置管理器对象是否要暂停位置更新。
	
- [allowsBackgroundLocationUpdates]()

	A Boolean value indicating whether the app should receive location updates when suspended.
	
	一个布尔值，指示应用程序在挂起时是否应接收位置更新。
	
- [showsBackgroundLocationIndicator]()

	A Boolean indicating whether the status bar changes its appearance when location services are used in the background.
	
	一个布尔值，指示在后台使用位置服务时状态栏是否会改变其显示。
	
- [distanceFilter]()

	The minimum distance (measured in meters) a device must move horizontally before an update event is generated.
	
	在生成更新事件之前，设备必须水平移动的最小距离（以米为单位）。
	
- [desiredAccuracy]()

	The accuracy of the location data.
	
	位置数据的精度。
	
- [activityType]()

	The type of user activity associated with the location updates.
	
	与位置更新关联的用户活动的类型。
	
- [CLActivityType]()

	Constants indicating the type of activity associated with location updates.
	
	指示与位置更新关联的活动类型的常量。
	
	