# Lab 8: HTTP Connection II, Handler, and Menu

## HTTP Connection II

`HttpClient` is another class that works similarly to `HttpURLConnection`. It can be used to communicate with a web server. The following example shows how to make a POST request to the web server using `HttpClient` and related classes.

```java
HttpClient h = new DefaultHttpClient();
HttpPost p = new HttpPost(url);

List<NameValuePair> values = new ArrayList<NameValuePair>();
values.add(new BasicNameValuePair("param1", u));
values.add(new BasicNameValuePair("param2", v));
try {
    p.setEntity(new UrlEncodedFormEntity(values));
    HttpResponse response = h.execute(p);
    BufferedReader reader = new BufferedReader(
            new InputStreamReader(response.getEntity().getContent()));
    while((line = reader.readLine()) != null) {
        buffer.append(line);
    }
} catch (UnsupportedEncodingException e) {
    Log.e("Error", "Invalid encoding");
} catch (ClientProtocolException e) {
    Log.e("Error", "Error in posting a message");
} catch (IOException e) {
    Log.e("Error", "I/O Exception");
}
```

Here, we use `HttpClient` to make a connection to a web server specified by `url`. Then, we create an object of class `HttpPost` for making a POST request. Two values are attached to this request i.e. `param1` and `param2`. After that, we create a `BufferedReader` to obtain the response from the server.

As stated before, every HTTP connection is a time-consuming task. We need to create an `AsyncTask` to perform it.


## Handler

`Handler` is a class in Android SDK that allows us to schedule tasks. Since it works in the UI thread, we should not use `Handler` to perform any time-consuming task. However, we may use it to start an `AsyncTask`.

After creating an instance of `Handler`, we can use a `postDelayed` method to schedule a task after a specified number of milliseconds. `Handler` needs an object implementing the `Runnable` interface to specify the task to be performed. This can simply done by making the activity implement the `Runnable` interface and providing the `run` method.

```java
public class MainActivity extends ActionBarActivity implements Runnable {
  Handler h;

  protected void onCreate(Bundle savedInstanceState) {
    ...
    h = new Handler();
    // Execute the run method after 30000 milliseconds = 30 seconds
    h.postDelayed(this, 30000);
    ...
  }

  public void run() {
    Toast t = Toast.makeText(this.getApplicationContext(),
                "Called by handler", Toast.LENGTH_SHORT);
    t.show();
    h.postDelayed(this, 30000); //execute again after another 30 seconds
  }

}
```

## Menu

Every activity comes with a menu icon at the top-right corner. We can create a menu item that allows the user to perform some jobs.

To create a new menu item, we need to edit a menu xml file in `res/menu`. Each activity is created with a menu xml file. For example, the menu file for the `MainActivity` is `menu_main.xml`. Here is an example of the menu xml file. We create a menu item named **Refresh** with ID `action_refresh`.

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context="th.ac.tu.siit.its333.lab8exercise1.MessageActivity">

    <item android:id="@+id/action_refresh" android:title="Refresh"
        android:orderInCategory="100" app:showAsAction="never" />
</menu>
```

We can provide a method to handle this menu item by editing `onOptionsItemSelected` method, e.g.

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();

    if (id == R.id.action_refresh) {
        // Perform some task here
        return true;
    }

    return super.onOptionsItemSelected(item);
}
```


## Exercise

Fork and clone `its333y2014/Lab8Exercise1`. Then, implement the following features to make it become a microblogging application:

1. Implement the `doInBackground` method of `LoadMessageTask` to load and update the new message from the microblogging service. When we make a GET request to `http://ict.siit.tu.ac.th/~cholwich/microblog/fetch.php?time=0` where `time` specifies that we want to retrieve the message with the timestamp newer than the `time` parameter. The microblogging service basically returns a JSON object including the messages. Here is an example of the returned JSON object

  ```javascript  
  {
    "response":true,
    "errmsg":"",
    "timestamp":"1427012052",
    "msg":[{"user":"cholwich","message":"Hello, world!","time":"1427011845"},
           {"user":"cholwich","message":"This is a very simple microblogging
           service","time":"1427012052"}
          ]
  }
  ```

  Here, `response` is `true` when there is no error, `errmsg` is the error message when `response` is `false`, `timestamp` is the timestamp of the latest message, and `msg` is a JSON array of messages. Each message composes of `user`, `message`, and `timestamp`.

  The application should save the `timestamp` so that it can fetch only the newer messages later.

  After loading and parsing the messages, we need to add it to the `data` list. This list is connected to the `ListView` to display the messages.

2. Implement the `doInBackground` method of `PostMessageTask` to create a new status. Here, we need to make a POST request to `http://ict.siit.tu.ac.th/~cholwich/microblog/post.php` and passing to parameters i.e. `user` and `message`. Then, the server will response with a JSON object e.g.

  ```javascript
  {
    "response":true,
    "errmsg":""
  }
  ```

  Similar to the previous request, `response` is `true` when there is no error in the message posting.

3. Implement the `run` method to make the application check for the new messages every 30 seconds.

4. Implement the `onOptionsItemSelected` to allow the user to manually refresh the messages. However, selecting this menu item must postpone the update performed by the `Handler`. We can use `handler.removeCallbacks(this)` to cancel the scheduled tasks. 
