# Android Consent SDK Helper
Android Consent SDK Helper is a library that helps you to integrate and use [Google Consent SDK Android](https://github.com/googleads/googleads-consent-sdk-android) library easily.

You can check the [sample](https://github.com/Swisyn/Android-Consent-SDK-Helper/tree/master/sample "Sample App")
 for usage of this library.


## Video
 
[![Android-Consent-SDK-Helper](https://img.youtube.com/vi/OZaq1Ljqge8/0.jpg)](https://youtu.be/OZaq1Ljqge8 "Android-Consent-SDK-Helper")

## Usage

#### Gradle

Add dependency in your app-level build.gradle

```groovy
implementation 'com.cuneytayyildiz:consent-sdk-helper:1.0.2'
```

<b>The constructor has 6 parameters, first 3 of them are mandatory.</b>
```kotlin
 class ConsentSDKHelper(
 private val context: Context, // An activity or application context
 private val publisherId: String, // Publisher ID from AdMob, Go to AdMob -> Left Menu -> Settings -> Under the Account information section
 private val privacyPolicyURL: String, // Privacy Policy URL for your app
 private val admobTestDeviceId: String = "", // Check the logcat to get a test device id 
 private var DEBUG: Boolean = false, // You can use BuildConfig.DEBUG to test consent dialog
 private var isEEA: Boolean = false // To check how consent library acts if user within EU region or not.
 )
```

#### Implementation

> Simply call the **checkConsent()**

```kotlin
ConsentSDKHelper(this, publisherId, policyURL).checkConsent()
```

> or

```kotlin
 val consentSDK = ConsentSDKHelper(this, publisherId, policyURL)
        consentSDK.checkConsent(object : ConsentCallback {
            override fun onResult(isRequestLocationInEeaOrUnknown: Boolean) {
                // do something with the result
            }
        })
```
---

#### To generate ad requests

After you have implemented this library, you should use **ConsentSDKHelper.getAdRequest** function to create new ad requests. 
The function has 3 parameters including 2 optionals to get test ads.
```kotlin
ConsentSDKHelper.getAdRequest(
context: Context, 
isDebug: Boolean = false, // To get test ads BuildConfig.DEBUG can be used.
admobTestDeviceId: String = "" // Check the logcat to get a test device id 
):
```
 
**for production**
```kotlin
    // load a banner ad
    adView.loadAd(ConsentSDKHelper.getAdRequest(context));
    // load a Interstitial ad
    interstitialAd.loadAd(ConsentSDKHelper.getAdRequest(context));
```

**for development**
```kotlin
    // load a banner ad
    adView.loadAd(ConsentSDKHelper.getAdRequest(context, BuildConfig.DEBUG, admobTestDeviceId));
    // load a Interstitial ad
    interstitialAd.loadAd(ConsentSDKHelper.getAdRequest(context, BuildConfig.DEBUG, admobTestDeviceId));
```

#### Change or revoke consent
There is also a preference, to allow users to update their consent just like the Google App. 

I have translated all the languages spoken in European Region, all pull requests are welcome to make translations clear.:heart:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.preference.PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <com.cuneytayyildiz.android.consent.sdk.helper.preference.ConsentSDKHelperPreference
        android:key="preference_ad_choice"
        app:csdk_privacy_policy_url="@string/admob_publisher_id"
        app:csdk_publisher_id="@string/privacy_policy_url" />

</android.support.v7.preference.PreferenceScreen>
```

**Publisher ID and Privacy Policy URL attributes must not be empty**
```xml
   <declare-styleable name="ConsentSDKHelperPreference">
        <attr name="csdk_publisher_id" format="string" />
        <attr name="csdk_privacy_policy_url" format="string"/>
        <attr name="csdk_title_size" format="dimension"/>
    </declare-styleable>
```

### License
MIT License

Copyright (c) 2018 Cüneyt AYYILDIZ

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---
#### More details:
  [Requesting Consent from European Users](https://developers.google.com/admob/android/eu-consent)
