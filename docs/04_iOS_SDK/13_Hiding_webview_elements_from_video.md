TestFairy enables developers to hide specific HTML elements from a recorded video in any `UIWebView` or `WKWebView` within their application. As the developer, you may choose to hide one or more elements from the video for security and privacy reasons.

For example, you might want to prevent all information related to credit card data from appearing in the session.

### Syntax

To hide an element from video, all you need to do is call the static instance method `hideWebViewElements` in the `TestFairy` class passing in a CSS selector to target your element:

**`[TestFairy hideWebViewElements:@"body"];`**  
**`[TestFairy hideWebViewElements:@"div#username,div#password"];`**  
**`[TestFairy hideWebViewElements:@".col-12,h1 .header"];`**

TestFairy will find any `UIWebView` or `WKWebView` in the view hierarchy, and hide a given HTML element based on a valid CSS selector.

### Sample video

Below is a sample screen taken from a demo video. On the left, you can see what an app looks like normally. On the right is a screenshot taken with the HTML elements hidden.

<div>
<img style="float:left" src="../../img/ios/hidden_views/iphone-with-fields.png" width="400" />
<img style="float:left" src="../../img/ios/hidden_views/iphone-no-fields.png" width="400" />
</div>

<br style="clear: both" /> 

### Notes

* Elements are hidden from screenshots before they are uploaded.
* You may use `hideWebViewElements` with multiple comma separated selectors.
* You may add more selectors at any time by calling `hideWebViewElements` again.


### Related Articles

* [Hiding views in iOS](https://docs.testfairy.com/iOS_SDK/Hiding_views_from_video.html)
* [Hiding views in Android](https://docs.testfairy.com/iOS_SDK/Hiding_views_from_video.html)
