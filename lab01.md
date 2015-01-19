# Lab 1: Introduction to Android Application Development

## Creating an Android project

Here are the steps to create a project in **Android Studio** for developing an Android application.

1. Select **Start a new Android Studio project** from the main window.
![Starting window of Android Studio](https://raw.githubusercontent.com/its333y2014/manual/master/figures/0101.png)

2. Set the Application name, Company Domain, and Project location.
![Set app name](https://raw.githubusercontent.com/its333y2014/manual/master/figures/0102.png)

3. Select the form factor, and the minimum Android SDK version.
![Set SDK version](https://raw.githubusercontent.com/its333y2014/manual/master/figures/0103.png)

4. Select the type of activity to be created with the project.
![Select activity type ](https://raw.githubusercontent.com/its333y2014/manual/master/figures/0104.png)

5. Set the activity name and the layout name, and click **Finish**.
![Select activity type ](https://raw.githubusercontent.com/its333y2014/manual/master/figures/0105.png)

6. Android Studio shows the project window.
![Select activity type ](https://raw.githubusercontent.com/its333y2014/manual/master/figures/0106.png)



## Running the application

We need an Android device or a virtual Android device to test our application.

### Creating a virtual device

1. Select **Tools** > **Android** > **AVD Manager** menu. This shows the **AVD Manager**. Click **Create a virtual device** to create a new one.
![Create AVD 1](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01avd01.png)

2. Select a device definition and click Next.
![Create AVD 2](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01avd02.png)

3. Select a system image to be used with the device definition, and click Next.
![Create AVD 3](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01avd03.png)

4. Verify the selected configuration.
![Create AVD 4](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01avd04.png)

5. The virtual device is finally created.
![Create AVD 5](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01avd05.png)

### Running

1. Select **Run** > **Run 'app'** menu.
2. Select the virtual device or device to run the application and click OK.
3. Wait until the application starts working.

<img src="https://raw.githubusercontent.com/its333y2014/manual/master/figures/01run.png" width="33%" alt="Running App" />

## Activity

An Android application composes of at least one activity. Each activity is a Java class extended from [``android.app.activity``](http://developer.android.com/reference/android/app/Activity.html).

![Application and Activity ](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01activity.png)

In this lab, we focus on developing an application with single activity.
Here is the source code of the ``MainActivity`` class automatically created with the project.

```java
package th.ac.tu.siit.cholwich.myapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;


public class MainActivity extends Activity {

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    // Called when this activity is create.
    super.onCreate(savedInstanceState);

    // Set the UI of the activity according to the layout
    // in res/layout/activity_main.xml
    setContentView(R.layout.activity_main);
  }

  ...
}
```

``onCreate`` method is called when we start the application and this activity is being created. It is typically used to (1) load the layout file for the user interface (UI), (2) create objects, and (3) initialize values of variables.

## Layout

A layout is a collection of UI components (or Views) for interacting with the user. It is written in XML format.

Here is the source code of the ``activity_main.xml`` layout created with the project. More explanations can be found in the comments.

```xml
<RelativeLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:tools="http://schemas.android.com/tools"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:paddingLeft="@dimen/activity_horizontal_margin"
  android:paddingRight="@dimen/activity_horizontal_margin"
  android:paddingTop="@dimen/activity_vertical_margin"
  android:paddingBottom="@dimen/activity_vertical_margin"
  tools:context=".MainActivity">

  <!--
  @dimen/activity_horizontal_margin, and
  @dimen/activity_vertical_margin are defined in res/values/dimens.xml
  -->

  <TextView
  android:text="@string/hello_world"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content" />

  <!--
  android:text = a string displayed by the TextView
  @string/hello_world = a string constant defined in res/values/strings.xml
  android:layout_width = the width of the TextView
  android:layout_height = the height of the TextView
  wrap_content = fit with the size of the string content
  -->

</RelativeLayout>
```

Android Studio provides a GUI to modify the layout without editing the XML source code.

## Handling Button Clicks

``Button`` is one of the most common views used to make the activity start working.

This is an example to change from *"Hello world!"* to *"The Button is clicked!"* when the user clicks the button.

0. Open ``activity_main.xml`` to add a button. Here, we use the **Design** view of the layout.

1. Drag ``Button`` from the **Palette** pane on the left side. Then, set the ``layout:width`` property in the **Properties** pane to ``match_parent``.
![Adding a Button ](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01event01.png)

2. Set the ``onClick`` property to ``buttonClicked``. This is the name of the method in ``MainActivity`` called when the button is clicked.
![buttonClicked method ](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01event03.png)

3. Open ``MainActivity.java`` and add the ``buttonClicked`` method. This method refers to ``TextView`` class. We need to import ``android.widget.TextView`` into the ``MainActivity`` class.

  ```java
  public void buttonClicked(View v) {
    TextView t = (TextView)findViewById(R.id.textView);
    t.setText("The button is clicked!");
  }
  ```

4. We can run the application.

![Event Handling](https://raw.githubusercontent.com/its333y2014/manual/master/figures/01event_handling.png)

### findViewById

Each UI component in Android can be called a view. The ``findViewById`` method is used to obtain the reference to a view specified by an id.

For example, ``findViewById(R.id.textView)`` returns a reference to the TextView object in the layout.

The id of a view can be retrieved and set in the **Properties** pane of the layout designer. However, it may automatically be assigned when we add more than one view into the layout.

## Exercise 1

1. Add an additional `TextView` into the layout, set its ID to `counterView`, and set its initial text to `0`.

2. Add an additional `Button` into the layout, and set its text to `Increase`.

3. Add an additional `Button` into the layout, and set its text to `Decrease`.

4. Modify the `MainActivity` class to make the value shown by `counterView` be increased by 1 when the user clicks on the `Increase` button, and the value be decreased by 1 when the user clicks on the `Decrease` button.

**Hints**

- We can define a variable as a member of the `MainActivity` class to keep the counter value.

- Use `Integer.toString(i)` to convert an integer value to a strong when `i` is an integer variable.

## Exercise 2

1. Log into http://github.com. You need to sign up for an account on GitHub before you can do this exercise.

2. Fork the project for this exercise at https://github.com/its333y2014/Lab1Exercise2.

3. Clone the fork project to the local machine. This can be done by selecting **VCS** > **Checkout from Version Control** > **GitHub**.

4. Complete the `MainActivity` class to make the application work as a temperature converter.

<center><img src="https://raw.githubusercontent.com/its333y2014/manual/master/figures/01exercise201.png" width="33%" alt="Temperature Converter" /></center>

**Hints**

- Check `res/layouts/activity_main.xml` for the IDs of `TextView` and `EditText`.

- Use the following snippet to get the input string from an `EditText`
  ```java
  EditText etInput = (EditText)findViewById(R.id.etInput);
  String s = etInput.getText().toString();
  ```

- Use `Double.parseDouble(s)` to convert a string `s` to a double value.

- To get the selected radio button, we need to refer to the `RadioGroup` and call `getCheckedRadioButtonId()` to obtain the ID of the selected button. For example,

  ```java
  RadioGroup rgFrom = (RadioGroup)findViewById(R.id.rgFrom);
  int selFrom = rgFrom.getCheckedRadioButtonId();

  if (selForm == R.id.rbFrmC) { //The user wants to convert from Celsius.
    ...
  }
  ```

- We have two radiogroups in the layout i.e. `rgFrom` and `rgTo`.

- Refer to http://en.wikipedia.org/wiki/Conversion_of_units_of_temperature for the conversion between temperature units.

- After finished this exercise, we can upload the source files to GitHub by
  - Select **VCS** > **Git** > **Commit**
  - Write a message for this commit
  - Click **Commit and Push** to commit the changes and push them to GitHub
  - Click **Push** again to push.
