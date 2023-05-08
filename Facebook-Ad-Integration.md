# Facebook Ad Integration in Easiest Way
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104661326724333568/my-dp.jpg" align="right"
      width="140" height="178">
    
> Provider **Sabbir MMS**
- BANNER AD
- INTERSTITIAL VIDEO AD

## Output
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1105050166253326429/1683534627692.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512"> 
     <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1105050166001680435/1683534627688.jpg"
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
