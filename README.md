MaterialViewPager
=======

[![Build Status](https://travis-ci.org/florent37/MaterialViewPager.svg)](https://travis-ci.org/florent37/MaterialViewPager)

[![Video](http://share.gifyoutube.com/KroLAw.gif)](http://www.youtube.com/watch?v=r95Tt6AS18c)

Download
--------

In your root build.gradle add
```groovy
repositories {
    maven {
        url  "http://dl.bintray.com/florent37/maven"
    }
}
```

In your module [![Download](https://api.bintray.com/packages/florent37/maven/MaterialViewPager/images/download.svg)](https://bintray.com/florent37/maven/MaterialViewPager/_latestVersion)
```groovy
compile ('com.github.florent37:materialviewpager:1.0.0@aar'){
    transitive = true
}
```

Usage
--------

Add MaterialViewPager to your activity's layout
```xml
<com.github.florent37.materialviewpager.MaterialViewPager
    android:id="@+id/materialViewPager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:viewpager_logo="@layout/header_logo"
    app:viewpager_logoMarginTop="100dp"
    app:viewpager_color="@color/colorPrimary"
    app:viewpager_headerHeight="200dp"
    app:viewpager_hideLogoWithFade="true"
    app:viewpager_hideToolbarAndTitle="true"
    app:viewpager_enableToolbarElevation="true"
    />
```

You will see on Android Studio Preview :

![alt preview](https://raw.github.com/florent37/MaterialViewPager/master/screenshots/preview_small.png)

**Retrieve the MaterialViewPager**

You can use MaterialViewPager as an usual Android View, and get it by findViewById

```java
public class MainActivity extends ActionBarActivity {

    private MaterialViewPager mViewPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mViewPager = (MaterialViewPager) findViewById(R.id.materialViewPager);
    }
}
```

Toolbar
--------

```java
Toolbar toolbar = mViewPager.getToolbar();

if (toolbar != null) {
     setSupportActionBar(toolbar);

     ActionBar actionBar = getSupportActionBar();
     actionBar.setDisplayHomeAsUpEnabled(true);
     actionBar.setDisplayShowHomeEnabled(true);
     actionBar.setDisplayShowTitleEnabled(true);
     actionBar.setDisplayUseLogoEnabled(false);
     actionBar.setHomeButtonEnabled(true);
}
```

ViewPager
--------
```java
ViewPager viewPager = mViewPager.getViewPager();
viewPage.setAdapter(...);

//After set an adapter to the ViewPager
mViewPager.getPagerTitleStrip().setViewPager(mViewPager.getViewPager());
```

Register your Scrollable
--------

**RecyclerView**

From your fragment
```java
MaterialViewPagerHelper.registerRecyclerView(getActivity(), mRecyclerView, null);
```

**ScrollView**

The ScrollView must be an [ObservableScrollView][android-observablescrollview]
```java
MaterialViewPagerHelper.registerScrollView(getActivity(), mScrollView, null);
```

**WebView - (killed for less...)**

The ScrollView must be an [ObservableWebView][android-observablescrollview]
```java

//must be called before loadUrl()
MaterialViewPagerHelper.preLoadInjectHeader(mWebView);

//have to inject header when WebView page loaded
mWebView.setWebViewClient(new WebViewClient() {
    @Override
    public void onPageFinished(WebView view, String url) {
        MaterialViewPagerHelper.injectHeader(mWebView, true);
    }
    @Override
    public boolean shouldOverrideUrlLoading(WebView view, String url) {
        view.loadUrl(url);
        return true;
    }
});

mWebView.loadUrl("http://...");

MaterialViewPagerHelper.registerWebView(getActivity(), mWebView, null);
```

~~ListView - Deprecated~~
```java
MaterialViewPagerHelper.registerListView(getActivity(), mScrollView, null);
```

Animate Header
--------

Simply listen to the ViewPager Page Change and modify the header's **color and image**

```java
//it's a sample ViewPagerAdapter
mViewPager.setAdapter(new FragmentStatePagerAdapter(getSupportFragmentManager()) {

            int oldPosition = -1;

            @Override
            public Fragment getItem(int position) {
                return RecyclerViewFragment.newInstance();
            }

            @Override
            public int getCount() {
                return 4;
            }

            @Override
            public CharSequence getPageTitle(int position) {
                return "Tab "+position;
            }

            //called when the current page has changed
            @Override
            public void setPrimaryItem(ViewGroup container, int position, Object object) {
                super.setPrimaryItem(container, position, object);

                //only if position changed
                if(position == oldPosition)
                    return;
                oldPosition = position;

                int color = 0;
                String imageUrl = "";
                switch (position){
                    case 0:
                        imageUrl = "http://cdn1.tnwcdn.com/wp-content/blogs.dir/1/files/2014/06/wallpaper_51.jpg";
                        color = getResources().getColor(R.color.blue);
                        break;
                    case 1:
                        imageUrl = "https://fs01.androidpit.info/a/63/0e/android-l-wallpapers-630ea6-h900.jpg";
                        color = getResources().getColor(R.color.green);
                        break;
                    case 2:
                        imageUrl = "http://www.droid-life.com/wp-content/uploads/2014/10/lollipop-wallpapers10.jpg";
                        color = getResources().getColor(R.color.cyan);
                        break;
                    case 3:
                        imageUrl = "http://www.tothemobile.com/wp-content/uploads/2014/07/original.jpg";
                        color = getResources().getColor(R.color.red);
                        break;
                }

                final int fadeDuration = 400;

                //change header's color and image
                mViewPager.setImageUrl(imageUrl,fadeDuration);
                mViewPager.setColor(color,fadeDuration);

            }

        });
```

Customisation
--------



Dependencies
--------

* [Picasso][picasso] (from Square)
* [KenBurnsView][kenburnsview] (from flavioarfaria)
* [Material PagerSlidingTabStrip][pagerslidingtitlestrip] (from jpardogo, forked from astuetz)
* [Android-Observablescrollview][android-observablescrollview] (from ksoichiro)
* Android Support v7
* Android Support v7 - RecyclerView
* Android Support v7 - CardsView

Credits
-------

Author: Florent Champigny

<a href="https://plus.google.com/+florentchampigny">
  <img alt="Follow me on Google+"
       src="https://raw.githubusercontent.com/florent37/DaVinci/master/mobile/src/main/res/drawable-hdpi/gplus.png" />
</a>
<a href="https://twitter.com/florent_champ">
  <img alt="Follow me on Twitter"
       src="https://raw.githubusercontent.com/florent37/DaVinci/master/mobile/src/main/res/drawable-hdpi/twitter.png" />
</a>
<a href="https://www.linkedin.com/profile/view?id=297860624">
  <img alt="Follow me on LinkedIn"
       src="https://raw.githubusercontent.com/florent37/DaVinci/master/mobile/src/main/res/drawable-hdpi/linkedin.png" />
</a>


License
--------

    Copyright 2015 florent37, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


[snap]: https://oss.sonatype.org/content/repositories/snapshots/
[picasso]: https://github.com/square/picasso
[kenburnsview]: https://github.com/flavioarfaria/KenBurnsView
[pagerslidingtitlestrip]: https://github.com/jpardogo/PagerSlidingTabStrip
[android-observablescrollview]: https://github.com/ksoichiro/Android-ObservableScrollView