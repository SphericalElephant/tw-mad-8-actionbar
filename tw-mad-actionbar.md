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

![ActionBar (source: http://developers.android.com/)](../actionbar-item-withtext.png)


![ActionBar (source: http://developers.android.com/)](../actionbar-tabs@2x.png)

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

# ActionBar Menu

## ActionBar - Menu 1

* The Menu resource for the ActionBar and ActionBarSherlock are compatible, the MenuInflater you need to use depends on the implementation though!
* As other layout / menu resources, an item can have an android:id, which is used to refer to the item from code
* android:title is the text to be displayed
* android:icon can be used to specify an image resource, that will be displayed left to the text, if certain conditions are met

## ActionBar - Menu 2

* android:showAsAction is used to specify if and how an item will be shown, it takes the following parameters, that can also be combined using the | operator
	* ifRoom - item will only be shown if there is room for it
	* withText - the text is shown as well
	* never - item is never shown
	* always - item will always be shown, careful, can result in overlapping ActionBar items!
* If there is no space left, items will be placed in the overflow menu (currently no icon support!)

## ActionBar - Menu 3

* In addition to simple ActionBar items, you may use so called ActionViews
* An ActionView is defined in a layout file (very much like a layout you would normally use for Activities / Fragments)
* Make sure to keep it simple when using ActionViews
* Do you really need the ActionView?
* Prominent example: SearchView (ActionbarSherlock has a compat version.)


## ActionBar - Menu 4

```xml
<?xml version="1.0" encoding="utf-8"?> 
<CheckBox
	xmlns:android="http://schemas.android.com/apk/res/android" 
	android:layout_width="match_parent" 
	android:layout_height="match_parent" />
```

## ActionBar - Menu 5

```xml
<?xml version="1.0" encoding="utf-8"?> 
<menu 
    xmlns:android="http://schemas.android.com/apk/res/android" > 
    <item android:id="@+id/mymenu_test" 
          android:title="test" 
          android:showAsAction="ifRoom"/> 
    <item android:id="@+id/mymenu_actionview" 
          android:actionLayout="@layout/myactionview" 
          android:showAsAction="ifRoom"/> 
</menu>
```

## ActionBar - Menu 6

```java
@Override 
public boolean onCreateOptionsMenu(com.actionbarsherlock.view.Menu menu) { 
	MenuInflater i = new MenuInflater(this); 
	i.inflate(R.menu.mymenu, menu); 
	((CheckBox) menu.findItem(R.id.mymenu_actionview)
		.getActionView())
		.setOnCheckedChangeListener(
		new OnCheckedChangeListener() {

		@Override 
		public void onCheckedChanged(
			CompoundButton buttonView, 							boolean isChecked) { 
			System.out.println("Check Changed: " + 
				isChecked); 
		} 
	}); 
	return super.onCreateOptionsMenu(menu); 
}
```

## ActionBar - Tabs 1

* As mentioned before, ActionBar supports tabs
* Tabs should be used for navigation
* If you are using an ActionBar, do not use TabWidget, instead, add the tabs to the ActionBar directly
* Usually tabs are used to switch between Fragments – ActionBar.TabListener will help you do that

## ActionBar - Tabs 2

```java
@Override 
public void onCreate(Bundle savedInstanceState) { 
	super.onCreate(savedInstanceState); 
	setContentView(R.layout.activity_main); 
	ActionBar b = getSupportActionBar(); 
	b.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);
	b.setDisplayShowTitleEnabled(false);
	Tab tab1 = b.newTab().setText("SomeText") 
		.setTabListener(t); 
	Tab tab2 = b.newTab().setText("SomeText1") 
		.setTabListener(t); 
	b.addTab(tab1); 
	b.addTab(tab2);
```

## ActionBar - Tabs 3

```java
ActionBar.TabListener t = new ActionBar.TabListener() { 
	@Override 
	public void onTabUnselected(Tab tab,
		FragmentTransaction ft) { 
	} 

	@Override 
	public void onTabSelected(Tab tab,
		FragmentTransaction ft) { 
		ft.replace(R.id.somefragmentid, someFragment);
		// DO NOT COMMIT THE TRANSACTION!
	} 

	@Override 
	public void onTabReselected(Tab tab,
		FragmentTransaction ft) {
	} 
};
```

## ActionBar - Tabs 4

* Do not commit the FragmentTransaction provided by any callback method of * ActionBar.TabListener, Android will do that for you! In addition, you will not be able to place these FragmentTransactions on the back stack
* Sometimes, Android will use a drop down list to display the tabs (one instance: tablet in portrait mode)

## ActionBar - ActionMode 1

* CAB – Contextual Action Bar!
* The ActionMode allows developers to create a contextual ActionBar
Examples:
	* Deleting multiple items from a list (i.e. selecting multiple items)
	* Marking text and offering text edit options
* Usually, regular parts of the UI are replaced as long is the ActionMode is active
* Can be triggered by several options, developer’s choice (e.g. long click on an item)

## ActionBar - ActionMode 2

* In order to implement an ActionMode, one has to work with the ActionMode.Callback interface
* The UI of the ActionMode is defined using XML
	* Basically, it is nothing more than a menu
* The ActionMode.Callback interface provides the following callback methods that need to be implemented
* onCreateActionMode(ActionMode mode, Menu menu)
	* Called when the ActionMode is created, use mode.getMenuInflater() to inflate your menu based UI. You must return true here, otherwise your ActionMode will not be shown!

## ActionBar - ActionMode 3

* onPrepareActionMode(ActionMode mode, Menu menu)
	* Gets called every time the ActionMode has been invalidated, return true here if you want to inform the system that an update has occurred
* onActionItemClicked(ActionMode mode, MenuItem item)
	* This callback method allows you to intercept item clicks (very much like onOptionsItemSelected, return true here if you have handled the event here and don’t wish to distribute the event any further
* onDestroyActionMode(ActionMode mode)
	* Do your cleanup here

## ActionBar - ActionMode 4

* You can start the ActionMode by using Activity.startActionMode(callback)
* As you can see, the ActionMode as such is defined by your implementation of the ActionMode.Callback interface
* Please refer to [http://developer.android.com/guide/topics/ui/menus.html#CAB](http://developer.android.com/guide/topics/ui/menus.html#CAB) in order to learn about standard procedures regarding multiselect (ListView, etc) and triggering the CAB

# Compatibility ActionBar 

## AppCompat 1

* If you want to, you can use the Compatibility ActionBar provided by Google
* Was added to the compatibility v7 library in revision 18 (July 2013)
* The v7 compatibility library runs from API level 7 onwards -> Android 2.1
In order to use this implementation of the ActionBar pattern, you will have to use a special theme: Theme.AppCompat (sounds familiar, doesn’t it)

## AppCompat 2

* Unlike ActionBarSherlock, which dispatches calls to the ActionBar if the currently running Android version nativly supports the ActionBar, the compatibility version does not do that

## AppCompat 3

* In order to make use of the compatibility ActionBar, you need to use the ActionBarActivity which itself is a v4 FragmentActivity.
* Instead of using getActionBar(), you need to use getSupportActionBar() 
* There are several other onSupport* methods that can be implemented in order to change the ActionBar’s behaviour (like “up” navigation behaviour)

# Conclusion

## Conclusion
* The ActionBar pattern is important. It replaces the menu (button) on newer devices, even though there are backwards compatible menus available.
* There is much more to ActionBar, please check out the documentation
	* ActionProvider
	* Drop down navigation
Theming