# Deprecation Notice

As the casual onlooker can tell this repo hasn't been updated in a while.  Unfortunately this project is deprecated.  If anyone would like to fork it and address issues please do so but PRs won't be reviewed and merged into this repo.  Cordova is not simple to test (or at least wasn't when this was maintained) and I don't have the time to contribute to maintaining it.

If the repos is forked and maintained let me know and we'll link to your repo here.

- Brad Reynolds

---
UIWebView removed! 

---

The remote injection plugin allows a remote site to interact with [cordova](https://cordova.apache.org)'s javascript APIs when loaded within your cordova app.  When compared with a cordova app that packages its HTML the downside to loading a remote site is if the network is down your app is down.  This can be an acceptable trade off if your site has a heavy dependency on web services.  The advantage to this approach is the build, deployment, and previous investment of an existing site doesn't need to change and can continue to work for browser based users providing a smooth transition to an app which provides tighter native integration.

## Features
* Injects cordova and installed plugin JS into the webview for any remotely browsed page allowing them the same access to the cordova object and its plugins as a packaged cordova app.
* After a developer defined interval will prompt the user if the loading of the main site is taking too long.  The user's options are to wait or retry.  If the user waits the prompt will be displayed again after the developer defined interval.  If the site loads while the dialog is displayed the dialog is dismissed.
* Support for iOS and Android platforms.
* Supports both UIWebView and WKWebView on iOS out of the box (added in v0.4.0).

## Installation
```bash
cordova plugin add cordova-plugin-remote-injection
```

## Configuration
Configuration is done via preferences in your config.xml.

### CRIInjectFirstFiles
```xml
<preference name="CRIInjectFirstFiles" value="www/js/init.js" />
```

<dl>
<dt>Type</dt><dd>String</dd>
<dt>Default</dt><dd>none</dd>
</dl>

List of paths to JS files within the project to inject before injecting cordova into the remote site.  To inject multiple separate the files with a ",".

<preference name="CRIInjectFirstFiles" value="www/js/file1.js,www/js/file2.js" />

### CRIPageLoadPromptInterval
```xml
<preference name="CRIPageLoadPromptInterval" value="5" />
```

<dl>
<dt>Type</dt>
<dd>int</dd>
<dt>Default</dt><dd>10</dd>
</dl>

If the site hasn't loaded after this interval the user will be provided a choice to continue waiting or to retry loading the site.  This is turned on by default.  If not wanting the prompt the user set the value to 0.

### CRIShowConnectionErrorDialog
```xml
<preference name="CRIShowConnectionErrorDialog" value="0" />
```

<em>iOS only</em>

<dl>
<dt>Type</dt>
<dd>int</dd>
<dt>Default</dt><dd>1</dd>
</dl>

By default the iOS app will display a dialog if there is a failure to connect to the site.  This creates issues if wanting to use Cordova's <code>ErrorUrl</code> config to handle connection errors.  "0" will turn off displaying of the dialog and allow the <code>ErrorUrl</code> config to kick in.

## FAQ

#### 1. Will Apple approve the app if it just wraps a site?  Point 2.12 in their [guidelines](https://developer.apple.com/app-store/review/guidelines/#functionality) states:

`2.12 Apps that are not very useful, unique, are simply web sites bundled as Apps, or do not provide any lasting entertainment value may be rejected`

I make no promises for your app but our app was approved by Apple and is in the App Store.  We feel we enhanced our site with the app and are not simply bundling it.  We display the website but also provide notifications to our users for important events, badge count updates for alerts, etc.  Our app is also specifically for the customers of our company and not general use so that may have been a factor in their decision although all we can do is speculate.

#### 2. Will the plugin work with CSP?

[Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/Security/CSP) (CSP) is a cool thing.  For the plugin to work you'll need to allow javascript to be executed and javascript data URIs.

The default policy generated by the cordova CLI will work with this plugin.  At the point in time this was written the policy was:

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: https://ssl.gstatic.com 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src *">
```

The key to making the plugin work on Android is to ensure that you allow data URIs to be executed in the origin of which the page is served.

The most restrictive way to specify this is:
```
script-src 'self' data:;
```

For more on the topic see [issue #6](https://github.com/TruckMovers/cordova-plugin-remote-injection/issues/6).

## Support

Log issues on [github](https://github.com/TruckMovers/cordova-plugin-remote-injection) and we'll get in contact.
