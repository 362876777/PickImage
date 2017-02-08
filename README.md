<!-- Library Logo -->
<img src="https://github.com/jrvansuita/PickImage/blob/master/app/src/main/res/mipmap-xxxhdpi/ic_launcher.png?raw=true" align="left" hspace="1" vspace="1">

<!-- Buy me a cup of coffe -->
<a href='https://ko-fi.com/A406JCM' style='margin:13px;' target='_blank' align="right"><img align="right" height='36' src='https://az743702.vo.msecnd.net/cdn/kofi4.png?v=f' alt='Buy Me a Coffee at ko-fi.com' /></a>
<a href='https://play.google.com/store/apps/details?id=com.vansuita.pickimage.sample&pcampaignid=MKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1' target='_blank' align="right"><img align="right" height='36' src='https://s20.postimg.org/muzx3w4jh/google_play_badge.png' alt='Get it on Google Play' /></a>
# PickImage


This is an [**Android**](https://developer.android.com) project. It shows a [DialogFragment](https://developer.android.com/reference/android/app/DialogFragment.html) with Camera or Gallery options. The user can choose from which provider wants to pick an image.

</br> 
</br> 
</br> 

<!-- JitPack integration -->
[![](https://jitpack.io/v/jrvansuita/PickImage.svg)](https://jitpack.io/#jrvansuita/PickImage)<!-- Android Arsenal -->
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-PickImage-green.svg?)](https://android-arsenal.com/details/1/4614)<!-- License -->
<a target="_blank" href="/LICENSE.txt"><img src="http://img.shields.io/:License-MIT-yellow.svg" alt="MIT License" /></a><!-- Minimun Android Api -->
<a target="_blank" href="https://developer.android.com/reference/android/os/Build.VERSION_CODES.html#GINGERBREAD"><img src="https://img.shields.io/badge/API-9%2B-blue.svg?style=flat" alt="API" /></a><!-- Methods Count -->
<a target="_blank" href="http://www.methodscount.com/?lib=com.github.jrvansuita%3APickImage%3Av2.0.2"><img src="https://img.shields.io/badge/Methods-428-e91e63.svg" /></a> <!-- Apptize.io -->[![Appetize.io](https://img.shields.io/badge/Apptize.io-Run%20Now-brightgreen.svg?)](https://appetize.io/embed/57ztxax2v7egygwz0nhybmxnf0?device=nexus7&scale=50&autoplay=true&orientation=portrait&deviceColor=black) <!-- Hits Count -->[![ghit.me](https://ghit.me/badge.svg?repo=jrvansuita/PickImage)](https://ghit.me/repo/jrvansuita/PickImage)<!--Open Source --> [![Open Source Love](https://badges.frapsoft.com/os/v2/open-source.svg?v=103)](https://github.com/jrvansuita)
<!--<a href="https://github.com/jrvansuita/PickImage/releases/latest">
  <img alt="Latest release" src="https://img.shields.io/github/release/jrvansuita/PickImage.svg" />
</a>-->



# Screenshots

#### Default icons.
<img src="images/dialogs/light_vertical_left_simple.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_top_simple.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_left_simple.png" height='auto' width='280'/>

#### Colored icons.
<img src="images/dialogs/light_vertical_left_colored.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_top_colored.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_left_colored.png" height='auto' width='280'/>

#### Custom dialog.
<img src="images/dialogs/dark_vertical_left.png" height='auto' width='280'/>
<img src="images/dialogs/dark_horizontal_top.png" height='auto' width='280'/>
<img src="images/dialogs/dark_horizontal_left.png" height='auto' width='280'/>

#### System dialog.
<img src="images/dialogs/all_system_dialog.png" height='auto' width='280'/>
<img src="images/dialogs/camera_system_dialog.png" height='auto' width='280'/>
<img src="images/dialogs/images_system_dialog.png" height='auto' width='280'/>

# Setup

#### Step #1. Add the JitPack repository to your build file:

    allprojects {
		repositories {
			...
			maven { url "https://jitpack.io" }
		}
	}

#### Step #2. Add the dependency ([See latest release](https://jitpack.io/#jrvansuita/PickImage)).

    dependencies {
           compile 'com.github.jrvansuita:PickImage:+'
	}

# Implementation

#### Step #1. Overriding the library file provider authority to avoid installation conflicts.
The use of this library can cause [INSTALL_FAILED_CONFLICTING_PROVIDER](https://developer.android.com/guide/topics/manifest/provider-element.html#auth) if you skip this step. Update your AndroidManifest.xml with this exact provider declaration below.

    <manifest ...>
        ... 
        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="${applicationId}.com.vansuita.pickimage.provider"
            tools:replace="android:authorities">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/provider_paths" />
        </provider>
    </manifest> 
    
#### Step #2 - Showing the dialog.

    PickImageDialog.build(new PickSetup()).show(this);
    
#### Step #3 - Applying the listeners.

##### Method #3.1 - Make your AppCompatActivity implements IPickResult.

    @Override
    public void onPickResult(PickResult r) {
        if (r.getError() == null) {
            //If you want the Uri.
            //Mandatory to refresh image from Uri.
            //getImageView().setImageURI(null);

            //Setting the real returned image.
            //getImageView().setImageURI(r.getUri());

            //If you want the Bitmap.
            getImageView().setImageBitmap(r.getBitmap());

            //Image path
            //r.getPath();
        } else {
            //Handle possible errors
            //TODO: do what you have to do with r.getError();
            Toast.makeText(this, r.getError().getMessage(), Toast.LENGTH_LONG).show();
        }
    }
           
##### Method #3.2 - Set the listener using the public method (Good for Fragments).

    PickImageDialog.build(new PickSetup())
                   .setOnPickResult(new IPickResult() {
                      @Override
                      public void onPickResult(PickResult r) {
                         //TODO: do what you have to...
                      }
                }).show(getSupportFragmentManager());


#### Step #4 - Customize you Dialog using PickSetup.
    PickSetup setup = new PickSetup()
                .setTitle(yourText)
                .setTitleColor(yourColor)
                .setBackgroundColor(yourColor)
                .setProgressText(yourText)
                .setProgressTextColor(yourColor)
                .setCancelText(yourText)
                .setCancelTextColor(yourColor)
                .setButtonTextColor(yourColor)
                .setDimAmount(yourFloat)
                .setFlip(true)
                .setImageSize(500)
                .setPickTypes(EPickTypes.GALLERY, EPickTypes.CAMERA)
                .setCameraButtonText(yourText)
                .setGalleryButtonText(yourText)
                .setIconGravity(Gravity.LEFT)
                .setButtonOrientation(LinearLayoutCompat.VERTICAL)
                .setSystemDialog(false)
                .setGalleryIcon(yourIcon)
                .setCameraIcon(yourIcon);
    /*... and more to come. */
       
# Additionals

#### Own click implementations.
###### _If you want to write your own button click event, your class have to implements [IPickClick](library/src/main/java/com/vansuita/pickimage/listeners/IPickClick.java) like in the example below. You may want to take a look at the sample app._
 
     @Override
     public void onGalleryClick() {
         //TODO: Your onw implementation
     }
 
     @Override
     public void onCameraClick() {
         //TODO: Your onw implementation
     }
     
#### For dismissing the dialog.
     PickImageDialog dialog = PickImageDialog.on(...);
     dialog.dismiss();
     
# Sample app code.
 You can take a look at the sample app [located on this project](/app/).
 
# 

<a href="https://plus.google.com/+JuniorVansuita" target="_blank">
  <img src="https://s20.postimg.org/59xees8vt/google_plus.png" alt="Google+" witdh="44" height="44" hspace="10">
</a>
<a href="https://www.linkedin.com/in/arleu-cezar-vansuita-júnior-83769271" target="_blank">
  <img src="https://s20.postimg.org/vxoeax4ah/linkedin.png" alt="LinkedIn" witdh="44" height="44" hspace="10">
</a>
<a href="https://www.instagram.com/jnrvans/" target="_blank">
  <img src="https://s20.postimg.org/lyyuap5h5/instagram.png" alt="Instagram" witdh="44" height="44" hspace="10">
</a>
<a href="https://github.com/jrvansuita" target="_blank">
  <img src="https://s20.postimg.org/jf37glhx5/github.png" alt="Github" witdh="44" height="44" hspace="10">
</a>
<a href="https://play.google.com/store/apps/dev?id=8002078663318221363" target="_blank">
  <img src="https://s20.postimg.org/5iuz4plo9/android.png" alt="Google Play Store" witdh="44" height="44" hspace="10">
</a>
<a href="mailto:vansuita.jr@gmail.com" target="_blank" >
  <img src="https://s20.postimg.org/slli3vn5l/email.png" alt="E-mail" witdh="44" height="44" hspace="10">
</a>


