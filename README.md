# Appstock SDK for Unity

## Overview
**Appstock SDK** is a native library that monetizes iOS/Android applications.  
*"Appstock SDK for Unity"* is a **C# facade** that allows using the native library within Unity.

### Supported Ad Formats
- **Banner Ads**: Anchored to screen edges.
- **Interstitial Ads**: Fullscreen ads for user engagement.
- **Rewarded Ads**: Provides bonuses to users.
- **Native Ads**: Seamlessly integrates into your app.

## Package Contents
This package primarily consists of a **code library**, minimizing the need to browse assets.

### Folder Structure:
- **Editor/**: Contains XML for External Dependency Manager for Unity (EDM4U).
- **Plugins/**: Includes managed (DLL) and native (AAR/framework) plugins.

[Unity Plugins Documentation](https://docs.unity3d.com/Manual/plug-ins.html)

## Installation

### Installing the Package
1. Open **Unity Package Manager**.
2. Add the **UPM package** manually.
3. Ensure all dependencies are resolved.

### Native Dependencies
- The package includes an `Editor/NativeDependencies.xml` file for dependency management.
- Uses **CocoaPods** for iOS and **Maven** for Android.
- If **EDM4U** is installed, dependencies will be handled automatically.

[Unity External Dependency Manager](https://github.com/googlesamples/unity-jar-resolver)  

## Requirements
This package was developed and tested with:

| Tool               | Version          |
|--------------------|-----------------|
| Unity             | 2021.3.45f1 / 6000.0.29f1 |
| Xcode             | 16.1 |
| Android Studio    | 2024.2.1 |
| Minimal iOS Target | 12.0 |
| Android Min SDK   | 22 |
| Android Compile SDK | 34 |
| Swift Version     | 6 |
| Java Version      | 17 |
| C# Version       | 9.0 |
| .NET Standard    | 2.1 |

## SDK Initialization
To initialize the SDK, call `InitializeSdk()` before loading any ads.

```csharp
using System;
using AppstockSDK.Api;
using UnityEngine;

public class SdkInitializer : MonoBehaviour
{
    private void Start()
    {
        Debug.Log($"[{DateTime.Now:O}] Initializing SDK...");
        Appstock.InitializeSdk("appstock-demo");
    }
}
```

## Ad Implementation

### Banner Ads
```csharp
using AppstockSDK.Api;
using UnityEngine;

public class BannerDemo : MonoBehaviour
{
    private IBannerAd? _adUnit;

    private void Start()
    {
        _adUnit = new BannerAd(new(320, 50)) { PlacementID = "4" };
        _adUnit.LoadAd();
    }
}
```

### Interstitial Ads
```csharp
using AppstockSDK.Api;
using UnityEngine;

public class InterstitialDemo : MonoBehaviour
{
    private IInterstitialAd? _adUnit;

    private void Start()
    {
        _adUnit = new InterstitialAd { PlacementID = "5" };
        _adUnit.LoadAd();
    }
}
```

### Rewarded Ads
```csharp
using AppstockSDK.Api;
using UnityEngine;

public class RewardedDemo : MonoBehaviour
{
    private IRewardedAd? _adUnit;

    private void Start()
    {
        _adUnit = new RewardedAd { PlacementID = "12" };
        _adUnit.LoadAd();
    }
}
```

## Troubleshooting

### Unity 2021/2022 Android Issues
#### Issue: Unity 2022.3 Android SDK 35 build fails  
```
Execution failed for task ':launcher:mergeExtDexDebug'.
> Could not resolve all files for configuration ':launcher:debugRuntimeClasspath'.
```
✅ Solution: Use **Unity 6** for Android builds.

[Unity Build Issue](https://discussions.unity.com/t/gradle-build-issues-for-android-api-sdk-35-in-unity-2022-3lts/1502187/1)

### iOS Network Security Issues
Modify `Info.plist` to allow **insecure HTTP** requests:

```csharp
using UnityEditor.iOS.Xcode;
using System.IO;

public class InfoPlistUpdater
{
    public void UpdatePlist(string pathToBuiltProject)
    {
        string plistPath = pathToBuiltProject + "/Info.plist";
        PlistDocument plist = new PlistDocument();
        plist.ReadFromString(File.ReadAllText(plistPath));
        plist.root.CreateDict("NSAppTransportSecurity").SetBoolean("NSAllowsArbitraryLoads", true);
        File.WriteAllText(plistPath, plist.WriteToString());
    }
}
```

[Apple Security Guide](https://developer.apple.com/documentation/security/preventing-insecure-network-connections)

## Sample Projects
The package includes sample projects:

| Sample Name      | Description |
|-----------------|----------------------------------|
| **Banner**       | Demonstrates banner ads |
| **Interstitial** | Demonstrates interstitial ads |
| **Rewarded**     | Demonstrates rewarded ads |
| **Native**       | Implements native ads |
| **Config & Targeting** | Demonstrates SDK configuration |

## SDK Configuration

### Setting Configurations
Modify **SDK settings** using `SdkConfig`.

```csharp
using AppstockSDK.Api;
using UnityEngine;

public class ConfigDemo : MonoBehaviour
{
    public SdkConfig sdkConfig = new();

    private void Start()
    {
        Appstock.Sdk.Apply(sdkConfig);
    }
}
```

### Targeting Configuration
Modify **targeting settings** using `TargetingData`.

```csharp
using AppstockSDK.Api;
using UnityEngine;

public class TargetingDemo : MonoBehaviour
{
    public TargetingData targetingData = new();

    private void Start()
    {
        Appstock.Targeting.Apply(targetingData);
    }
}
```

## References & Resources
- [Unity Documentation](https://docs.unity3d.com/)
- [Google External Dependency Manager](https://github.com/googlesamples/unity-jar-resolver)
- [OpenRTB Native Ads Specification](https://iabtechlab.com/wp-content/uploads/2016/07/OpenRTB-Native-Ads-Specification-Final-1.2.pdf)

## License & Credits
© 2025 **Appstock SDK**. All Rights Reserved.

## Support
For support and updates, visit:

- **Website**: [https://appstock.io](#)  
- **GitHub Issues**: [https://github.com/appstock/issues](#)  
- **Email**: support@appstock.io  

🚀 **Thank you for using Appstock SDK!**
