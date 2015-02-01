# Lab 3: Multiple Activities

## Adding a new activity

Here are the steps to add a new acitivity into an existing application project:

1. Select **File** > **New...** > **Activity**

2. Select one of the provided activity templates. Here, we select **Blank Activity**.

3. Set the class name, the layout name, and the title. Then, we have the activity added to the project.

## Starting an activity

To start a new activity from the current activity, we need to create an object of class `Intent` as follow.

```java
Intent i = new Intent(this, Activity2.class);
startActivity(i);
```
Here, we create an intent `i` for `Activity2`. Then, the activity is started via the intent. This activity is created and put on top of the current activity.

To end an activity and return back to the parent activity, we use

```java
finish();
```

## Starting an activity and Passing values

We can pass values to the activity to be created by attaching the values with the intent.

```java
Intent i = new Intent(this, Activity2.class);
i.putExtra("value1", v);
startActivity(i);
```

Each passed value is referred by a string constant. Then, the value can be accessed in the target activity as follow.

```java
String input;

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_activity3);

  // Get the intent used to create this activity
  Intent i = this.getIntent();
  // Get a string value named "value1"
  input = i.getStringExtra("value1");
}
```

## Starting an activity and Waiting for results

We can use `startActivityForResult` instead of `startActivity` to create a new activity and wait for values returned from the new activity.

When the new activity ends, the `onActivityResult` method of the parent activity will be called.

```java
// Parent Activity
Intent i = new Intent(this, Activity3.class);
i.putExtra("value1", v);
startActivityForResult(i, 88);
```

`88` is the request code. It will be sent back from the child activity to confirm the type of request.

Here is how to return the value,

```java
// Child Activity
Intent res = new Intent();
res.putExtra("retValue", r);
setResult(RESULT_OK, res);
finish();
```

Then, we need to implement `onActivityResult` in the parent activity to obtain the returned values.

```java
// Parent Activity
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
  TextView tvResult = (TextView)findViewById(R.id.tvResult);

  if (requestCode == 88) {
    if (resultCode == RESULT_OK) {
      String result = data.getStringExtra("retValue");
      tvResult.setText(result);
    }
    else if (resultCode == RESULT_CANCELED) {
      tvResult.setText("CANCELED");
    }
  }
}
```

We check both the request and result codes before accessing the values from the child activity. When, the user clicks on *Back* button to end the child activity the result code is automatically set to `RESULT_CANCELED`.

See https://github.com/its333y2014/Lab3Example


## Exercise

Fork and clone the exercise repository at https://github.com/its333y2014/Lab3Exercise1

Complete the Android application project to make it a GPA calculator.

- When the user clicks on `Add Course` button, the `CourseActivity` is created and started. This activity allows the user to input the credit and grade for a new course.

- The values are then sent back to the `MainActivity` and used for calculating the GPA.

- When the user clicks on `Reset` button, all the accumulated values are reset to 0.
