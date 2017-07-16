<iframe width="560" height="315" src="https://www.youtube.com/embed/HpLOsNwd_FM" frameborder="0" allowfullscreen></iframe>

TestFairy for React native is a bridge to the [TestFairy](https://www.testfairy.com) SDK. Integrating the TestFairy SDK into your app allows better understanding of how your app performs on real devices. It tells you when and how people are using your app, and provide you with any metric you need to optimize for better user experience and better code.

## iOS installation

1. `npm install --save react-native-testfairy`
3. In Finder, Go to `node_modules` ➜ `react-native-testfairy`, and drag the `ios` directory into your project. Be sure to "Copy items if needed" is selected in the dialog box shown.
4. Add libTestFairy.a from the linked project to your project properties ➜ "Build Phases" ➜ "Link Binary With Libraries"
5. Next you will have to link a few more SDK framework/libraries which are required by TestFairy (if you do not already have them linked.) Under the same "Link Binary With Libraries", click the + and add the following:  
   * CoreMedia.framework  
   * CoreMotion.framework  
   * AVFoundation.framework  
   * SystemConfiguration.framework  
   * OpenGLES.framework  

## Android installation
```
npm install --save react-native-testfairy
```

```gradle
// file: android/settings.gradle
...

include ':react-native-testfairy'
project(':react-native-testfairy').projectDir = new File(settingsDir, '../node_modules/react-native-testfairy/android')
```

```gradle
// file: android/app/build.gradle
...

dependencies {
    ...
    compile project(path: ':react-native-testfairy')
}
```

```java
// file: MainApplication.java
...

import com.testfairy.react.TestFairyPackage; // import package

public class MainApplication extends ReactActivity {

   /**
   * A list of packages used by the app. If the app uses additional views
   * or modules besides the default ones, add more packages here.
   */
  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    ....

    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
          new TestFairyPackage()
      );
    }
  };
  
...
}

```

## Usage
Once the native library has been added to your project, you can now enable session recording with TestFairy. You will need an app token, which you can get from your [preferences](http://app.testfairy.com/settings/) page on your TestFairy account.

Next, from your JavaScript file, (index.ios.js or index.android.js for example), import the TestFairy bridge into your project, and invoke `begin` passing in the app token. Best time to invoke `begin` is usually in `componentWillMount` or right before you register your application. 

```
const TestFairy = require('react-native-testfairy');
...
componentWillMount: function() {
  TestFairy.begin(<insert ios app token here>);
}
```

And that's it! You can now log into your [account](http://app.testfairy.com) and view your sessions. Also, feel free to refer to the [documentation](https://github.com/testfairy/react-native-testfairy/blob/master/index.js) for other available APIs.

### User ID and Session Attributes

TestFairy can automatically detect sessions recorded by the same user, however, in many cases there is some additional information that would help you generate better insights.

You have to invoke `setUserId` or `setAttribute`. With `setUserId`, you can pass in a string representing an association to your backend. It may be, for example, the ID of this user in your database or some random GUID. This value may not be null or empty, and is searchable via API and web search.

The second method, `setAttributes` uses predefined key/value attributes. These attributes are available later in the session recording page, is available via API, and is searchable.

```
const TestFairy = require('react-native-testfairy');
...
componentWillMount: function() {
  TestFairy.setUserId(<correlation id>);
  TestFairy.setAttribute("email": "johns@wall.gov");
  TestFairy.setAttribute("name": "John Snow");
  TestFairy.setAttribute("phone_number": "+672-14-5109");
  TestFairy.setAttribute("age": 14);
  TestFairy.setAttribute("custom.wears": "black");
  TestFairy.setAttribute("custom.works_at": "The Wall");
  
  TestFairy.begin(<insert ios app token here>);
}
```

For more information on `identify`, head over [here](http://docs.testfairy.com/iOS_SDK/Identifying_Your_Users.html).

### Hiding views

TestFairy allows the developer to hide specific views from the recorded video. As the developer, you may choose to hide one or more views from video for security and privacy reasons. For example, you might want to remove all information related to credit card data from appearing in the session.

In order to hide views from your recorded session, you will need to pass a reference to a view to TestFairy. First, give the element to be hidden a `ref` attribute. For example:

```
<Text ref="instructions">This will be hidden</Text>
```

Next, in a component callback, such as `componentDidMount`, pass the reference ID back to TestFairy by invoking `hideView`. 

```
const TestFairy = require('react-native-testfairy');
...
var MyComponent = React.createClass({
  ...
  componentWillMount: function() {
    TestFairy.begin('<insert ios app token here>');
  },

  componentDidMount: function() {
    TestFairyBridge.hideView(this.refs.instructions);
  },
  ...
  render: function() {
    return (<Text ref="instructions">This will be hidden</Text>);
  }
});
```

Now, your Views will be hidden before any video is uploaded to TestFairy.

And that's it! You can now log into your [account](http://app.testfairy.com) and view your sessions. Also, feel free to refer to the [documentation](https://github.com/testfairy/react-native-testfairy/blob/master/index.js) for other available APIs.

## Migrating from 1.x to 2.x

In order to migrate from 1.x to 2.x, a minor change is required in the way you hide views.

Remove the import of `findNodeHandle`
```
- var { findNodeHandle } = React;
```

Pass the `ref` object directly into `TestFairy.hideView()`
```
- TestFairyBridge.hideView(findNodeHandle(this.refs.instructions));
+ TestFairyBridge.hideView(this.refs.instructions);
```

### Where to go from here?
* Have a look at the [API documentation](https://app.testfairy.com/reference/ios/) for other calls you can make to the TestFairy plugin

* Follow the project on [GitHub](https://github.com/testfairy/react-native-testfairy) for updates, reporting bugs, or contributing to the project!
