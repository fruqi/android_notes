# General Knowledge

* Android project contains one more more modules, such as:
  * Android devices modules (wearable, tv, glass) - this will generate an apk
  * Android library modules - this will not generate apk, Android library modules contains source codes, resources, manifests
  * Java library modules  - this contains only source code

* `dp` is density pixel - It is a way of android to measure the pixel based on the
  screen density. (e.g. 
* `sp` is scale pixel - It is almost the same as `dp`. The only difference is
  that it is used for text and it will resize if users change the font-size from
the system level
* `R` stands for Resource. It refers to the resource in the layout
* `View` in android includes all the components. Such as: layout and widgets
* `Drawable` is also part of the Resource (`R`)
* When creating a method that is being called by Android view, we need to add
  `View` type as the argument to the method declaration. 
  (Or else it will throw an error)
* In order to set the `ImageView` resource to nothing, use
  `setImageDrawable(null)`
* `VideoView` doesn't have `src` attribute, thus we have to set it by using the
  following method:
  ```
    VideoView videoView = (VideoView) findViewById(R.id.VideoView);
    videoView.setVideoPath("android.resource://" + getPackageName() 
                            + "/" + R.raw.demo_vid);
    videoView.start();
  ```
* In order to add a control to a video you have to create another widget called
  `MediaController`. See example below:
  ```
    MediaController mediaController = new MediaController(this);
    mediaController.setAnchorView(videoView); // set param to video view object
    
    videoView.setMediaController(mediaContrller); // set to media controller obj
  ```

* You can add an audio player by using `MediaPlayer` class
  ```
    MediaPlayer media = MediaPlayer.create(this, R.raw.audioSource);
    media.start();
  ```

* If you are going to access the Android system volume, use `AudioManager` class
  and instantiate it by doing the following:

  ```
  AudioManager aMgr = (AudioManager) getSystemService(Context.AUDIO_SERVICE);
  
  int maxVol = aMgr.getStreamMaxVolume(AudioManager.STREAM_MUSIC);
  ```

* `AppCompatActivity` is the subclass of `Activity` class that provides
  compatibility support for older version of Android

* Most devices use API Level 16 - 21

* In order to add a list to the `ListView` widget, we need to create an
  `ArrayList` object then wrap it with `ArrayAdapter`. 

* We can cast `Long` (or `Big`) to integer by doing the following
  ```
    Long longInt = 300;
    int integer = (int) longInt;

  ```

* If you require to get the name id of the element/widget please do the
  following:

  ```

* Adding "m" before the widget/element/component's name is Android convention
  E.g.
    ```
      private Button mTrueButton;
      private Button mFalseButton;

    ```

    int viewID = view.getId();
    String nameID = view.getResources().getResourceEntryName(viewID);

  ```

* In order to clear out the content of `ArrayList` you can use `clear` method
  ```
    List list = new ArrayList();
    list.clear();
  ```

* Use try/catch for a condition where we can't assure the result of a code block
  (such as: downloading content, querying database)

* You can't update view in `doInBackgroundThread` of `AsyncTask`, you should do
  that in `UI` thread instead (Such as: `onCreate`). However, `AsyncTask` has 
  a method called `onPostExecute` that allow you to interact with `UI` thread

* We can use `JSONObject` and `JSONArray` to transform `JSON` string into either
  an array or object, depending on which class you assigned the string to.

* Use `URLEncoder` to encode a string into query string parameter
  ```
    URLEncoder.encode("balik papan", "UTF-8");
    >> Result in: "balik%20papan"
  ```

* In order to use user's current location you should do the following:
  1. Implement `LocationListener` to the Activity

  2. Initialize `LocationManager` by doing the following
    ```
      LocationManager locMan;
      locMan = (LocationManager) getSystemService(LOCATION_SERVICE);
    ```

  3. Initialize `Provider` to be used by `LocationManager`
    ```
      String provider = locationManager.getBestProvider(new Criteria, false);
    ```

  4. Add location use permission to `AndroidManifest` file
  5. Now you can use locationManager.getLastKnownLocation() to get the location

* In order to have fullscreen background, do the following:
  1. Set the imageView layout widht and height to `fill_parent`
  2. Set `scaleType` property to `centerCrop`

# Activity

* Activity consists of 6 main lifecycles:
  1. onCreate
  2. onStart
  3. onResume
  4. onPause
  5. onStop
  6. onDestroy

* If we specify an activity launchMode as 'singleTop' in the manifest file, this activity will not be recreated if it already exists in the back task stacks;
  meaning onPause, onStop, onDestroy, and onCreate will not be called

  """
    For example, there are 2 activities on the app
    Activity A is the parent of activity B, and activity A is set as 'singleTop'.

    User clicks a button on 'activity A' and the app goes to 'activity B', if user click back button then activity A will not be recreated. 
    Meaning, activity A's onCreate method will not be called
  """

# FRAGMENT

* You can think of fragment as partial view 
* There are two ways of adding fragment to Android layout (Statically &
  Dynamically)
  1. Statistically
    * Create a layout for the fragment
    * In the parent layout, specify the fragment by the fragment name
      ```
        <fragment
            android:name="com.example.afaruqi.fragdemo.FooFragment"
            android:id="@+id/fooFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
      ```
    * Note: make sure to give id to the `fragment` tag

  2. Dynamically
    * Create a `FrameLayout` tag within the containing activity layout
    * Add id to the `FrameLayout` tag for Activity to refer
    * Inside of the Activity, create `FragmentTransaction` object by doing the
      following
      ```
        FragmentTransaction ft = getSupportFragmentManager().beginTranscation();

        // To add fragment to the activity
        ft.add(R.id.fragment_container, new FooFragment());

        // To begin transaction 
        ft.commit();
      ```

---------

* There are two ways to set custom icon for the home icon on the Actionbar/Toolbar
  (left-hand side) 

  1. Use `toolbar` object and call following method:
    ```
      toolbar.setNavigationIcon(R.drawable.ic_boobs)
    ```
  2. Use `actionbar` object and call following method:
    ```
      ActionBar actionBar = getActionBar();
      actionBar.setDisplayHomeAsUpEnabled(true);
    ```

----------

# LIFE CYCLE

* There is quite a number of Activities life cycles, such as (in order):
  1. onCreate
  2. onStart
  3. onResume
  4. onPause
  5. onStop
  6. onDestroy

* You should prepare for app death as early as onPause; though onStop seems to be the perfect checkpoint
* Each Activity has its own stash (object to store state) in `bundle` object
* You can use this `bundle` object to store some data
  > Use case, changing the orientation will not store the current index of 
    GeoQuiz app. Thus, storing current index in `bundle` object and call it 
    back later on will maintain the quiz index in whichever orientation

* Override `onSaveInstanceState` to store some data in `bundle` object. So that
  you can retrieve the data back in `onCreate`

* To add layout for *landscape* orientation, create new folder then select
  `landscape` from orientation option. It will create "layout-land" folder.
  If you don't see it, change the view structure from "Android" to "Project"

--------

* Anything can be converted to `byteArray` (byte[] byteArray)
* In order to upload a file to Parse server, you need to convert the image to
  byteArray then create `ParseFile` object with the byteArray as the parameter

* `tools:text` attribute is on `TextView` widget display text in preview mode. It
  helps us to give a clear vision how it looks like without polluting the real
  applications.

* Every activity in an application must be declared in the manifest so OS can
  access it

* When you want to start an activity from another (parent activity), you need to
  pass `Intent` object. 

* When an activity sends a directive to start another activity within an
  application, the request goes to the OS level and OS decides to ask
  `ActivityManager` to take care of the request

* When we create a new `Intent` object, we need to pass the `context` and the
  class of the intended activity. The context will give information of the
  package name in which the intended activity can be found.
  Thus, we need to specify newly created activity in `AndroidManifest` file.
  So that OS can find the activity and not throwing an exception.

---------------

# FRAGMENT

* Fragment's lifecycle is the responsible of hosting Activity

* While Activity's lifecycle is the responsible of OS

* OS doesn't have anything to do with Fragment's lifecycle

* It is a better idea to use `Fragment` from support-v4 instead of the native
  version of it

* Fragment's lifecycle is almost the same with Actvity's
  * Fragment has `onCreate` method like Activity but it doesn't set the view in
    this lifecycle
  * Instead, Fragment `onCreateView` method is the one who set the view
  
* In order for an Activity to inflate fragment to its layout, we need the help
  from `FragmentManager` (from support library)

* `FragmentManager` is responsible of two things:
  1. Keep track of back stack
  2. Keep track of Fragment list; That's why it has `findFragmentById` method

* Think of `FragmentManager` like `ActivityManager`. But instead of managed by
  OS (`ActivityManager`), FragmentManager is managed by Activity

* View can save state across configuration change as long as it has an `id`
  attribute

* I suppose this is about retained fragments. 
  Inside fragment you can call setRetainedInstance(true), this way your fragment will not be recreated during configuration changes. 
  Normally when you rotate your device, all fragments will be recreated. If you call setRetainedInstance(true) inside onCreate(), then your fragment instance will not be recreated.

* `RecyclerView` is a better option compared to `ListView`, because it provides
  us more robust way to create custom layout for every items in the list

* `RecyclerView` requires us to define our own `Adapter` and `ViewHolder`
  * `ViewHolder` is template for item on the list; it also the place to put the
    listener of each items
  
  * `Adapter` is the main guy behind every items' view creation and reuse

  * In order to add `Up` button in the toolbar, you need to specify the parent
    of that particular Activity in `AndroidManfiest.xml` file

    ```
      <activity
        ...
        ...
        android:parentActivityName=".MainActivity" />
      
    ```
* Theme

* AppCompat

* `getApplicationContext` lives longer than activity. In the case where we have
  singleton, using `getApplicationContex` is better since activity come and go.
  And garbage collector can collect the activity context since it's not being
  keep by singleton.

* `app` name space in the resource files (such as: menu, layout) is the way of
   Android to use attribute provided by support library so that legacy version
   can still get the same look & feel.

# MATERIAL DESIGN

* It is introduce since API 21 (Lollipop)
* There are two branches of material design theme that we can apply to our
  android applications:
  1. `@android:style/Theme.Material.Light` ~> Native support
  2. `Theme.AppCompat.Light.DarkActionBar` ~> Support Library *Recommended*
 

# Toolbar

* We can use `toolbar` as stand alone toolbar or actionbar

# Assets

  * Resource folder only allows you to ship flat structure. For example, you 
    can't add folder named *sounds* in `res` (resource) folder
  * We should use `assets` instead of putting source in `res` folder if we want 
    to have our own custom folder structures
  * `AssetsManager` is the object which enables you to access assets
  * `AssetsManager` can be accessed from any context
    ```
      AssetManager assetManager = Context.getAssets();
    ```

  * `SoundPool` can load a large set of sound and control the maximum number of
    sounds playing at one time

  * Try to 'Restart & Invalidate Cache' if there is issue with layout preview

  * action view is a view that may be included in the `toolbar` (such as:
    `SearchView`)

# Intent Service

  * 

# Code Structure
  
  * Put methods in a class based on the following:
    1. Constructor methods
    2. Static methods
    3. Override methods
    4. Instance methods

* Tying background thread to UI component (UI thread) is a bad idea, the reason when the UI thread changes (orinetation change for example) then the background thread will be terminated

* Android M and above have changed its permission model, check out this url for more details 
  > https://developer.android.com/training/permissions/index.html
  > https://developer.android.com/guide/topics/security/permissions.html
  > https://developer.android.com/reference/android/Manifest.permission.html

* To create the best quality Android app, check out: https://developer.android.com/distribute/essentials/quality/core.html
* Udacity webcast: https://docs.google.com/document/d/1WEdC10ldAL1FX4W0ssdiptQnlOMP2u6jyj1o09jdjJw/pub?embedded=true

* ArrayAdapter#add or ArrayAdapter#addAll methods call #notifyDataSetChanged by default, therefore we don't have to call notifyDataSetChanged explicitly

* It's a good practice to define keys for intent extras using your app's package name as a prefix


# Content Provider

  * Content Provider is like middleman between your app and data source

  * The benefits of using Content Provider are:
    1. We can easily change underlying datasource 
      > e.g. we can easily change from SQLite to Realm database without having to worry of using Realm DSL 
    
    2. Leverage functionality of Android Classes
      > e.g. SyncAdapter, Loaders, CursorAdapter

    3. Allow many apps to access, use, and modify a single data source __securely__

  * Content Provider implements functionality based upon URI passed to them

  * For ease of implementation, Content Provider typically ties each URI type internally to an integer constant

  * Provider is an Android component, thus we need to specify it in AndroidManifest.xml

  * In order for us to use ContentResolver on our own Content Provider, we need to add <provider ...> tag into AndroidManifest file

# Android Manifest 

# PackageManager

  * `PackageManager` knows about all the components installed on your Android device, including all of its activities
    This is achieved since PackageManager read information from AndroidManifest.xml file


# Service

  * Service is an Android component, therefore we need to specify it in AndroidManfiest.xml

  * Services run in **Main Thread** of hosting process, like any other application objects 

  * `IntentService` class is available in case you need to create service with its own thread

  *  Services have their own context, so there is no need to set or pass in a context to YourService. Instead, just use **this**

  * 

  

# Alarm
 
  * Alarm allows us to perform operation outside of the application lifecycle; we can set time interval 

  * We need to consider the trigger of alarm when we want to have one, these are:
    1. Inexact repeating #setInexactRepeating 
      > Android will synchronize with other applications alarm at the same time
      > Starting API 19, all alarm are set as inexact repeating
    2. Exact repeating #setRepeating

  * We also need to consider the type of alarm:
    1. Elapsed - recommended
    2. RTC (Real time clock)

  * 

# Big Cookie Model
  
  * This model is used mainly to preserve battery life; to reduce radio state transition which plays critical role to device's battery

  * Here are the rule of thumb:
    1. Minimize state transition
    2. Prefetch 2-5 mins of data (1-5 mb)
    3. Batch non time-critical transfers
    4. Bundle time-sensitive / insensitive transfers

# SyncAdapter


# Notification
  
  * If you reuse an id for your app notification, the application will post at most one notification
  *

# Flavors
  
  * Android allows applications to have differrent variation with the same code base 
    > For example, we can have production and mock flavor; where we put all of our production ready code in production package folder and code to mock our app in mock package folder 

# Shared Preference

  * SharedPreferenceChangeListener is triggered after any value is saved to the SharedPreferences file.
  
  * PreferenceChangeListener is triggered before a value is saved to the SharedPreferences file. Because of this, it can prevent an invalid update to a preference. 
    PreferenceChangeListeners are also attached to a single preference.

# PreferenceFragmentCompat
  
  * To use this, add the following line in the build.gradle:

    compile 'com.android.support:preference-v7:25.0.0'
  
  * Don't forget to add the following line in styles.xml if you use `PreferenceFragmentCompat`, else it will crash
            <item name="preferenceTheme">@style/PreferenceThemeOverlay</item>

# Android Components

  * There are four components in Android:
    1. Activity
    2. Provider
    3. Services
    4. Receiver

  * Android's components have their own context, you can get it by calling ``getContext()``