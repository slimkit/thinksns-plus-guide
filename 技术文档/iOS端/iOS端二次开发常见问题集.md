#### TS+ iOS端二次开发常见问题集
1. ProductName
 * 错误描述：

	```
	ERROR ITMS-90121: "This bundle is invalid. The executable name, as reported by CFBundleExecutable in the Info.plist file, may not contain any of these characters: \ [ ] { } ( ) . + *"
	```

 * 解决方案：Target -> Build Settings -> Packaging -> Product Name，不要使用中文符号，而且TS+默认使用 "$(TARGET_NAME)"，该target值有问题

2. UsageDescription
 * 错误描述：

	```
	** Missing Info.plist key ** - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSContactsUsageDescription key with a string value explaining to the user how the app uses this data.
	** Missing Info.plist key ** - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSPhotoUsageDescription key with a string value explaining to the user how the app uses this data.
	** Missing Info.plist key ** - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSCameraUsageDescription key with a string value explaining to the user how the app uses this data.
	Once these issues have been corrected, you can then redeliver the corrected binary.
	``` 
	
 * 解决方案：Info.plist给UsageDescription添加描述

	```
	<key>NSCameraUsageDescription</key>
	<string>需要您的同意才能使用相机</string>
	<key>NSPhotoLibraryUsageDescription</key>
	<string>需要您的同意才能访问相册</string>
	<key>NSContactsUsageDescription</key>
	<string>需要您的同意才能访问通讯录</string>
	<key>NSLocationWhenInUseUsageDescription</key>
	<string>需要您的同意才能使用位置服务</string>
	```

3. Other


