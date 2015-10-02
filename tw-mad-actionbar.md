% ActionBar / Toolbar
% Patrick Sturm
% 02.10.2015

# ActionBar

## Resources

* Resources
	* ActionBar: [http://developer.android.com/guide/topics/ui/actionbar.html](http://developer.android.com/guide/topics/ui/actionbar.html)
	* ActionBarSherlock: [http://actionbarsherlock.com/](http://actionbarsherlock.com/)
* Helpful Stuff: 
	* [http://jgilfelt.github.io/android-actionbarstylegenerator/](http://jgilfelt.github.io/android-actionbarstylegenerator/)
	* [http://android-holo-colors.com/](http://android-holo-colors.com/)
	

## ActionBar - Introduction 1

* The ActionBar was introduced in Honeycomb
* The actual ActionBar UI design pattern has been around before that
* Fact is: Google saw that a lot of applications were using the ActionBar UI pattern and they decided to provide a default implementation
* The ActionBar is available on Honeycomb+, but there are ways to provide a compatibility ActionBar using ActionBarSherlock or the support library ActionBar
* Android 5.x's material design added a new form of the ActionBar: Toolbar
* Overall, there are 4 (yes 4!!!!111) flavors of "ActionBars" that can be found in the wild ...
* ... we will cover all of them <3

## ActionBar - Introduction 2

![ActionBar1 (source: http://developers.android.com/)](../actionbar-item-withtext.png)


![ActionBar1 (source: http://developers.android.com/)](../actionbar-tabs@2x.png)

## ActionBar - Concepts 1

* The ActionBar can hold multiple items
* Each item can have a text and an image related to it
* You can control what items will be shown in the Action
* You can also control what will be shown
	* Text
	* Image
	* Both
* Items that do not fit the ActionBar (limited space) are displayed in the overflow menu
* The ActionBar supports tabbed navigation

## ActionBar - Concepts 2

* The ActionBar uses extended menu layouts to inflate content
* In addition to the “back” button, ActionBar features the “up” button
	* The “up” button should navigate to the next logical step in the current application
	* Example:
		* If you share a website using gmail, Android will open the gmail compose window, if you press “up”, your inbox will open

## ActionBar - Concepts 3

![ActionBar1 (source: http://developers.android.com/)](../back.png)

## ActionBar - Concepts 4

![ActionBar1 (source: http://developers.android.com/)](../up.png)

## ActionBar - Acccess

* The ActionBar can be accessed using:
	* ActionBar b = getActionBar();
* For ActionBarSherlock
	* ActionBar b = getSupportActionBar();

# ActionBarSherlock

## ActionBarSherlock - Why / How? 1

* ActionBarSherlock is deprecated now, but you might have the pleasure to support a legacy app, you need to know about it!
* ActionBarSherlock is an extension for the support library provided by Google
* As mentioned before, your App should support as many Android versions as possible, if you use the normal ActionBar implementation, your App will only run on Honeycomb+
* ActionBarSherlock will use the normal ActionBar where applicable. On a mobile device running ICS for instance, the normal ActionBar will be used, on older devices, it will create an ActionBar that resembles the original
* Don’t worry about that, it’s all abstracted

## ActionBarSherlock - Why / How? 2
* As with the support library, you cannot use normal Activities / Fragments to access the ActionBar
* Support library
	* FragmentActivity
* ActionBarSherlock
	* SherlockActivity
	* SherlockFragment
* Other, specialized versions are supported as well
	* SherlockListFragment / SherlockListActivity / SherlockDialogFragment / etc.
* **In order for ActionBarSherlock to work you must use a Sherlock theme!**

## ActionBarSherlock - Why / How? 3
```xml
    <application 
        android:icon="@drawable/ic_launcher" 
        android:label="@string/app_name" 
        android:theme="@style/Theme.Sherlock" > 
    </application> 
```

## ActionBarSherlock - Relevant Methods 1
* onCreateOptionsMenu(Menu menu)
	* Menu is an instance of com.actionbarsherlock.view.Menu
	* Use this method to inflate your menu
	* The menu instance passed to this method is the actual menu that will be displayed in the ActionBar
	* Must return true in order for the menu to appear
* onOptionsItemSelected(MenuItem item)
	* The default callback method that will be invoked when a user presses an item
	* No need to add callback manually (similar to ListActivity)
	* Can return true / false, event can be consumed by returning true

## ActionBarSherlock - Relevant Methods 2

* setHasOptionsMenu(boolean hasMenu)
	* This is a Fragment method
	* You need to use it if you want your Fragments to be able to participate in menu creation
setHasOptionsMenu(true)
	* If you don’t do this, otherwise, onCreateOptionsMenu(…) will never be called!

## ActionBarSherlock - Example 1

```java
@Override 
public void onCreate(Bundle savedInstanceState) { 
	super.onCreate(savedInstanceState); 
	setContentView(R.layout.activity_main); 
	ActionBar a = getSupportActionBar(); 
}
```

## ActionBarSherlock - Example 2

```java
@Override 
public boolean onCreateOptionsMenu(
	com.actionbarsherlock.view.Menu menu) { 
	MenuInflater i = new MenuInflater(this); 
	i.inflate(R.menu.mymenu, menu); 
	return super.onCreateOptionsMenu(menu); 
}
```

## ActionBarSherlock - Example 3

```java
@Override 
public boolean onOptionsItemSelected(MenuItem item) { 
	if (item.getItemId() == R.id.mymenu_test) { 
		System.out.println("test pressed"); 
	} 
	return super.onOptionsItemSelected(item); 
}
```

## ActionBarSherlock - Example 4

```xml
<?xml version="1.0" encoding="utf-8"?> 
<menu 
xmlns:android="http://schemas.android.com/apk/res/android" > 
    <item android:id="@+id/mymenu_test" 
          android:title="test" 
          android:showAsAction="ifRoom"/> 
</menu>
```


## ActionBarSherlock - Menu 1

* The Menu resource for the ActionBar and ActionBarSherlock are compatible, the MenuInflater you need to use depends on the implementation though!
* As other layout / menu resources, an item can have an android:id, which is used to refer to the item from code
* android:title is the text to be displayed
* android:icon can be used to specify an image resource, that will be displayed left to the text, if certain conditions are met

## ActionBarSherlock - Menu 2

* android:showAsAction is used to specify if and how an item will be shown, it takes the following parameters, that can also be combined using the | operator
	* ifRoom - item will only be shown if there is room for it
	* withText - the text is shown as well
	* never - item is never shown
	* always - item will always be shown, careful, can result in overlapping ActionBar items!
* If there is no space left, items will be placed in the overflow menu (currently no icon support!)

## ActionBarSherlock - Menu 3

* In addition to simple ActionBar items, you may use so called ActionViews
* An ActionView is defined in a layout file (very much like a layout you would normally use for Activities / Fragments)
* Make sure to keep it simple when using ActionViews
* Do you really need the ActionView?
* Prominent example: SearchView (ActionbarSherlock has a compat version.)
