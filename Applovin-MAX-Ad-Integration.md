# Applovin MAX Ad Integration in Easiest Way
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104661326724333568/my-dp.jpg" align="right"
      width="140" height="178">
    
> Provider **Sabbir MMS**
- BANNER AD {Via XML & via programmactically-(commented)}
- INTERSTITIAL VIDEO AD

## Output
[<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1109770332119777290/Screenshot_2023-05-21-15-09-29-399_com.mrmms.applovinchecking.jpg"
     width="256" height="512">] | 
     <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1109770332300128328/Screenshot_2023-05-21-15-09-53-459_com.mrmms.applovinchecking.jpg"
      width="256" height="512">]


## Library Implementation
> _Implement the latest library if avilable **[Alt+Enter]** while you are integrating the SDk_

```gradle
//Applovin MAX
implementation 'com.applovin:applovin-sdk:11.9.0'
```

## AndroidManifest.xml
> Uses Permissions _ above Application Tag
 * Internet Permission
 * Ad ID Permission
     * in case targetSdk 33++

```xml
 <uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
    <uses-permission android:name="android.permission.INTERNET"/>
```
> Meta data => SDK Key

* Between Application Tag
   * SDK key path (Applovin > Account > keys > SDK Key) ✔
   * Or
   * [Follow this link](https://dash.applovin.com/documentation/mediation/android/getting-started/integration#:~:text=Add%20the%20SDK,%3Capplication%3Eelement%3A)
   * This will directly redirect you with attacted sdk key 
   * Note: You must log in to your AppLovin account to get your SDK key generated.
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1109774254649651250/sdkMAx.png"
      width="1000" height="150">
```xml
<!-- <application

_______ -->
<meta-data android:name="applovin.sdk.key"
            android:value="Your_SDK_Key"/>
<!--</application>-->
```

## activity_main.xml

> _Just copy and paste it to your project (Modify if necessary)_

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Interstitial"
        app:layout_constraintBottom_toTopOf="@+id/adView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <!--Banner Creating by XML
    To create programmatically make
    comment MaxAdView and uncomment below LinearLayout-->
    <com.applovin.mediation.ads.MaxAdView
        xmlns:maxads="http://schemas.applovin.com/android/1.0"
        maxads:adUnitId="YOUR_BANNER_ID"
        android:id="@+id/adView"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        />

    <!--Banner Creating programmatically  -->
    <!--<LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:id="@+id/adContainer"
        app:layout_constraintBaseline_toTopOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        android:visibility="gone"
        />-->
    
</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.java

> _Just copy and paste it to your project (Modify if necessary)_
#### **Required** 

-  Ad Unit Ids
-  implements MaxAdListener ``with AppCompatActivity => public class (follow the given screenshot below)`` 

# ![](https://cdn.discordapp.com/attachments/1104657948866773063/1109776697844305950/implement.png)

```java

public class MainActivity extends AppCompatActivity implements MaxAdListener {
    private MaxAdView adView;
    private MaxInterstitialAd interstitialAd;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        MaxInitializeSdk();
        MaxBannerLoad();
        MaxInterstitialLoad();

        findViewById(R.id.button).setOnClickListener(v -> {
            if ( interstitialAd.isReady() ) interstitialAd.showAd();
            else Toast.makeText(MainActivity.this, "Interstitial Ad Load Failed", Toast.LENGTH_SHORT).show();
        });
    }//OnCreate Method Finish
    
    private void MaxInitializeSdk(){
        AppLovinSdk.getInstance( this ).setMediationProvider( "max" );
        AppLovinSdk.initializeSdk( this, new AppLovinSdk.SdkInitializationListener() {
            @Override
            public void onSdkInitialized(final AppLovinSdkConfiguration configuration)
            {
                // AppLovin SDK is initialized, start loading ads
            }
        } );
    }//Max Sdk Initialization
    
    private void MaxBannerLoad(){
        /* Todo: Banner Creating by XML
        To create programmatically make
        comment next 2 lines and uncomment below commented codes*/
        adView= findViewById(R.id.adView);
        adView.loadAd();

        //*Todo: Banner Creating programmatically*///
        /*adView= new MaxAdView("YOUR_BANNER_ID",this);
        LinearLayout adContainer=findViewById(R.id.adContainer);
        adContainer.setVisibility(View.VISIBLE);
        adContainer.addView(adView);
        adView.loadAd();*/

        adView.setListener(new MaxAdViewAdListener() {
            @Override
            public void onAdExpanded(MaxAd maxAd) {
                Toast.makeText(getApplicationContext(), "onAdExpanded", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdCollapsed(MaxAd maxAd) {
                Toast.makeText(getApplicationContext(), "onAdCollapsed", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdLoaded(MaxAd maxAd) {
                Toast.makeText(getApplicationContext(), "onAdLoaded", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdDisplayed(MaxAd maxAd) {
                Toast.makeText(getApplicationContext(), "onAdDisplayed", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdHidden(MaxAd maxAd) {
                Toast.makeText(getApplicationContext(), "onAdHidden", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdClicked(MaxAd maxAd) {
                Toast.makeText(getApplicationContext(), "onAdClicked", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdLoadFailed(String s, MaxError maxError) {

                Toast.makeText(getApplicationContext(), "onAdLoadFailed", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdDisplayFailed(MaxAd maxAd, MaxError maxError) {
                Toast.makeText(getApplicationContext(), "onAdDisplayFailed", Toast.LENGTH_SHORT).show();
            }
        });

    }//Max Banner

    private void MaxInterstitialLoad(){
        interstitialAd = new MaxInterstitialAd( "YOUR_INTERSTITIAL_ID", this );
        interstitialAd.setListener( this );
        // Load the first ad
        interstitialAd.loadAd();
    }//Max Interstitial
    
    @Override
    public void onAdLoaded(MaxAd maxAd) {
        Toast.makeText(this, "onAdLoaded", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onAdDisplayed(MaxAd maxAd) {
        Toast.makeText(this, "onAdDisplayed", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onAdHidden(MaxAd maxAd) {
        Toast.makeText(this, "onAdHidden", Toast.LENGTH_SHORT).show();
        interstitialAd.loadAd();
    }

    @Override
    public void onAdClicked(MaxAd maxAd) {
        Toast.makeText(this, "onAdClicked", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onAdLoadFailed(String s, MaxError maxError) {
        Toast.makeText(this, "onAdLoadFailed", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onAdDisplayFailed(MaxAd maxAd, MaxError maxError) {
        Toast.makeText(this, "onAdDisplayFailed", Toast.LENGTH_SHORT).show();
    }//MAX Ads integration finished
    
}//public class finish
```
## Credit for this documentation
> Monjel Morshed Sabbir
* Social Media
    * [Facebook](https://www.facebook.com/DeveloperMrMMS/)
    * [Facebook](https://www.facebook.com/Sabbir.MMS/)
    * [Instagram](https://www.instagram.com/sabbirmms/)
    * [Youtube](https://www.youtube.com/@DeveloperMrMMS)
    * [Linkedin](https://www.linkedin.com/in/sabbirmms/)
# ![](https://cdn.discordapp.com/attachments/1104657948866773063/1104658590595301487/cover.jpg)
