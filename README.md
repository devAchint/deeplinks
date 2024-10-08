# Deep Links and App Links in Android

This guide explains how to implement both **Deep Links** (using custom URI schemes) and **App Links** (using HTTPS) in Android apps.

## 1. Deep Links Overview
Deep Links allow users to navigate to specific screens within your app from external sources like websites, messages, or emails. There are two types:
- **Custom Scheme Deep Links** (e.g., `app://some/path`)
- **App Links using HTTPS** (e.g., `https://www.example.com/some/path`)

## 2. Custom Scheme Deep Links

### Definition
A custom scheme is a URI you define for your app (e.g., `app://`). It lets your app handle links with that scheme.

### Setup
To implement deep linking with a custom scheme, define the following in your `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="app" />
    </intent-filter>
</activity>
```
Notes
Custom schemes like app:// are treated as plain text in apps like WhatsApp and Messages, so they may not always be clickable.
Best used in closed environments (within apps or custom QR codes).

3. App Links (Using HTTPS)
Definition
App Links use HTTPS URLs and allow your app to handle web URLs natively. Unlike custom schemes, these links are recognized across browsers and apps.

Setup
Add the following to your AndroidManifest.xml:
```
<activity android:name=".MainActivity">
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="https" android:host="www.example.com" />
    </intent-filter>
</activity>
```
Host a file named assetlinks.json at https://www.example.com/.well-known/assetlinks.json:
```
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.example",
    "sha256_cert_fingerprints":
    ["14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"]
  }
}]
```
google reference https://developer.android.com/training/app-links/verify-android-applinks

Notes
App Links require hosting and domain verification using the assetlinks.json file.
Use autoVerify to ensure the links open directly in your app when clicked.

4. Testing Deep Links
Custom Scheme Testing:
Use ADB to test:

bash
```
>adb shell am start -W -a android.intent.action.VIEW -d "quiz://some/path" com.example.quiz
```
App Links Testing:
Open the URL in a browser:
https://www.example.com/some/path

5. Conclusion
Use Custom Schemes for internal linking (e.g., within your app).
Use App Links for a seamless experience with web URLs and to handle HTTPS links.
