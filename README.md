# Password Stealer Demo

This app demonstrates how an app can use JavaScript to steal your password in a log in form. It specifically looks for Google's password box (specific minified class name). If you log into your account, it should show your password in a toast at the bottom of the screen. This could be stored without showing the user at all.

### How it works

The WebViewClient's method `shouldOverrideUrlLoading` is used as a sort of listener to call the method `getPassword` every time a URL is loaded. `getPassword` runs a JavaScript snippet on the view which gets all the elements of class `"whsOnd zHQkBf"` (the minified class name of Google's input fields) and returns the value of the first element with input type `password`. When the JS finishes evaluating, the retrieved password (if found) is displayed via a toast. `shouldOverrideUrlLoading` then returns false and the webview continues as normal. This demo does not save your password, but a malicious application easily could without the user knowing.

If Google's login doesn't immediately redirect you to another page (maybe for two factor authentication), then `shouldOverrideUrlLoading` will not get called and no password will be retrieved. I'd imagine there is some other way to force the JS to get exectuted or even have it run in a loop on every page, but I haven't tried.

One benefit of `shouldOverrideUrlLoading` being used to run the JavaScript is that the password is only stolen if the password is correct (since it took them to the next page). This makes the stolen information valueable 100% of the time.

### Why this is scary

 - It uses the official Google login form. A smart user may think it's fine since it's Google's site, but may not notice that the WebView is hosted in the app. Further tricks can probably be used to make the activity appear to be chrome or whatever.
 - It's difficult to detect without seeing the source code. The moral of the story is to never type your password into a third party's browser.
