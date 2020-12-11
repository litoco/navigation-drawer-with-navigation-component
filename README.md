# Navigation Drawer with Navigation Component

# Prerequisites: 
**None (just follow along with this documentation)**

# Steps:
**1. Create an empty android repository**

**2. Add dependencies:**
  In the app level build.gradle file, add the following dependencies:
  ```
  def nav_version = "<latest navigation component version>"
  implementation "androidx.navigation:navigation-fragment:$nav_version"
  implementation "androidx.navigation:navigation-ui:$nav_version"
  ```
  [latest navigation component's version can be found here](https://developer.android.com/jetpack/androidx/releases/navigation)
 
**3. Create fragments:**
  Create fragments for different screens of your application.\
  To add new fragment go to `File(right click)>new>Fragment>BlankFragment`.
  
**4. Add fragments to navigation graph:**
  Add these fragments [navigation graph](https://developer.android.com/jetpack/androidx/releases/navigation).\
  To add a navigation graph to your project, do the following:\
  i. &nbsp;&nbsp;In the Project window, right-click on the res directory and select `New > Android Resource File`. \
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The New Resource File dialog appears.\
  ii. &nbsp;Type a name in the File name field, such as "nav_graph".\
  iii. Select Navigation from the Resource type drop-down list, and then click OK.
  
  Now to add fragments to the navigation graph:\
  i. &nbsp;&nbsp;In the design window of navigation graph there is `new destination icon` (square with + icon)\
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click on the `new destination icon` to add fragments from the popup dialog.\
  ii. &nbsp;Make any fragment as home fragment by selecting the framgent and then clicking the `home button icon`\
  iii. In the code window of navigation graph change the `android:label` of `<fragment>` to what you want to be displayed on the 
  &nbsp;&nbsp;&nbsp;&nbsp;toolbar
  
**5. Remove the default action bar:**
  Default action bar can be found in `res>values>style` or `res>values>themes>both files here` change (Dark/Light)ActionBar to NoActionBar
  
**6. Add navigation UI and toolbar in xml:**
  It must follow the following standard:
  ```
  <androidx.drawerlayout.widget.DrawerLayout
   ...
   ...
   ...>
   
     <!-- Drawer will slide over this-->
     <androidx.constraintlayout.widget.ConstraintLayout
     ...
     ...
     ...>

       <Toolbar
       ...
       ...
       .../>
       
       <androidx.fragment.app.FragmentContainerView
            android:id="@+id/nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toBottomOf="@id/toolbar"
            app:layout_constraintBottom_toBottomOf="parent"
            app:defaultNavHost="true"
            app:navGraph="@navigation/nav_graph" />

     </androidx.constraintlayout.widget.ConstraintLayout>

   <com.google.android.material.navigation.NavigationView
   ...
   ...
   .../>
   </androidx.drawerlayout.widget.DrawerLayout>
  ```
  
**7. Add navigation UI and toolbar in Java:**
  Create global Objects of AppBarConfiguration, NavController, DrawerLayout
  ```
  public void onCreate(Bundle saveInstanceState){
  ...
  ...
  ...
  drawerLayout = findViewById(R.id.drawer_layout);
  NavHostFragment navHostFragment = (NavHostFragment) getSupportFragmentManager().findFragmentById(R.id.nav_host_fragment);
  navController = Objects.requireNonNull(navHostFragment).getNavController();
  AppBarConfiguration appBarConfiguration = new AppBarConfiguration.Builder(navController.getGraph())
                        .setOpenableLayout(drawerLayout)
                        .build();
  NavigationUI.setupWithNavController(toolbar, navController, appBarConfiguration);
  NavigationView navView = findViewById(R.id.nav_view);
  NavigationUI.setupWithNavController(navView, navController);
  }

  @Override
  public boolean onOptionsItemSelected(MenuItem item) {
      return NavigationUI.onNavDestinationSelected(item, navController)
              || super.onOptionsItemSelected(item);
  }
  ```
  
  **NavHostFragment:** It is used to show the corresponding destination fragment's UI, that the user has navigated to.\
  **NavController:** Responsible for updating the navhostfragment with the new fragment, that the user has navigated to.\
  **AppBarConfiguration:** It is used to manage the behaviour of the navigation icon (hamburger icon). Top level destinations show hamburger icon while child level destination show back arrow.

**8. Add navigation menu and tie menu item with their destinations:**
  Create a file for navigation drawer menu. (Lets say drawer_menu.xml)
  add items to the drawer menu:
  ```
  <menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
        android:title="Item1"/>
    <item android:id="@+id/item2"
        android:title="Item2"/>
    ...
    ...
    ...
  </menu>
  ```
  >Note:It is abosolutely necessary that the id mentioned here are same as mentioned in the "nav_graph" that we created in step 4\
  For example:
  ```
  <menu ...>
    <item android:id="@+id/item1"
        .../>
    ...
    ...
    ...
  </menu>
  ```
  ```
  <navigation
   ...
   ...
   ...>
    <fragment
        android:id="@+id/item1"
        ...
        ...
        .../>    
   </navigation>
  ```

**9. Run the app on a device.**\
**10. `Congratulations!!!` You have successfully added navigation drawer to your app**
