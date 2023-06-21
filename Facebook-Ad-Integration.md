# Facebook Ad Integration in Easiest Way
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104661326724333568/my-dp.jpg" align="right"
      width="140" height="178">
    
> Provider **Sabbir MMS**
- BANNER AD
- INTERSTITIAL VIDEO AD
- REWARDED VIDEO AD (another activity)

## Output
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1105050166253326429/1683534627692.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512"> 
     <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1105050166001680435/1683534627688.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512">
     <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1121062504378941490/Screenshot_2023-06-21-19-01-31-680_com.mms.testfbreward.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512">

## Properties Setup
> gradle.properties

```properties

android.enableJetifier=true
```

## Library Implementation 
> _Implement the latest library if avilable **[Alt+Enter]** while you are integrating the SDk_

> build.gradle (Module:app)

```gradle
implementation 'com.facebook.android:audience-network-sdk:6.14.0'
```

## AndroidManifest.xml
> Uses Permissions
 * Internet Permission
 * Ad ID Permission
     * in case targetSdk 33++

```xml
 <uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
    <uses-permission android:name="android.permission.INTERNET"/>
```

## activity_main.xml

> _Just copy and paste it to your project (Modify if necessary)_

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"
    tools:context=".MainActivity">
    <LinearLayout
        android:id="@+id/banner_container"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_marginBottom="100dp"
        android:background="#F3CFCF"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent" />
    <Button
        android:id="@+id/btShowAd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Show Ads" />
</RelativeLayout>
```

## MainActivity.java

> _Just copy and paste it to your project (Modify if necessary)_
#### **Required** 
- Ad Unit ID
- For testing we are using test ID

```java

public class MainActivity extends AppCompatActivity {
    private AdView adView;
    private InterstitialAd interstitialAd;
    String FB_BANNER_UNIT = "IMG_16_9_APP_INSTALL#YOUR_PLACEMENT_ID", FB_INTERSTITIAL_UNIT = "IMG_16_9_APP_INSTALL#YOUR_PLACEMENT_ID";
    LinearLayout adContainer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        BannerAds();
        LoadIntAd();
        
        //Showing Interstitial Ad on Click
        findViewById(R.id.btShowAd).setOnClickListener(v -> {
            if (interstitialAd == null || !interstitialAd.isAdLoaded())
                Toast.makeText(this, "interstitialAd is not ready yet", Toast.LENGTH_SHORT).show();
            else interstitialAd.show();
        });

    }//OnCreate End Mark

    //Banner Ads Load, Show and listener
    private void BannerAds() {
        adView = new AdView(this, FB_BANNER_UNIT, AdSize.BANNER_HEIGHT_50);
        adContainer = findViewById(R.id.banner_container);
        adContainer.addView(adView);

        AdListener adListener = new AdListener() {
            @Override
            public void onError(Ad ad, AdError adError) {
                Toast.makeText(MainActivity.this, "Error: " + adError.getErrorMessage(), Toast.LENGTH_LONG).show();
            }
            @Override
            public void onAdLoaded(Ad ad) {
                Toast.makeText(MainActivity.this, "Banner onAdLoaded", Toast.LENGTH_SHORT).show();
            }
            @Override
            public void onAdClicked(Ad ad) {
                Toast.makeText(MainActivity.this, "Banner onAdClicked", Toast.LENGTH_SHORT).show();
            }
            @Override
            public void onLoggingImpression(Ad ad) {
                Toast.makeText(MainActivity.this, "Banner onLoggingImpression", Toast.LENGTH_SHORT).show();
            }
        };

        // Requesting and Loading Banner Ad
        adView.loadAd(adView.buildLoadAdConfig().withAdListener(adListener).build());
    }//Banner Ads Load, Show and listener

    //Loading Interstitial Ad here
    private void LoadIntAd() {
        AudienceNetworkAds.initialize(this);

        interstitialAd = new InterstitialAd(this, FB_INTERSTITIAL_UNIT);

        interstitialAd.loadAd(interstitialAd.buildLoadAdConfig()
                .withAdListener(interstitialAdListener)
                .build());
    }//Loading Interstitial Ad here

    // Create listeners for the Interstitial Ad
    InterstitialAdListener interstitialAdListener = new InterstitialAdListener() {
        @Override
        public void onInterstitialDisplayed(Ad ad) {
            Toast.makeText(MainActivity.this, "Inter onInterstitialDisplayed", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onInterstitialDismissed(Ad ad) {
            Toast.makeText(MainActivity.this, "Inter onInterstitialDismissed", Toast.LENGTH_SHORT).show();
            //Interstitial Ad Callback
            LoadIntAd();
        }
        @Override
        public void onError(Ad ad, AdError adError) {
            Toast.makeText(MainActivity.this, "Inter onError", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onAdLoaded(Ad ad) {
            Toast.makeText(MainActivity.this, "Inter onAdLoaded", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onAdClicked(Ad ad) {
            Toast.makeText(MainActivity.this, "Inter onAdClicked", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onLoggingImpression(Ad ad) {
            Toast.makeText(MainActivity.this, "Inter onLoggingImpression", Toast.LENGTH_SHORT).show();
        }
    };// Create listeners for the Interstitial Ad

    @Override
    protected void onDestroy() {
        if (adView != null) {
            adView.destroy();
        }
        if (interstitialAd != null) {
            interstitialAd.destroy();
        }
        super.onDestroy();
    }//On destroy method

}//AppCompatActivity ends here
```

## Rewarded Video Ad 
> Showing the ad auto after loaded

> Must add unique test device ID from logcat to load test ad
```java

public class MainActivity extends AppCompatActivity {

    private RewardedVideoAd rewardedVideoAd;
    String TAG="null";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        findViewById(R.id.tvhello).setOnClickListener(v -> check());

    }

    private void check(){

        // Initialize the Audience Network SDK
        AudienceNetworkAds.initialize(this);
        AdSettings.addTestDevice("08bc8bd5-2815-4842-a9b0-b68fa9011476");
        AdSettings.addTestDevice("cee833d8-84d2-4242-8504-7edd585b11c7");
        // Instantiate a RewardedVideoAd object.
        // NOTE: the placement ID will eventually identify this as your App, you can ignore it for
        // now, while you are testing and replace it later when you have signed up.
        // While you are using this temporary code you will only get test ads and if you release
        // your code like this to the Google Play your users will not receive ads (you will get
        // a no fill error).
        rewardedVideoAd = new RewardedVideoAd(this, "YOUR_PLACEMENT_ID");
        RewardedVideoAdListener rewardedVideoAdListener = new RewardedVideoAdListener() {
            @Override
            public void onError(Ad ad, AdError error) {
                // Rewarded video ad failed to load
                Log.e(TAG, "Rewarded video ad failed to load: " + error.getErrorMessage());
                Toast.makeText(getApplicationContext(), "Rewarded video ad failed to load: " + error.getErrorMessage(), Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdLoaded(Ad ad) {
                // Rewarded video ad is loaded and ready to be displayed
                Log.d(TAG, "Rewarded video ad is loaded and ready to be displayed!");
                Toast.makeText(getApplicationContext(), TAG, Toast.LENGTH_SHORT).show();
                  //Showing the ad auto after loaded
                rewardedVideoAd.show();
            }

            @Override
            public void onAdClicked(Ad ad) {
                // Rewarded video ad clicked
                Log.d(TAG, "Rewarded video ad clicked!");
                Toast.makeText(getApplicationContext(), TAG, Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onLoggingImpression(Ad ad) {
                // Rewarded Video ad impression - the event will fire when the
                // video starts playing
                Log.d(TAG, "Rewarded video ad impression logged!");
                Toast.makeText(getApplicationContext(), TAG, Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onRewardedVideoCompleted() {
                // Rewarded Video View Complete - the video has been played to the end.
                // You can use this event to initialize your reward
                Log.d(TAG, "Rewarded video completed!");
                Toast.makeText(getApplicationContext(), TAG, Toast.LENGTH_SHORT).show();

                // Call method to give reward
                // giveReward();
            }

            @Override
            public void onRewardedVideoClosed() {
                // The Rewarded Video ad was closed - this can occur during the video
                // by closing the app, or closing the end card.
                Log.d(TAG, "Rewarded video ad closed!");
                Toast.makeText(getApplicationContext(), TAG, Toast.LENGTH_SHORT).show();
            }
        };
        rewardedVideoAd.loadAd(
                rewardedVideoAd.buildLoadAdConfig()
                        .withAdListener(rewardedVideoAdListener)
                        .build());

    }

}
```
### On Error - or Not Working
* If test network is not working follow the steps below
   * go to facebook app ->
   * settings -> (search)
   * Ad preferences ->
   * Ad settings ->
   * Activity information from ad partners ->
   * Review settings ->
   * here might be selected
   > ``No, don't make my ads more relevant by using this information`` **Select the option above**
   > ``Yes, show me ads that are more relevant by using this information``
* if it's still not working
   * please change the debugging device and try again
   
## Credit for this documentation
> Monjel Morshed Sabbir
* Social Media
    * [Facebook](https://www.facebook.com/DeveloperMrMMS/)
    * [Facebook](https://www.facebook.com/Sabbir.MMS/)
    * [Instagram](https://www.instagram.com/sabbirmms/)
    * [Youtube](https://www.youtube.com/@DeveloperMrMMS)
    * [Linkedin](https://www.linkedin.com/in/sabbirmms/)
# ![](https://cdn.discordapp.com/attachments/1104657948866773063/1104658590595301487/cover.jpg)
