# UTPSDKGallery (Pro)

<p align="center" >
  <img src="https://github.com/hankshan/UTPSDKGallery/blob/master/Images/Preview/Preview_1.png" alt="UTPSDKGallery (Pro)" title="UTPSDKGallery (Pro)">
</p>

[Download UTPSDKGallery (Pro)](https://www.assetstore.unity3d.com/#!/content/90910)


## Requirements

| UTPSDKGallery (Pro) Version | Minimum iOS Target  | Minimum OS X Target  | Minimum Win Target  | Minimum Android Target  |                                   Notes                                   |
|:--------------------:|:---------------------------:|:----------------------------:|:----------------------------:|:----------------------------:|:-------------------------------------------------------------------------:|
| 1.0 | iOS 8.0 | OS X 10.0 | Windows 7 | Android 4.2 "Jelly Bean"- API Level 17 |  |

The above is only the minimum version recommended. IOS minimum is 8, other platforms can support lower, but do not recommend the use of lower version of the platform.

#Description

UTPSDKGallery (Pro) is a rather powerful cross platform save game, screenshots, and Texture2D plugin. It handles Texture2D directly and saves it. You can save some screenshots of the screen to the album, save all screenshots of the screen to the album, save the image of the component (Texture2D) to the album. Support JPG and PNG save formats.

## How To Get Started
```C#
  public enum TextureEncodeToFormat
  {
    EncodeToFormat_JPG,
    EncodeToFormat_PNG
  }
    
  public void CaptureSaveToAlbum(Rect rect, TextureEncodeToFormat imageFormat = TextureEncodeToFormat.EncodeToFormat_PNG, string imageName = null, UTPSDKGalleryCom.OnUTPSDKGallerySaveToAlbum handler = null);
  
  public void CaptureSaveToAlbum(TextureEncodeToFormat imageFormat = TextureEncodeToFormat.EncodeToFormat_PNG, string imageName = null, UTPSDKGalleryCom.OnUTPSDKGallerySaveToAlbum handler = null);
  
  public void InCoroutineSaveToAlbum(Texture2D texture2D, TextureEncodeToFormat imageFormat = TextureEncodeToFormat.EncodeToFormat_PNG, string imageName = null, UTPSDKGalleryCom.OnUTPSDKGallerySaveToAlbum handler = null);

```
Note: iOS needs to add album access rights to info.plist [NSPhotoLibraryUsageDescription]

## =>Mac
Save Path : /Users/[ComputerName]/Pictures
<p align="center" >
  <img src="https://github.com/hankshan/UTPSDKGallery/blob/master/Images/Mac/QQ20170518-214231.png" alt="UTPSDKGallery (Pro)" title="UTPSDKGallery (Pro)">
</p>

## =>Windows
Save Path : \Users\[ComputerName]\Pictures
<p align="center" >
  <img src="https://github.com/hankshan/UTPSDKGallery/blob/master/Images/Windows/QQ20170519151125.png" alt="UTPSDKGallery (Pro)" title="UTPSDKGallery (Pro)">
</p>

## =>Android
Save Path : Album
<p align="center" >
  <img src="https://github.com/hankshan/UTPSDKGallery/blob/master/Images/Android/Album1.png" alt="UTPSDKGallery (Pro)" title="UTPSDKGallery (Pro)">
</p>

## =>IOS
Save Path : Album
<p align="center" >
  <img src="https://github.com/hankshan/UTPSDKGallery/blob/master/Images/IOS/IMG_1380.PNG" alt="" title="UTPSDKGallery (Pro)">
  <img src="https://github.com/hankshan/UTPSDKGallery/blob/master/Images/IOS/QQ20170518-222423.png" alt="UTPSDKGallery (Pro)" title="UTPSDKGallery (Pro)">
</p>

#Sample

```C#
 #region Authorization

    public void RequestAuthorization()
    {
        bool isAuth = false;
        if (!utpsdkGallery.IsAuthorization())
        {
            isAuth = utpsdkGallery.RequestAuthorization();
        }
        else
        {
            isAuth = true;
        }

        Debug.Log("=>isAuth=" + isAuth);
    }

    #endregion


    #region Album - Save Full Screen

    public void OnClickSaveFullScreen()
    {
        utpsdkGallery.CaptureSaveToAlbum(UTPSDKGallery.UTPSDKGalleryCom.TextureEncodeToFormat.EncodeToFormat_PNG, null, this.OnHandlerSaveFullScreen);

    }

    public void OnHandlerSaveFullScreen(int resultCode, string message)
    {
        if (resultCode == 1)
        {
            this.uiTextConsole.text = "SaveFullScreen:<color=\"green\">" + "resultCode:" + resultCode + " message:" + message + "</color>";
        }
        else
        {
            this.uiTextConsole.text = "SaveFullScreen:<color=\"red\">" + "resultCode:" + resultCode + " message:" + message + "</color>";
        }
    }

    #endregion


    #region Album - Save Part Screen

    public void OnClickSavePartScreen()
    {
        Rect rectCapture = new Rect(0, 0, Screen.width, Screen.height);
        try
        {
            int x = Convert.ToInt32(this.uiInputFieldRectX.text);
            int y = Convert.ToInt32(this.uiInputFieldRectX.text);
            int width = Convert.ToInt32(this.uiInputFieldRectWidth.text);
            int height = Convert.ToInt32(this.uiInputFieldRectHeight.text);
            rectCapture.Set(x, y, width, height);
        }
        catch (Exception ex)
        {
            Debug.LogError(ex.Message);
        }

        utpsdkGallery.CaptureSaveToAlbum(rectCapture, UTPSDKGallery.UTPSDKGalleryCom.TextureEncodeToFormat.EncodeToFormat_PNG, null, this.OnHandlerSavePartScreen);
    }

    public void OnHandlerSavePartScreen(int resultCode, string message)
    {
        if (resultCode == 1)
        {
            this.uiTextConsole.text = "SavePartScreen:<color=\"green\">" + "resultCode:" + resultCode + " message:" + message + "</color>";
        }
        else
        {
            this.uiTextConsole.text = "SavePartScreen:<color=\"red\">" + "resultCode:" + resultCode + " message:" + message + "</color>";
        }
    }

    #endregion


    #region Album - Save Part Screen

    public void OnClickSaveTexture()
    {
        StopCoroutine("CoroutineSaveTexture");
        StartCoroutine(this.CoroutineSaveTexture(uiImageSample.sprite, UTPSDKGallery.UTPSDKGalleryCom.TextureEncodeToFormat.EncodeToFormat_PNG, null));
    }

    private IEnumerator CoroutineSaveTexture(Sprite sprite, UTPSDKGallery.UTPSDKGalleryCom.TextureEncodeToFormat imageFormat, string imageName)
    {
        yield return new WaitForEndOfFrame();

        Texture2D texture2D = UTPSDKGalleryTexture2D.CaptureTexture2D(sprite);

        this.utpsdkGallery.InCoroutineSaveToAlbum(texture2D, imageFormat, imageName, this.OnHandlerSaveTexture);

        if (texture2D != null)
        {
            GameObject.DestroyObject(texture2D);
        }
    }

    public void OnHandlerSaveTexture(int resultCode, string message)
    {
        if (resultCode == 1)
        {
            this.uiTextConsole.text = "SaveTexture:<color=\"green\">" + "resultCode:" + resultCode + " message:" + message + "</color>";
        }
        else
        {
            this.uiTextConsole.text = "SaveTexture:<color=\"red\">" + "resultCode:" + resultCode + " message:" + message + "</color>";
        }
    }

    #endregion
```


