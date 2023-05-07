# Unity Ad Integration in Easiest Way
<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104661326724333568/my-dp.jpg" align="right"
     alt="Size Limit logo by Anton Lovchikov" width="140" height="178">
    
> Provider **Sabbir MMS**
## Output
[<img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104658590926635099/unity_M1.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512">] | 
     <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104658590926635099/unity_M1.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512"> | <img src="https://cdn.discordapp.com/attachments/1104657948866773063/1104658590926635099/unity_M1.jpg"
     alt="Size Limit logo by Anton Lovchikov" width="256" height="512">]]


## Library Implementation
> _Implement the latest library if avilable **[Alt+Enter]** while you are integrating the SDk_

```yaml
implementation 'com.unity3d.ads:unity-ads:4.7.0'
```


## activity_main.xml

> _Just copy and paste it to your project (Modify if necessary)_

```yaml
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
