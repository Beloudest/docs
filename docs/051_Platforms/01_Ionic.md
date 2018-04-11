Adding the TestFairy plugin to your Ionic project is simple. The following instructions are for Ionic 3.

## Installation

Run the following commands from your application root folder:

```
ionic cordova plugin add com.testfairy.cordova-plugin
```

Alternatively, you can install it directly from GitHub:

```
ionic cordova plugin add https://github.com/testfairy/testfairy-cordova-plugin
```

## Upgrading

To upgrade your plugin, please run:

```
ionic cordova plugin update com.testfairy.cordova-plugin
```

## Usage

Initialize TestFairy with your [App Token](https://app.testfairy.com/settings/#apptoken) by calling `TestFairy.begin`. Your **APP TOKEN** is available at `https://app.testfairy.com/settings/#apptoken`.

We recommend to invoking `TestFairy.begin` from `platform.ready()` in `src/app/app.component.ts`. Also, be sure to declare `TestFairy` at the top of the file.

```
import { Component } from '@angular/core';
import { Platform } from 'ionic-angular';
import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';

import { HomePage } from '../pages/home/home';

// Declare the TestFairy instance
declare var TestFairy: any;

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  rootPage:any = HomePage;

  constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen) {
    platform.ready().then(() => {
			TestFairy.begin(APP TOKEN);
      // Okay, so the platform is ready and our plugins are available.
      // Here you can do any higher level native things you might need.
      statusBar.styleDefault();
      splashScreen.hide();
    });
  }
}
```

**Note**: We currently do not support plugin mocking or browser development. During your development phase, we recommend checking for the existence of `TestFairy` on the `window` object before invoking any methods on the TestFairy object, e.g.

```
// Check if TestFairy is available (will be undefined in browser)
if ((<any>window).TestFairy) {
	TestFairy.begin(APP TOKEN);
}
```

### Identifying your users

See the [SDK Documentation](https://docs.testfairy.com/SDK/Identifying_Your_Users.html#cordova) for more information.

### Session Attributes

See the [SDK Documentation](https://docs.testfairy.com/SDK/Session_Attributes.html#cordova) for more information.

### Remote Logging

See the [SDK Documentation](https://docs.testfairy.com/SDK/Remote_Logging.html#cordova) for more information.

## Where to go from here?

Congratulations! You've successfully integrated TestFairy into your Ionic project! Visit your [dashboard](http://app.testfairy.com/), where you should see your app listed.

* Have a look at the [API documentation](https://github.com/testfairy/testfairy-cordova-plugin/blob/master/www/testfairy.js) for other calls you can make to the TestFairy plugin

* Follow the project on [GitHub](https://github.com/testfairy/testfairy-cordova-plugin) for updates, bug reports, or to contribute to the project!
