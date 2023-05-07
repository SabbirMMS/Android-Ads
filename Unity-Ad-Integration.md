# Unity Ad Integration in Easiest Way
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104661326724333568/my-dp.jpg" align="right"
      width="140" height="178">
    
> Provider **Sabbir MMS**
- BANNER AD
- INTERSTITIAL VIDEO AD

## Output
[<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104658590926635099/unity_M1.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512">] | 
     <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104658591232835584/unity_M3.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512">]


## Library Implementation
> _Implement the latest library if avilable **[Alt+Enter]** while you are integrating the SDk_

```gradle
implementation 'com.unity3d.ads:unity-ads:4.7.0'
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
    android:padding="20dp"
    tools:context=".MainActivity">
    <RelativeLayout
        android:layout_width="320dp"
        android:layout_height="50dp"
        android:id="@+id/topBannerView"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/loadBottomBanner"
        android:layout_centerInParent="true"
        android:text="Show Interstitial Ad" />
    <RelativeLayout
        android:layout_width="320dp"
        android:layout_height="50dp"
        android:id="@+id/bottomBanner"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true" />
</RelativeLayout>
```

## MainActivity.java

> _Just copy and paste it to your project (Modify if necessary)_
#### **Required** 
- Game Id
-  Ad Unit Ids
-  implements IUnityAdsInitializationListener ``with AppCompatActivity => public class (follow the given screenshot below)`` 

# ![](https://cdn.discordapp.com/attachments/1104657948866773063/1104658591593537576/unity1_InitializeListener.png)

```java

public class MainActivity extends AppCompatActivity implements IUnityAdsInitializationListener {

    private String unityGameID = "14851"; /** Your Game Id Here*/
    private Boolean testMode = true;
    String topAdUnitId = "Banner_Android";
    String bottomAdUnitId = "Banner_Android";
    private String adUnitId = "Interstitial_Android";
    BannerView bottomBanner,topBanner;
    RelativeLayout bottomBannerView,topBannerView;
    private boolean adIsReady = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //Top Banner Ad
        UnityAds.initialize(getApplicationContext(), unityGameID, testMode, this);
        topBanner = new BannerView(this, topAdUnitId, new UnityBannerSize(320, 50));
        topBanner.setListener(bannerListener);
        topBannerView = findViewById(R.id.topBannerView);
        topBanner.load();
        topBannerView.addView(topBanner);

        //Bottom Banner Ad
        bottomBanner = new BannerView(this, bottomAdUnitId, new UnityBannerSize(320, 50));
        bottomBanner.setListener(bannerListener);
        bottomBannerView = findViewById(R.id.bottomBanner);
        bottomBanner.load();
        bottomBannerView.addView(bottomBanner);

        //Interstitial Ad
        UnityAds.load(adUnitId, loadListener);
        findViewById(R.id.loadBottomBanner).setOnClickListener(v -> {
            if (adIsReady)
                UnityAds.show(MainActivity.this, adUnitId, new UnityAdsShowOptions(), showListener);
            else Toast.makeText(this, "Ad is not ready yet", Toast.LENGTH_SHORT).show();
        });
    }// End of on create method

    // Listener for banner events:
    private BannerView.IListener bannerListener = new BannerView.IListener() {
        @Override
        public void onBannerLoaded(BannerView bannerAdView) {
            Toast.makeText(getApplicationContext(), "Banner Ad Loaded", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onBannerFailedToLoad(BannerView bannerAdView, BannerErrorInfo errorInfo) {
            Toast.makeText(getApplicationContext(), "Banner Ad Failed to Load", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onBannerClick(BannerView bannerAdView) {
            Toast.makeText(getApplicationContext(), "Banner Ad Clicked", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onBannerLeftApplication(BannerView bannerAdView) {
            Toast.makeText(getApplicationContext(), "Banner Ad Left Application", Toast.LENGTH_SHORT).show();
        }
    }; //End of banner event

    //Interstitial loading listener
    private IUnityAdsLoadListener loadListener = new IUnityAdsLoadListener() {
        @Override
        public void onUnityAdsAdLoaded(String placementId) {
            adIsReady = true;
        }
        @Override
        public void onUnityAdsFailedToLoad(String placementId, UnityAds.UnityAdsLoadError error, String message) {
            adIsReady = false;
        }
    };//End of interstitial loading listener

    //Interstitial ad show listener
    private IUnityAdsShowListener showListener = new IUnityAdsShowListener() {
        @Override
        public void onUnityAdsShowFailure(String placementId, UnityAds.UnityAdsShowError error, String message) {
            Toast.makeText(getApplicationContext(), "onUnityAdsShowFailure", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onUnityAdsShowStart(String placementId) {
            Toast.makeText(getApplicationContext(), "onUnityAdsShowStart", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onUnityAdsShowClick(String placementId) {
            Toast.makeText(getApplicationContext(), "onUnityAdsShowClick", Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onUnityAdsShowComplete(String placementId, UnityAds.UnityAdsShowCompletionState state) {
            Toast.makeText(getApplicationContext(), "onUnityAdsShowComplete", Toast.LENGTH_SHORT).show();
            adIsReady=false;
            UnityAds.load(adUnitId, loadListener);
        }
    };//End of interstitial ad show listener

    //SDK initialization override methods
    @Override
    public void onInitializationComplete() {
    }
    @Override
    public void onInitializationFailed(UnityAds.UnityAdsInitializationError error, String message) {
    }
}//End of public class
```
## Credit for this documentation
> Monjel Morshed Sabbir
* Social Media
    * [Facebook](https://www.facebook.com/MonjelMorshedSabbir.MMS)
    * [Instagram](https://www.instagram.com/sabbirmms/)
    * [Youtube](https://www.youtube.com/@DeveloperMrMMS)
    * [Linkedin](https://www.linkedin.com/in/sabbirmms/)
# ![](https://cdn.discordapp.com/attachments/1104657948866773063/1104658590595301487/cover.jpg)
