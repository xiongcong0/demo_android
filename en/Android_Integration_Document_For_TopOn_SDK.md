# TopOn Android SDK

[1. Introduction](#1)<br>
[2. Integrating the TopOn SDK](#2)<br>
[3. Initializing the TopOn SDK](#3)<br>
[4. Native](#4)<br>
[5. NativeBanner](#5)<br>
[6. NativeSplash](#6)<br>
[7. RewardedVideo](#7)<br>
[8. Interstitial](#8)<br>
[9. Banner](#9)<br>
[10. Splash](#10)<br>
[11. GDPR](#11)<br>
[12. HeadBidding](#12)<br>


<h2 id='1'>1. TopOn introduction</h2>

This document mainly introduces how to integrate TopOn SDK in Android project. TopOn SDK provides integration in the form of AAR, Jar packages, including third-party advertising platform SDKs, all in the form of AAR, Jar packages. **In the integration process, you need to be careful not to miss the integration of third-party SDK packages, otherwise there will be no advertising resources after integration.**<br>



<h2 id='2'>2.  Integrating the TopOn SDK </h2>

<h3>2.1 SDK download address</h3>

[Baidu network disk download address]( https://pan.baidu.com/s/1bz6wDl-sdZgjpoep5pbzag)：password：7ukt<br>
[Google network disk download address](https://drive.google.com/drive/folders/1yBvuKY61aa6WzBtGBr4BE04vDH6EZqtj?usp=sharing)

TopOn SDK Demo：[TopOn SDK Demo&SDK](https://github.com/uparputeam/uparpu_demo_android)

<h3>2.2 Directory description of the SDK package</h3>

**libs:** There are basic package of TopOn SDK under this directory, and **introduced according to the form of advertising that you needs to be integrated**. <br>
**network_sdk:** There are all package of third-party network under this directory，**introduced according to the form of advertising that you needs to be integrated**

<h4>2.2.1 SDK package integration instructions for the libs directory</h4>

| SDK package | Description | Necessary |
| --- | --- |---|
|uparpu_core.aar|TopOn basic package，must imported| **Yes** |
|uparpu_native.aar|TopOn integration package for native| No |
|uparpu_banner.aar|TopOn integration package for banner|No|
|uparpu_interstitial.aar|TopOn integration package for interstitial|No|
|uparpu_rewardvideo.aar|TopOn integration package for rewarded video|No|
|uparpu_splash.aar|TopOn integration package for splash|No|
|uparpu_headbiding.aar|TopOn integration package for headbidding, it is a package that aggregates third-party SDK head bids. For details, please see [HeadBidding Instructions](#12)|No|

<h4>2.2.2 Description of the network_sdk directory</h4>

| Directory       | Description              |
| --------------- | ------------------------ |
| adcolony        | AdColony SDK             |
| admob           | Admob SDK                |
| applovin        | Applovin SDK             |
| appnext         | Appnext SDK              |
| baidu           | Baidu SDK(China)         |
| chartboost      | Chartboost SDK           |
| facebook        | Facebook SDK             |
| flurry          | Flurry SDK               |
| gdt             | GDT SDK(China)           |
| inmobi          | Inmobi SDK               |
| ironsource      | Ironsource SDK           |
| ks              | Kuaishou SDK(China)      |
| ksyun           | Ksyun SDK(China)         |
| luomi           | Luomi SDK(China)）       |
| maio            | Maio SDK                 |
| mintegral_china | Mintegral SDK(China)     |
| mintegral_other | Mintegral SDK(Non-China) |
| mopub           | Mopub SDK                |
| nend            | Nend SDK                 |
| oneway          | Oneway SDK(China)        |
| startapp        | StartApp SDK             |
| superawesome    | SuperAwesome SDK         |
| tapjoy          | Tapjoy SDK               |
| toutiao         | Tiktok SDK(China)        |
| uniplay         | Uniplay SDK(China)       |
| unityads        | UnityAds SDK             |
| vungle          | Vungle SDK               |

According to the platform that you need to integrate, add SDK package to your Android project，description of the folder contents of each third-party SDK：

| Directory/file | Description |
|---|---|
|libs|The package in this directory must be integrated for the third-party network platform.|
|extra|The plugin package in this directory must be integrated for the third-party network platform. If the plugin package already exists in the project, there is no need to repeat the integration.|
|AndroidManifest.xml|Need to add the component information in the AndroidManifest to the AndroidManifest of your project (If the file does not exist, no configuration is required).|
|proguard-android.txt|Confusion configuration for the third-party network platform(If the file does not exist, no configuration is required).|
|res|The resources that must be imported for the third-party network platform(If no exist, nothing is required).|

**In the Gradle file, you need to add the configuration of the android-v7 package:**

```java
implementation 'com.android.support:appcompat-v7:28.0.0'
```

<h4>2.2.3 The configuration description of AndroidManifest</h4>

```java
 <application
        ...
        <!--Configuration for Http request for Android high version-->
        android:usesCleartextTraffic="true" 
        >
        ....
		<!--Configuration For Android 9.0 and above, it is used to adapt network request-->
        <uses-library android:name="org.apache.http.legacy" android:required="false"/>
        ....
<application>
```

The configuration of AndroidManifest for the third-party network platform, please refer to the AndroidManifest.xml under the platform folder in the **network_sdk** directory.

<h4>2.2.4 The configuration of confusion</h4>

```java
-dontwarn com.uparpu.**
-keep public class com.uparpu.network.**
-keepclassmembers class com.uparpu.network.** {
   public *;
}
```

The configuration of confusion for the third-party network platform, please refer to the proguard-android.txt under the platform folder in the **network_sdk** directory.



<h2 id='3'>3. Integrating the TopOn SDK </h2>

<h3>3.1 API</h3>

**UpArpuSDK**

| API | Parameter | Description |
| --- | --- |---|
| init | (Context context, String appId, String appKey) | Initialization method for TopOnSDK, where appId and appKey need to be acquired after the application is created in the TopOn background. <br/> **This method is recommended to initialize when Application onCreate**. |
| setChannel | (String channel) | Set channel information for TopOn background to distinguish ads data. |
| setSubChannel | (String subChannel) | Set sub channel information. |
| initCustomMap | (Map<String, String> customMap)| Custom key-value, which can be used to match the ads list information delivered by the server. |
| setGDPRUploadDataLevel|(Context context, int level) |Set the reporting level of the data for GDPR. The level is divided into the following three levels: <br/> **UpArpuSDK.PERSONALIZED**: The reported data contains device parameters.<br/> **UpArpuSDK.NONPERSONALIZED**: The reported data does not contain device parameters. < Br> **UpArpuSDK.FORBIDDEN**: Do not report any, stop ad request.|
|getGDPRDataLevel|(Context context)| Get the current reporting level                              |
|isEUTraffic|(Context context)| Judge if it is an EU region |
|showGdprAuth|(Activity activity)|Show the GDPR authorization page|
|setNetworkLogDebug|(boolean debug)|Set whether there is Debug log output. <br/>**It is recommended to open it during the test stage to verification of the callback status**|


<h3>3.2 Sample code</h3>

```java
UpArpuSDK.init(getApplicationContext(), appid, appKey);
```



<h2 id='4'>4. Native</h2>

<h3>4.1 Native ad introduction</h3>

1.Native ad integration，TopOn is provided to developers for rendering in the form of source material. (In addition to some ad platforms, the mediaview of large images and videos will be provided directly.);<br>
2.The obtained Native ads can only be displayed in the same place at the same time, otherwise the original ads that are initially displayed will not display or click normally;<br>
3.In the process of integrating Native advertising, some of the material acquired through the API is empty, so it is not necessary to display the material information at this time.


<h3>4.2 Native ad API</h3>

**UpArpuNative:** Native ad loaded class

| API | Parameter | Description |
| --- | --- | ---|
|UpArpuNative|Context context, String placementId, UpArpuNativeNetworkListener listener | Initialization method for Native ad, where placementId is obtained by creating an ad placement in the TopOn background. |
| makeAdRequest|-|Start Native Ad Request.|
| getNativeAd |-|Get the loaded advertisement **(It is recommended that non-null judgment be made after acquisition, because null may occur in some cases)**.|

**UpArpuNativeNetworkListener:** Event callback for Native ad

| Method | Parameter | Description |
| --- | --- |---|
| onNativeAdLoaded|-|The callback for successful ad loading.|
| onNativeAdLoadFail|(AdError error)| The callback for failed ad Loading, all error messages can be obtained via AdError.printStackTrace() |

**NativeAd:** The Ad object obtained by **getNativeAd** is described as follows:

| Method | Parameter | Description |
| --- | --- |---|
| setNativeEventListener|(UpArpuNativeEventListener listener) | Set the ad event listener, where UpArpuNativeEventListener is the interface class for the ad event |
| renderAdView|(UpArpuNativeAdView view, UpArpuNativeAdRenderer render)|Used for ad rendering, where view must use the UpArpuNativeAdView we provide, and UpArpuNativeAdRenderer needs to inherit the corresponding interface function. |
| prepare|(UpArpuNativeAdView view)| Used to configure ad click events, called after the renderAdView method (default all views can be clicked, there will be default placement and size of the ad logo). |
| prepare|(UpArpuNativeAdView view, FrameLayout.LayoutParams layoutParams) |Set the ad click event, which is called after the renderAdView method: <br/>layoutParams is used to configure the location and size of the ad logo(currently only Facebook, GDT is valid, can be empty).|
| prepare|(UpArpuNativeAdView view, List<View> clickViewList, FrameLayout.LayoutParams layoutParams) |Set  the ad click event, which is called after the renderAdView method: <br/>clickViewList is used to configure the clickable View <br/>layoutParams to configure the location and size of the ad tag (currently only Facebook, GDT is valid, can be empty).|
| clear|(UpArpuNativeAdView view)|Remove the binding of the ad to the view.|
|onResume|-|Called when the Activity's onResume (mainly for the video Ad of some advertisingplatforms).|
|onPause|-|Called when the Activity's onPause(mainly for the video Ad of some advertisingplatforms).|

**UpArpuNativeEventListener:** Ad event listener interface class

| Method | Parameter | Description |
| --- | --- |---|
| onAdImpressed|(UpArpuNativeAdView view, UpArpuAdInfo entity) | The callback for ad display, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. ||
| onAdClicked|(UpArpuNativeAdView view, UpArpuAdInfo entity)| The callback for the ad click. ||
| onAdVideoStart|(UpArpuNativeAdView view)| The callback when the ad video start to play (only some advertising platforms exist). ||
| onAdVideoEnd|(UpArpuNativeAdView view) | The callback when the ad video end to play(only some advertising platforms exist). ||
| onAdVideoProgress|(UpArpuNativeAdView view, int progress) | The callback when the progress of the ad video playing is changing(only some advertising platforms exist). ||

**UpArpuNativeAdRenderer:** Interface class for implementing ad rendering

| Method | Parameter | Description |
| --- | --- |---|
| createView|（Context contex, int networkType）|View for creating a custom Native ad layout.|
| renderAdView|(View view, CustomNativeAd customNativeAd)|A method for rendering an ad content, where customNativeAd is an object of the ad material and can provide material for rendering.|

**CustomNativeAd:** Native ad's material object (The following materials may appear to be null, because some advertising platforms do not necessarily have all the material information).

| Method | Parameter | Description |
| --- | --- |---|
| getIconImageUrl |-| Get the url of the ad icon. |
| getAdChoiceIconUrl|-| Get the icon url of the advertiser's logo |
| getAdIconView|- |Get the advertisement IconView **(Facebook only available and must be used)** |
| getAdMediaView|(ViewGroup view, int width)| Get the rendering container of the large image of the ad (only some of the advertising platforms will exist), possibly static images and videos. Parameter description: view: parent container of the ad, width: MediaView width configuration (currently only **Inmobi** requires these two parameters, the other can be null). |
| getMainImageUrl |-| Get the url of the large image. |
| getTitle |-| Get the ad title. |
| getDescriptionText|-| Get the ad description. |
| getCallToActionText |-| Get the text of the ad CTA button. |
| getAdFrom |-| Get the source of the ad (the Nend ad platform must render this information). |
| getAdLogo |-|Get the bitmap of the ad logo (currently only toutiao can get it).|
| getImageUrlList|-|Get the url list of the image.|
|getStarRating|-|Get the rating of the ad.|


<h3>4.3 Native ad sample code</h3>

```java
if (upArpuNativeAdView == null) {
	upArpuNativeAdView = new UpArpuNativeAdView(this);
}

UpArpuNative upArapuNatives= new UpArpuNative(this, placementId, new UpArpuNativeNetworkListener() {
                @Override
                public void onNativeAdLoaded() {
                    NativeAd nativeAd = upArapuNatives[mCurrentSelectIndex].getNativeAd();
                		if (nativeAd != null) {
                    	nativeAd.renderAdView(upArpuNativeAdView, new NativeAdRender());
                       nativeAd.prepare(upArpuNativeAdView);
                		} else {
                    

                		}
                }

                @Override
                public void onNativeAdLoadFail(AdError adError) {
                  

                }
            });
upArapuNatives.makeAdRequest();
```

```java

public class NativeAdRender implements UpArpuNativeAdRenderer<CustomNativeAd> {

    Context mContext;
    List<View> mClickView = new ArrayList<>();

    public UpArpuRender(Context context) {
        mContext = context;
    }

    View mDevelopView;

    int mNetworkType;

    @Override
    public View createView(Context context, int networkType) {
        if (mDevelopView == null) {
            mDevelopView = LayoutInflater.from(context).inflate(R.layout.native_ad_item, null);
        }
        mNetworkType = networkType;
        if(mDevelopView.getParent() != null){
            ((ViewGroup)mDevelopView.getParent()).removeView(mDevelopView);
        }
        return mDevelopView;
    }

    @Override
    public void renderAdView(View view, CustomNativeAd ad) {
        TextView titleView = (TextView) view.findViewById(R.id.native_ad_title);
        TextView descView = (TextView) view.findViewById(R.id.native_ad_desc);
        TextView ctaView = (TextView) view.findViewById(R.id.native_ad_install_btn);
        TextView adFromView = (TextView) view.findViewById(R.id.native_ad_from);

        FrameLayout contentArea = (FrameLayout) view.findViewById(R.id.native_ad_content_image_area);
        View mediaView = ad.getAdMediaView(contentArea, contentArea.getWidth());
        View adiconView = ad.getAdIconView();

        FrameLayout iconArea = (FrameLayout) view.findViewById(R.id.native_ad_image);
        final SimpleDraweeView iconView = new SimpleDraweeView(mContext);
        iconView.setImageDrawable(null);
        if (adiconView == null) {
            iconArea.addView(iconView);
            iconView.setImageURI(ad.getIconImageUrl());
        } else {
            iconArea.addView(adiconView);
        }


        final SimpleDraweeView logoView = (SimpleDraweeView) view.findViewById(R.id.native_ad_logo);
        if (!TextUtils.isEmpty(ad.getAdChoiceIconUrl())) {
            logoView.setVisibility(View.VISIBLE);
            logoView.setImageURI(ad.getAdChoiceIconUrl());
        } else {
            logoView.setVisibility(View.GONE);
        }


        contentArea.removeAllViews();
        if (mediaView != null) {
            int height = contentArea.getWidth() == 0 ? ViewGroup.LayoutParams.WRAP_CONTENT : contentArea.getWidth() * 3 / 4;
            contentArea.addView(mediaView, new FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.MATCH_PARENT));

        } else {

            final SimpleDraweeView imageView = new SimpleDraweeView(mContext);

            imageView.setImageURI(ad.getMainImageUrl());
            ViewGroup.LayoutParams params = new FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.MATCH_PARENT);
            imageView.setLayoutParams(params);
            contentArea.addView(imageView, params);
        }

        titleView.setText(ad.getTitle());
        descView.setText(ad.getDescriptionText());
        ctaView.setText(ad.getCallToActionText());
        if (!TextUtils.isEmpty(ad.getAdFrom())) {
            adFromView.setText(ad.getAdFrom() != null ? ad.getAdFrom() : "");
            adFromView.setVisibility(View.VISIBLE);
        } else {
            adFromView.setVisibility(View.GONE);
        }

        mClickView.clear();

        mClickView.add(ctaView);

    }

    public List<View> getClickView() {
        return mClickView;
    }
}

```



<h2 id='5'>5. NativeBanner</h2>

<h3>5.1 NativeBanner ad introduction</h3>

1.NativeBanner's ad provides the API of the advertisement in the form of View. Currently, it only supports 640x150, 320x50, Auto, and Auto will adapt the width and height of the NativeBanner View;<br>
2.NativeBanner needs to use the ad placement of the Native ad to load the ad.

<h3>5.2 NativeBanner ad API</h3>

**UpArpuNativeBannerView:** The loaded class of the NativeBanner ad is also a View that displays the NativeBanner ad.

| Method          | Parameter                              | Description                                                  |
| --------------- | -------------------------------------- | ------------------------------------------------------------ |
| setUnitId       | (String placementId)                   | Set the ad placement id (you must set the ad placement id of the Native ad). |
| setAdListener   | (UpArpuNaitveBannerListener listener)  | Set the callback for NativeBanner ad , where UpArpuNaitveBannerListener is the interface class that needs to implement the ad event callback. |
| loadAd          | (Map<String, String> customRequestMap) | set to null, and Incoming parameters are no longer used.     |
| setBannerConfig | (UpArpuNativeBannerConfig config)      | Set the native configuration of NativeBanner, for example: font color and font size. |

**UpArpuNaitveBannerListener:** Event callback for NativeBanner ad

| Method            | Parameter             | Description                                                  |
| ----------------- | --------------------- | ------------------------------------------------------------ |
| onAdLoaded        | -                     | The callback for successful ad loading.                      |
| onAdError         | (String error)        | The callback for failed ad loading, all error messages can be obtained through AdError.printStackTrace(). |
| onAdClick         | (UpArpuAdInfo entity) | The callback of the ad click, where UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onAdShow          | (UpArpuAdInfo entity) | The callback of the ad display, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onAdClose         | -                     | The callback for closing ad (some callbacks are available on some platforms), where you can remove the view. |
| onAutoRefresh     | (UpArpuAdInfo entity) | The callback of the automatic refresh of the ad, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party nwtwork platform. |
| onAutoRefreshFail | (String error)        | The callback for automatic refresh failure.                  |

**UpArpuNativeBannerConfig:** Local settings of NativeBanner

| Setting item   | Type                   | Description                                                  |
| -------------- | ---------------------- | ------------------------------------------------------------ |
| titleColor     | int                    | Color of ad headline.                                        |
| descColor      | int                    | Color of ad description.                                     |
| ctaColor       | int                    | Color of  the text of CTA button.                            |
| ctaBgColor     | int                    | Color of  the background of CTA button.                      |
| backgroupResId | int                    | Resource id of the background picture, default no exist.     |
| isCloseBtnShow | boolean                | Whether to show the close button.                            |
| isCtaBtnShow   | boolean                | Whether to show the CTA button(Show as much as possible on overseas platform, otherwise the display may be invalid. |
| refreshTime    | long                   | Refresh time in milliseconds.                                |
| bannerSize     | UpArpuNaitveBannerSize | The enumeration of size for NativeBanner: <br/>**UpArpuNaitveBannerSize.BANNER\_SIZE\_AUTO**: Adaptive the width and height of NativeBanner View <br/>**UpArpuNaitveBannerSize.BANNER\_SIZE\_640x150**: NativeBanner 640x150 aspect ratio <br/>**UpArpuNaitveBannerSize.BANNER\_SIZE\_320x50**: NativeBanner 320x50 aspect ratio |

<h3>5.3 NativeBanner ad sample code</h3>

```java
UpArpuBannerView  mBannerView = new UpArpuBannerView(BannerAdActivity.this);
mBannerView.setUnitId(placementId);
mBannerView.loadAd();
frameLayout.addView(mBannerView, new FrameLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, CommonUtil.dip2px(getApplicationContext(), 50)));
mBannerView.setBannerAdListener(new UpArpuBannerListener() {
	@Override
	public void onBannerLoaded() { 
	}

	@Override
	public void onBannerFailed(AdError adError) {
	}

	@Override
	public void onBannerClicked(UpArpuAdInfo entity) {
	}

	@Override
	public void onBannerShow(UpArpuAdInfo entity) {
	}

	@Override
	public void onBannerClose() {
	}

	@Override
	public void onBannerAutoRefreshed(UpArpuAdInfo entity) {
	}

	@Override
	public void onBannerAutoRefreshFail(AdError adError) {
	}
});
```


<h2 id='6'>6. NativeSplash</h2>

<h3>6.1 NativeSplash ad introduction</h3>

1.NativeSplash ads are implemented by relying on the developer to create their own Activity. Only the container ViewGroup that needs to display the ad is passed to the NativeSplash API. NativeSplash will automatically render the ad to the ViewGroup;<br>
2.The display area of the NativeSplash ad is preferably **75% or more of the screen height** and **skips the number of seconds over 3 seconds**,otherwise the ad impression may be invalid;<br>
3.**NativeSplash** supports vertical and horizontal screens application.


<h3>6.2 NativeSplash ad API</h3>

**UpArpuNativeSplash:** NativeSplash ad loaded class

| Method             | Parameter          | Description      |
| ------------------ | ------------------  | ------------------  |
| UpArpuNativeSplash | (Activity activity, ViewGroup container, View skipView, String placementId UpArpuNativeSplashListener listener) | Initialization method for NativeSplash, the following is the parameter description: <br/>**activity**: display activity <br/>**container**: display ad container <br/>**skipView**:the view for skip (the event automatically bind In the SDK, developers can't bind click events)<br/>**placementId**: ad placement id <br/>**listener**: ad event listener |
| UpArpuNativeSplash | (Activity activity, ViewGroup container, View skipView, String placementId, long requestTimeOut, long fetchDelay, UpArpuNativeSplashListener listener) | Initialization method for NativeSplash, the following is the parameter description: <br/>**activity**: display activity <br/>**container**: display ad container <br/>**skipView**: the view for skip(the event automatically bind In the SDK, developers can't bind click events)<br/>**placementId**: ad placement id <br/>**requestTimeOut**: request timeout time<br/>**fetchDelay**: ad display total countdown time <br/>**listener**: ad event listener |


**UpArpuNativeSplashListener:** Event callback for NativeSplash ad

| Method       | Parameter                   | Description                         |
| ------------ | --------------------------- | ------------------------------------------------------------ |
| onAdLoaded   | -                           | The callback for successful ad loading.                      |
| onNoAdError  | (AdError error)             | The callback for failed ad loading, all error messages can be obtained through AdError.printStackTrace(). |
| onAdShow     | (UpArpuAdInfo entity)       | The callback of the ad display, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onAdClick    | (UpArpuAdInfo entity)       | The callback of the ad click, where UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onAdSkip     | -                           | The callback of the skip button clicked.                     |
| onAdTimeOver | -                           | The callback of the end of the ad countdown, where you can turn off the Activity of the NativeSplash ad. |
| onAdTick     | （long millisUtilFinished） | The callback of the ad countdown, used to refresh the countdown seconds, in milliseconds. |

<h3>6.3 NativeSplash ad sample code</h3>

```java
public *** extends Activity {
	public void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);

        setContentView(R.layout.splash_ad_show);

        FrameLayout container = findViewById(R.id.splash_ad_container);

        UpArpuNativeSplash splash = new UpArpuNativeSplash(this, container, null, placementId, new UpArpuNativeSplashListener() {
            @Override
            public void onAdLoaded() {
            }

            @Override
            public void onNoAdError(String msg) {
            }

            @Override
            public void onAdShow(UpArpuAdInfo entity) {
            }

            @Override
            public void onAdClick(UpArpuAdInfo entity) {
            }

            @Override
            public void onAdSkip() {
            }

            @Override
            public void onAdTimeOver() {
            }

            @Override
            public void onAdTick(long millisUtilFinished) {
            }
        });
	}
}
```



<h2 id='7'>7. RewardedVideo</h2>

<h3>7.1 RewardedVideo ad introduction</h3>
1.Reward delivery of the rewarded video is to issue a Boolean field in the event callback of the video advertisement to close, telling the developer whether the reward can be issued to the user.<br>

2.At present, the rewarded video does not support the S2S.


<h3>7.2 RewardedVideo ad API</h3>

**UpArpuRewardVideoAd:** Rewarded video ad loaded class

| Method | Parameter | Description |
| --- | --- |---|
|UpArpuRewardVideoAd|(Context context, String placementId)|Initialization method for Rewarded video ad.|
|load|-|Start ad loading.|
|setAdListener|(UpArpuRewardVideoListener listener)| Set the Rewarded video ad listener, where UpArpuRewardVideoListener is the interface class for the ad event callback that needs to be implemented. |
|isAdReady|-|Judge if there is an ad that can be displayed currently.|
|show|-|Show rewarded video's ad.|
|setUserData|(String userId, String customData)|Set user information, mainly used to issue rewards.|

**UpArpuRewardVideoListener:** Event callback for rewarded video ads:

| Method | Parameter | Description |
| --- | --- |--- |
| onRewardedVideoAdLoaded|-|The callback for successful ad loading.|
| onRewardedVideoAdFailed|(AdError error)| The callback for failed ad loading, all error messages can be obtained through AdError.printStackTrace(). |
| onRewardedVideoAdPlayStart|(UpArpuAdInfo entity)|The callback that the ad starts to play, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|
| onRewardedVideoAdPlayEnd|(UpArpuAdInfo entity)|The callback of the end of the ad, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|
| onRewardedVideoAdPlayFailed|(AdError errorCode, UpArpuAdInfo entity)|The callback of the ad playback failure can be obtained by AdError.printStackTrace(), where UpArpuAdInfo is the information object of the advertisement, mainly including the id information of the third-party aggregation platform.|
| onRewardedVideoAdClosed|(boolean isRewarded, UpArpuAdInfo entity)| The callback of closing ad, isRewarded indicateds whether to issue rewards. The UpArpuAdInfo is an information object of the ad, and mainly includes the id information of the third-party network platform. |
| onRewardedVideoAdPlayClicked|(UpArpuAdInfo entity)|The callback of the ad click, where UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|


<h3>7.3 RewardedVideo ad sample code</h3>


```java
UpArpuRewardVideoAd mRewardVideoAd = new UpArpuRewardVideoAd(this,placementId);
mRewardVideoAd.setAdListener(new UpArpuRewardVideoListener() {
            @Override
            public void onRewardedVideoAdLoaded() {
            }

            @Override
            public void onRewardedVideoAdFailed(AdError errorCode) {
            }

            @Override
            public void onRewardedVideoAdPlayStart(UpArpuAdInfo entity) {
            }

            @Override
            public void onRewardedVideoAdPlayEnd(UpArpuAdInfo entity) {
            }

            @Override
            public void onRewardedVideoAdPlayFailed(AdError errorCode, UpArpuAdInfo entity) {
            }

            @Override
            public void onRewardedVideoAdClosed(boolean isRewarded, UpArpuAdInfo entity) {
            }

            @Override
            public void onRewardedVideoAdPlayClicked(UpArpuAdInfo entity) {
            }
        });

 if(mRewardVideoAd.isAdReady()){
 	mRewardVideoAd.show();
 } else {
 	mRewardVideoAd.load();
 }
```



<h2 id='8'>8. Interstitial</h2>

<h3>8.1 Interstitial ad introduction</h3>
Interstitial ad including the image interstitial ads and video interstitial ads of other third-party advertising platforms. both types of ads can use the api of Interstitial to load and play the ads.


<h3>8.2 Interstitial ad API</h3>

**UpArpuInterstitial:** Interstitial ad loaded class

| Method | Parameter | Description |
| --- | --- |---|
|UpArpuInterstitial|(Context context, String placementId)|Initialization method for Interstitial ad.|
|load|-|Start ad loading.|
|setAdListener|(UpArpuInterstitialListener listener)| Set the Interstitial ad listener, where UpArpuInterstitialListener is the interface class that needs to implement the ad event callback. |
|isAdReady|-|Judge if there is an ad that can be displayed current ly.|
|show|-|Show Interstitial ad.|

**UpArpuInterstitialListener:** Event callback for Interstitial ad

| Method | Parameter | Description |
| --- | --- |--- |
| onInterstitialAdLoaded|-|The callback for successful ad loading.|
| onInterstitialAdLoadFail|(AdError error)| The callback for failed ad loading, all error messages can be obtained through AdError.printStackTrace(). |
| onInterstitialAdClicked|(UpArpuAdInfo entity)|The callback that the ad click, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|
| onInterstitialAdShow|(UpArpuAdInfo entity)| The callback of the ad display, wherein UpArpuAdInfo is the information object of the advertisement, mainly including the id information of the third-party network platform. |
| onInterstitialAdClose|(UpArpuAdInfo entity)| The callback of closing ad, where UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onInterstitialAdVideoStart|-|The callback that the video ad starts to play, where UpArpuAdInfo is the information object of the advertisement, mainly including the id information of the third-party network platform.|
| onInterstitialAdVideoEnd|-| The callback for the end of video ad.                        |
| onInterstitialAdVideoError|-|The callback for interstitial video ad play failure, all error messages can be obtained through AdError.printStackTrace().|


<h3>8.3 Interstitial ad sample code</h3>


```java
UpArpuInterstitial mInterstitialAd = new UpArpuInterstitial(this, placementId);
mInterstitialAd.setAdListener(new UpArpuInterstitialListener() {
            @Override
            public void onInterstitialAdLoaded() {               
            }
            @Override
            public void onInterstitialAdLoadFail(AdError adError) {                
            }
            @Override
            public void onInterstitialAdClicked(UpArpuAdInfo entity) {                
            }
            @Override
            public void onInterstitialAdShow(UpArpuAdInfo entity) {                
            }
            @Override
            public void onInterstitialAdClose(UpArpuAdInfo entity) {           
            }
            @Override
            public void onInterstitialAdVideoStart() {         
            }
            @Override
            public void onInterstitialAdVideoEnd() {            
            }
            @Override
            public void onInterstitialAdVideoError(AdError adError) {     
            }
});

 if(mInterstitialAd.isAdReady()){
 	mInterstitialAd.show();
 } else {
 	mInterstitialAd.load();
 }
```



<h2 id=9'>9. Banner</h2>

<h3>9.1 Banner ad introduction</h3>
1.Banner's advertisement provides the API of the advertisement in the form of View. View can customize the width and height, but some ad platforms may not be able to adapt to the custom width and height, so the aspect ratio should be configured with the TopOn background before setting the width and height. Consistent, and need to adjust according to the actual effect after test;<br>
2.Banner provides automatic refresh function, the default is 20 seconds,  in TopOn background you can be set interval time, or set no refresh (Banner's View will be automatically refreshed after it has been added to Windows and visible).<br>


<h3>9.2 Banner ad API</h3>

**UpArpuBannerView:** The loaded class of the Banner ad is also a View that displays the Banner ad.

| Method | Param | Description |
| --- | --- |---|
| setUnitId|(String placementId)|Set the ad placement id (must be set).|
| setBannerAdListener|(UpArpuBannerListener listener)| Set the callback of the Banner ad, where UpArpuBannerListener is the interface class that needs to implement the callback of the ad event. |
| loadAd|-|Start ad loading.|

**UpArpuBannerListener:** Event callback for Banner ad

| Method | Parameter | Description |
| --- | --- |--- |
| onBannerLoaded|-|The callback for successful ad loading|
| onBannerFailed|(AdError error)| The callback for failed ad loading, all error messages can be obtained through AdError.printStackTrace(). |
| onBannerClicked|-|The callback for the ad click.|
| onBannerShow|(UpArpuAdInfo entity)| The callback of the ad display, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onBannerClose|-| The callback for closing ad (some callbacks are available on some platforms), where you can remove the view. |
| onBannerAutoRefreshed|(UpArpuAdInfo entity)|The callback of the automatic refresh of the ad, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|
| onBannerAutoRefreshFail(AdError adError)|(AdError error)|The callback that fails to automatically refresh the ad. All error messages can be obtained through AdError.printStackTrace().|


<h3>9.3 Banner ad sample code</h3>

```java
UpArpuBannerView  mBannerView = new UpArpuBannerView(BannerAdActivity.this);
mBannerView.setUnitId(unitIds[mCurrentSelectIndex]);
mBannerView.loadAd();
frameLayout.addView(mBannerView, new FrameLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, CommonUtil.dip2px(getApplicationContext(), 300)));
mBannerView.setBannerAdListener(new UpArpuBannerListener() {
	@Override
	public void onBannerLoaded() { 
	}

	@Override
	public void onBannerFailed(AdError adError) {
	}

	@Override
	public void onBannerClicked(UpArpuAdInfo entity) {
	}

	@Override
	public void onBannerShow(UpArpuAdInfo entity) {
	}

	@Override
	public void onBannerClose() {
	}

	@Override
	public void onBannerAutoRefreshed(UpArpuAdInfo entity) {
	}

	@Override
	public void onBannerAutoRefreshFail(AdError adError) {
	}
});
```



<h2 id='10'>10. Splash</h2>

<h3>10.1 Splash ad introduction</h3>

1.Splash ads are implemented by relying on the developer to create their own Activity. Only the container ViewGroup that needs to display the ad is passed to the Splash API, and Splash will automatically render the ad to the ViewGroup;<br>
2.The display area of the Splash ad is preferably **75% or more of the screen height** and **skips the number of seconds over 3 seconds**, otherwise the ad impression may be invalid;<br>
3.Splash **does not support horizontal screen** applications.


<h3>10.2 Splash ad API</h3>

**UpArpuSplashAd:** Splash ad loaded class

| Method | Parameter | Description |
| --- | --- |---|
|UpArpuSplashAd|(Activity activity, ViewGroup container, View skipView, String placementId, UpArpuSplashAdListener listener, long fetchDelay)|Initialization method for Splash ad, the following is the parameter description: <br/> **activity**: display activity <br/> **container**: display ad container <br/> **skipView**: the view for skip(the event automatically bind In the SDK, developers can't bind click events)<br/> **placementId**: ad placement id <br/> **listener**: ad event listener<br/> **fetchDelay**: ad show total countdown time|
|UpArpuSplashAd|(Activity activity, ViewGroup container, View skipView, String placementId, UpArpuSplashAdListener listener)|Initialization method for Splash ad(the default display time is 3 seconds), the following is the parameter description: <br/>**activity**: display activity <br/>**container**: display ad container <br/>**skipView**: the view for skip(the event is automatically bind in the SDK, the developer can't bind click event)<br/>**placementId**: the ad placement id <br/>**listener**: the ad event listener<br/>|

**UpArpuSplashAdListener:**  Event callback for Splash ad

| Method | Parameter | Description |
| --- | --- |--- |
| onAdLoaded|-|The callback for successful ad loading.|
| onNoAdError|(AdError error)| The callback for failed ad loading, all error messages can be obtained through AdError.printStackTrace(). |
| onAdShow|(UpArpuAdInfo entity)|The callback of the ad display, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|
| onAdClick|(UpArpuAdInfo entity)| The callback of the ad click, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform. |
| onAdDismiss|(UpArpuAdInfo entity)|The callback of closing ad, wherein UpArpuAdInfo is the information object of the ad, mainly including the id information of the third-party network platform.|
|onAdTick|(long millisUtilFinished)|The countdown callback for the ad, used to refresh the countdown seconds.|


<h3>10.3 Splash ad sample code</h3>


```java
public *** extends Activity {
	public void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);

        setContentView(R.layout.splash_ad_show);

        FrameLayout container = findViewById(R.id.splash_ad_container);
        TextView skipView = findViewById(R.id.splash_ad_skip);

        UpArpuSplashAd splashAd = new UpArpuSplashAd(this, container, skipView, placementId, new UpArpuSplashAdListener(){
		    @Override
		    public void onAdLoaded() {
		    }

		    @Override
		    public void onNoAdError(AdError adError) {
		        
		    }
		
		    @Override
		    public void onAdShow(UpArpuAdInfo entity) {
		    }
		
		    @Override
		    public void onAdClick(UpArpuAdInfo entity) {
		        
		    }
		
		    @Override
		    public void onAdDismiss(UpArpuAdInfo entity) {
		        finish();
		    }
		
		    @Override
		    public void onAdTick(long millisUtilFinished) {
		        skipView.setText(String.valueOf(millisUtilFinished / 1000) + "| SKIP");
		    }
		        
        }, 5000L);	
		
	}
}
```

<h2 id='11'>11. GDPR</h2>
Beginning on May 25, 2018, the Eurapean Union's General Data Protection Regulation(GDPR) will go into effect. To protect the interests and privacy of our developers and your users, we have updated our <a href="https://www.toponad.com/privacy-policy" target =" _blank">"《TopOn privacy Policy》"</a>. At the same time, we have added privacy permission settings in the SDK. Please check the following configuration and complete the SDK integration.

1.The method of setting the TopOn GDPR level is as follows: **(after setting, SDK will set the level of the third-party advertising platform according to the level you set)**:

```java
int level= { //Level has the following options
  UpArpuSDK.PERSONALIZED //Device data is allowed to be reported
  UpArpuSDK.NONPERSONALIZED //Device data is not allowed to be reported
  UpArpuSDK.FORBIDDEN //Do not do any data reporting (all ad requests stop)
}
UpArpuSDK.setGDPRUploadDataLevel(context, level);
```

2.The authorized page provided by TopOn **(the authorization page will set the level according to the user's choice)**:

```java
UpArpuSDK.showGdprAuth(activity);
```

3.You can also set the GDPR level of the third-party SDK separately through TopOn:

```java 
Map localMap = new HashMap<>();
//Admob configuration
// true:Agree to report personal information， false:Disagree with reporting personal information
localMap.put(AdmobUpArpuConst.LOCATION_MAP_KEY_GDPR, true);   
UpArpuSDK.addNetworkGDPRInfo(this, AdmobUpArpuConst.NETWORK_FIRM_ID, localMap);
    
//Inmobconfiguration
//1:In the EU，0:Not in the EU
localMap.put(InmobiUpArpuConst.LOCATION_MAP_KEY_GDPR_SCOPE, "1");//1 | 0
//true:Agree to report personal information，false:Disagree with reporting personal information
localMap.put(InmobiUpArpuConst.LOCATION_MAP_KEY_GDPR, true);//true | false
UpArpuSDK.addNetworkGDPRInfo(this, InmobiUpArpuConst.NETWORK_FIRM_ID, localMap);

//Applovin configuration
//true:Agree to report personal information，false:Disagree with reporting personal information
localMap.put(ApplovinUpArpuConst.LOCATION_MAP_KEY_GDPR, true);
UpArpuSDK.addNetworkGDPRInfo(this, ApplovinUpArpuConst.NETWORK_FIRM_ID, localMap);

//Mintegral configuration
//true:Agree to report personal information，false:Disagree with reporting personal information
localMap.put(MintegralUpArpuConst.LOCATION_MAP_KEY_GDPR, MIntegralConstans.IS_SWITCH_ON);
UpArpuSDK.addNetworkGDPRInfo(this, MintegralUpArpuConst.NETWORK_FIRM_ID, localMap);

//Mopub configuration
//true:Agree to report personal information，false:Disagree with reporting personal information
localMap.put(MopubUpArpuConst.LOCATION_MAP_KEY_GDPR, true);
UpArpuSDK.addNetworkGDPRInfo(this, MopubUpArpuConst.NETWORK_FIRM_ID, localMap);

//Chartboost configuration
//false:Agree to report personal information，true:Disagree with reporting personal information
localMap.put(ChartboostUpArpuConst.LOCATION_MAP_KEY_RESTRICTDATACONTROL, true);
UpArpuSDK.addNetworkGDPRInfo(this, ChartboostUpArpuConst.NETWORK_FIRM_ID, localMap);

//Ironsource configuration
//true:Agree to report personal information，false:Disagree with reporting personal information
localMap.put(IronsourceUparpuConst.LOCATION_MAP_KEY_CONSENT, true);
UpArpuSDK.addNetworkGDPRInfo(this, IronsourceUparpuConst.NETWORK_FIRM_ID, localMap);

//UnityAds configuration
//true:Agree to report personal information，false:Disagree with reporting personal information
localMap.put(UnityAdsUpArpuConst.LOCATION_MAP_KEY_GDPR_CONSENT, true);
UpArpuSDK.addNetworkGDPRInfo(this, UnityAdsUpArpuConst.NETWORK_FIRM_ID, localMap);

//Adcolony configuration
//1:Agree to report personal information，0:Disagree with reporting personal information
localMap.put(AdColonyUpArpuConst.LOCATION_MAP_KEY_GDPRCONTENT, "0"); 
//true:In the EU，false:Not in the EU       
localMap.put(AdColonyUpArpuConst.LOCATION_MAP_KEY_GDPRREQUEST, true);
UpArpuSDK.addNetworkGDPRInfo(this, AdColonyUpArpuConst.NETWORK_FIRM_ID, localMap);
```



<h2 id='12'>12. HeadBidding</h2>
In-app header bidding is an advanced programmatic ad auction technology that allows all demanders to bid for the same impression at the same time, and the highest bidder gets the impression, which ensures that the publisher’s per impression can be more profitable. The TopOn platform currently supports in-app header bidding for Mintegral and Facebook.

The SDK versions of Mintegral and Facebook that support header bidding are as follows:


| Network   | Platform | AD Format                                  | Network SDK Version | Extra SDK            |
| --------- | -------- | ------------------------------------------ | ------------------- | -------------------- |
| Facebook  | Android  | Native, Rewarded Video, Interstitial       | >= 4.99.x           | BiddingKit.aar       |
| Mintegral | Android  | Native, Rewarded Video, Interstitial Video | >= 9.12.4           | mintegral_mtgbid.aar |

Note: Extra's SDK has been in the directory of Network SDK, you can import the SDK if you need.