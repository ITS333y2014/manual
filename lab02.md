# Lab 2: Event Handling

## Handle Click Events

There are multiple ways to handle click events. Let's explain them using the basic counter exercise from the previous lab.

<center><img src="https://raw.githubusercontent.com/its333y2014/manual/master/figures/02basiccounter.png" width="40%" alt="Temperature Converter" /></center>

### Using `onClick` property

Create a function for each on the button and set it to the `onClick` property of that button.

```java  
  int counter = 0;

  // set incClicked to the onClick property of Increase button
  public void incClicked(View v) {
    counter++;
    TextView tv = (TextView)findViewById(R.id.tvOutput);
    tv.setText(Integer.toString(counter));
  }

  // set decClicked to the onClick property of Decrease button
  public void decClicked(View v) {
    counter--;
    TextView tv = (TextView)findViewById(R.id.tvOutput);
    tv.setText(Integer.toString(counter));
  }
```

Create a function to handle events for multiple buttons. We can identify the button originating the click from the button's id.

```java  
int counter = 0;

// set buttonClicked to the onClick property of both Increase
// and Decrease buttons.
public void buttonClicked(View v) {
  int id = v.getId();

  switch (id) {
    case R.id.btInc: // Increase botton's id
      counter++;
      break;
    case R.id.btDec: // Decrease button's id
      counter--;
      break;
  }

  TextView tv = (TextView)findViewById(R.id.tvOutput);
  tv.setText(Integer.toString(counter));
}
```

### Using `OnClickListener` interface

Create an object implementing `OnClickListener` and use `setOnClickListener` method of a button to specify the method to be executed when the button is clicked.

```java
public class MainActivity extends ActionBarActivity implements View.OnClickListener {

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    Button btInc = (Button)findViewById(R.id.btInc);
    btInc.setOnClickListener(this);

    Button btDec = (Button)findViewById(R.id.btDec);
    btDec.setOnClickListener(this);
  }

  int counter = 0;

  @Override
  public void onClick(View v) {
    int id = v.getId();
    switch (id) {
      case R.id.btInc:
      counter++;
      break;
      case R.id.btDec:
      counter--;
      break;
    }
    TextView tv = (TextView)findViewById(R.id.tvOutput);
    tv.setText(Integer.toString(counter));
  }
```

## Toast

Toast is a basic way to make a short notification after the application has updated or performed some tasks.

For example, we update the `onClick` method of the BasicCounter to show a toast when the counter has reached the value divisible by 10.

```java
@Override
public void onClick(View v) {
  int id = v.getId();
  switch (id) {
    case R.id.btInc:
    counter++;
    break;
    case R.id.btDec:
    counter--;
    break;
  }
  TextView tv = (TextView)findViewById(R.id.tvOutput);
  tv.setText(Integer.toString(counter));

  if (counter % 10 == 0) {
    Toast t = Toast.makeText(this.getApplicationContext(),
    "The counter is " + counter,
    Toast.LENGTH_SHORT);
    t.show();
  }
}
```

The following figure shows how the toast is displayed.

<center><img src="https://raw.githubusercontent.com/its333y2014/manual/master/figures/02toast.png" width="40%" alt="Temperature Converter" /></center>

## Exercise

Complete the provided **Calculator** application project to make it work similar to the calculator application on Nexus 4. This calculator app supports only *integer calculator*.

To do this exercise, we need to fork and clone https://github.com/its333y2014/Lab2Exercise1.git

Here are the detailed information related to the application and the features that we need to implement:

1. The main layout composes of two textviews i.e. `tvExpr` and `tvAns`.

2. `tvExpr` displays the expression that the user wants to calculate. It is always set to the value of `expr` variable. For example, `expr` can be `1234+5`.

3. `tvAns` displays the result of the calculation from `expr` when it can be calculated.

3. The UI layout composes of several buttons. They can be categorized into 3 types i.e. digit buttons (`0`-`9`), operator buttons (`+`, `-`, `*`, `/`), equal sign button (`=`), clear buttons (`AC`, `BS`), and memory-related buttons (`M+`, `M-`, `MC`, `MR`).

4. When the user clicks on one of the digit buttons, the digit corresponding to that button is appended to `expr`. This feature has already been implemented in the `digitClicked` method.
  ```java
  public void digitClicked(View v) {
    //d = the label of the digit button
    String d = ((TextView)v).getText().toString();
    //append the clicked digit to expr
    expr.append(d);
    //update tvExpr
    updateExprDisplay();
    //calculate the result if possible and update tvAns
    recalculate();
  }
  ```

5. When the user clicks on one of the operator buttons, the operator is append to `expr`. Note that we cannot not input any operator when `expr` is empty, and two consecutive operators cannot be input. We need to implement this feature in `operatorClicked` method.

6. When the user clicks on the equal sign button (`=`), the current result will replace `expr`.

7. To calculate the expression in `expr`, we can use `split` method in `String` class to separate between numbers and operators in `expr`. Here is how to split `expr`. Note that the output is an array of strings named `tokens`.

  (Reference: http://stackoverflow.com/questions/2206378/how-to-split-a-string-but-also-keep-the-delimiters)
  ```java
  //Split expr into numbers and operators
  //e.g. 123+45/3 --> ["123", "+", "45", "/", "3"]
  String e = expr.toString();
  String[] tokens = e.split("((?<=\\+)|(?=\\+))|((?<=\\-)|(?=\\-))|((?<=\\*)|(?=\\*))|((?<=/)|(?=/))");
  ```

7. The expression in `expr` is evaluated from left to right without considering the operator precedence. For example, `123+45/3` is evaluated as `(123+45)/3 = 168/3 = 56`.

8. `M+`, `M-`, `MR` and `MC` buttons are for the memory features of the calculator. The calculator has a memory to store one integer.

  - `M+` is for adding the current result to the memory.
  - `M-` is for subtracting the current result from the memory.
  - `MR` is for displaying the value of the memory to `tvAns` and `tvExpr`.
  - `MC` is for setting the value of the memory to `0`.

  We need to display a toast to show the result of these memory operations.
