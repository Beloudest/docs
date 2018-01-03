
## Installation

1. Download the latest [TestFairy plugin for Unity](https://github.com/testfairy/testfairy-unity-plugin/releases).

2. Unpack the zip on your disk.

3. Drag **TestFairy.cs**, **iOS** and **Android** into your Project under `Assets/Plugins`. If you don't have Plugins, you can drag the entire folder onto your project.

  ![Step 1](https://raw.githubusercontent.com/testfairy/testfairy-unity-plugin/master/Images/step1.png)

4. Open `mainCamera` in Inspector by clicking on it, and then click on `Add Component`. Note: you can add the TestFairy script to any game object. TestFairy is a singleton so no harm is done.

  ![Step 2](https://raw.githubusercontent.com/testfairy/testfairy-unity-plugin/master/Images/step2.png)

5. Type in the name of the script, for example `mainCameraScript`, choose `CSharp` and click on `Create and Add`.

  ![Step 3](https://raw.githubusercontent.com/testfairy/testfairy-unity-plugin/master/Images/step3.png)

6. Edit the newly created CSharp script, add `using TestFairyUnity;` to the import section, and a call to `TestFairy.begin()` with your app token. You can find your app token in the [Account Settings](https://app.testfairy.com/settings/#apptoken) page.

  ![Step 4](https://raw.githubusercontent.com/testfairy/testfairy-unity-plugin/master/Images/step4.png)

 Here is the code, for easy copy-pasting:

 ```
 using UnityEngine;
 using System.Collections;
 using TestFairyUnity;

 public class mainCameraScript : MonoBehaviour {

     // Use this for initialization
     void Start () {
         TestFairy.begin(PUT YOUR SDK APP TOKEN HERE SEE COMMENT ABOVE);
     }

     ...
 }
 ```

7. As minimum, TestFairy requires the `INTERNET` and `ACCESS_NETWORK_STATE` permission for your Android build. You can copy a version of your AndroidManifest.xml from `<root>/Temp/StagingArea/AndroidManifest.xml` into the `<root>/Assets/Plugin/Android` directory. From here, edit `AndroidManifest.xml` with the following line:

 ```xml
 <uses-permission android:name="android.permission.INTERNET" />
 <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
 <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
 ```

 Additional features may require the extra persmissions given below:

 ```xml
 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
 <uses-permission android:name="android.permission.BATTERY_STATS"/>
 <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
 <uses-permission android:name="android.permission.GET_TASKS"/>
 <uses-permission android:name="android.permission.READ_LOGS"/>
 ```

8. Save, build and run.

## Usage

### Setting Screen Name

TestFairy can capture screenshots during a recorded session. It attempts to autmatically name a screenshot based on different measures. In order to override this you can invoke `setScreenName`, and set your own name for a captured screen. `setScreenName` expects a String, so developers are free to label screenshots with any appropriate label. Some developers make use of the level name to set the screenshot, for example:

```
using UnityEngine;
using System.Collections;
using TestFairyUnity;
using UnityEngine.SceneManagement;

public class cameraScript : MonoBehaviour {
...
	void OnLevelWasLoaded(int level) {
		TestFairy.setScreenName(Application.loadedLevelName);
    }
...
}
```

### Identifying your users

TestFairy allows developers to correlate sessions to app specific information such as users, server-sessions or events.   
This is useful in cases where sessions are anonymous and or when sessions are related to server activities that are critical to understanding test behaviour.

Furthermore, TestFairy enables you to identify the user with traits such as name, email or phone number. 
These will later be available for the developer to search upon, or review when looking at a specific session recordings.

In order to set session level attributes associated with your user, please see the document on [Session Attributes](https://docs.testfairy.com/Android/Session_Attributes.html).

Identifying a session means setting a unique identifier for your user.

```
TestFairy.setUserId("<userId>");
```

Where `userId` is a string representing an association to your backend. We recommend passing values that your app may use, such as email, phone number, or user ID. This value may not be nil, and is searchable via API and web search.

### Exaple: identify user by email

```
TestFairy.setUserId("john@example.com");
```

### Session Attributes

TestFairy can collect additional information fom your session, which can help you generate better insights.

```
TestFairy.setAttribute("<key>", "value");
```

The first value is a string `key` to help you search for the attribute in your session. The second paramter, `value`, is any string value for the attribute associated with the session. Neither value can be nil. These attributes are later available in the session recording page, are available via API, and are searchable.

#### Example

```
TestFairy.setAttribute("first-session","True");
TestFairy.setAttribute("visits-in-checkout-during-session","3");
TestFairy.setAttribute("max-score-this-session","200");
```

### Remote Logging

In order to collect logs from your Unity app's session, there are a couple of options:

1. Make use of the `TestFairy.log` method.

2. For iOS, you can edit your project's `pch` according to the steps given [here](https://docs.testfairy.com/iOS_SDK/Logs_on_iOS_10.html). For some apps written in Unity, this may not be possible since your build may overwrite the pch file on each build. One workaround is to use a post-process step that will allow you to append to a created pch file. The post-process snippet can be found below:

```
[PostProcessBuildAttribute()]
public static void OnPostprocessBuild(BuildTarget buildTarget, string path) {   
  string pchPath = path + "/Classes/Prefix.pch";   
  string testfairySnippet =
  "#ifdef __OBJC__\n" + 
  "  #import \"TestFairy.h\"\n" + 
  "#endif\n" + 
  "#define NSLog(s, ...) do { NSLog(s, ##__VA_ARGS__); TFLog(s, ##__VA_ARGS__); } while (0)\n";  
  File.AppendAllText(pchPath, testfairySnippet);
}
```
