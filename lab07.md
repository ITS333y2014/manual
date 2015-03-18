# Lab 7: AsyncTask and HttpURLConnection

## AsyncTask

`AsyncTask` is suitable for running a time-consuming task in background. Here, the time-consuming task will be executed in a new thread separated from the UI thread. This therefore will not freeze the user interface.

To use `AsyncTask`, we need to create a subclass of `AsyncTask`. This class needs three generic types i.e.

1. `Params` specifies the type of values passed to the task when it starts.

2. `Progress` is the type of the progress value.

3. `Result` is the type of the result of the task.

See https://github.com/its333y2014/Lab7Example1AsyncTask for an example on how to use `AsyncTask`.

## HTTP Connection

`HttpURLConnection` can be used to connect our application to a web server. In this lab, we study how to use `HttpURLConnection` to load information from a server and display to the user.

```java
URL u = new URL("http://www.siit.tu.ac.th");
HttpURLConnection h = (HttpURLConnection)u.openConnection();
h.setRequestMethod("GET");
h.setDoInput(true);
h.connect();

int response = h.getResponseCode();
if (response == 200) {
  BufferedReader reader = new BufferedReader(new InputStreamReader(h.getInputStream()));

  // Read the data from reader
}
```

Since downloading data from the server is a time-consuming task, it is recommended to use `AsyncTask` with `HttpURLConnection`.

We need to modify `AndroidManifest.xml` in `app/manifests` to specify that our application needs a connection to the Internet by adding

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

## JSON

JSON or JavaScript Object Notation is a text-based standard format for data exchange. JSON has become popular to exchange data between a client and a web server. The following source code shows a simple JSON format representing an object of two values i.e. firstName and lastName.

```javascript
{
  "firstName": "Cholwich",
  "lastName": "Nattee"
}
```

Each JSON object is enclosed in a pair of curly braces. The object composes of multiple attribute-value pairs. Each pair is a map from a property name to a value. Here, the value can be an integer, a floating-point number, a string, a JSON object, or a JSON array. The following example shows a more sophisticated JSON object.

```javascript
{
  "studentId": "5522771111",
  "studentName": {
    "firstName": "peter",
    "lastName": "pettigrew"
  },
  "grades": [
    {"course": "its336", "grade": 4.0},
    {"course": "its333", "grade": 3.5}
  ]
}
```

Note that a pair of square brackets are used to denote an array in JSON.

Android natively provides a number of methods to parse and manipulate JSON objects through the JSONObject and JSONArray classes. Here is an example.

```java
String s = "{" +
        "  \"studentId\": \"5522771111\"," +
        "  \"studentName\": {" +
        "    \"firstName\": \"peter\"," +
        "    \"lastName\": \"pettigrew\"" +
        "  }," +
        "  \"grades\": [" +
        "    {\"course\": \"its336\", \"grade\": 4.0}," +
        "    {\"course\": \"its333\", \"grade\": 3.5}" +
        "  ]" +
        "}";
try {
    JSONObject obj = new JSONObject(s);
    String sId = obj.getString("studentId");
    JSONObject sName = obj.getJSONObject("studentName");
    String firstName = sName.getString("firstName");
    JSONArray grades = obj.getJSONArray("grades");
    double its336Grade = grades.getJSONObject(0).getDouble("grade");

} catch (JSONException e) {
    //Show error
    e.printStackTrace();
}
```

## Exercise

Fork and clone `its333y2014/Lab7Exercise1`. Then, implement the following features:

1. The application displays all the predefined textviews, i.e. weather, temperature (including min and max), humidity, and wind speed.

2. The application allows the user to load weather information of Nonthaburi and Pathumthani when a corresponding button is clicked.

3. The application will not reload the data if the user selects the same province within 1 minutes (to help reduce the load of the server). **Hint**: use `System.currentTimeMillis()` to get the current timestamp.
