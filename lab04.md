# Lab 4: ListView and SQLite Database

We study how to create an Android application that stores data in a database.

## Database Helper

To store our data in an SQLite database, we first need to create a *database helper* to manage a connection between our activities and the database engine. This database helper is a subclass of `SQLiteDBOpenHelper`. We then need to override the following methods:

1. `onCreate` is called once to create a database file and all related tables.

2. `onUpgrade` is called once when the version of the database helper is greater than that of the database file.

**Note** To create a new class, right-click on the package folder under the **java** fold, and select **New** and **Java Class**.

The following example shows a database helper for managing to-do list.

```java
package th.ac.tu.siit.its333.lab4example;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class ToDoDBHelper extends SQLiteOpenHelper {

    // Database file name
    public static final String name = "todo.sqlite3";
    // Database version
    public static final int version = 1;

    public ToDoDBHelper(Context context) {
        super(context, name, null, version);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        // This is called when the database is still NOT available
        // The primary key of the table must be "_id".
        String sql = "CREATE TABLE todo (" +
                "_id integer primary key autoincrement, " +
                "title text not null," +
                "priority integer default 0);";
        db.execSQL(sql);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        // This is called when the version defined in this class is
        // different from the version in the database file
        String sql = "DROP TABLE IF EXISTS todo;";
        db.execSQL(sql);
        this.onCreate(db);
    }
}
```

## Manipulating Data in the Database

### Retrieving data

After creating the database helper, we get the readable database from the helper using `getReadableDatabase`. Then, we can use `rawQuery` method to retrieve records from the database by specifying a SELECT statement.

```java
ToDoDBHelper helper = new ToDoDBHelper(this);
SQLiteDatabase db = helper.getReadableDatabase();
Cursor cursor = db.rawQuery("SELECT * FROM todo ORDER BY priority DESC;", null);
cursor.moveToFirst(); // get the first row
String title = cursor.getString(0); // get the first column
```

### Inserting a new record

To manage the data in the database, we need the writable database from the helper by calling `getWritableDatabase`. We can then use `insert` method to insert a new record to a table as shown below.

```java
SQLiteDatabase db = helper.getWritableDatabase();
ContentValues r = new ContentValues();
r.put("title", "AI homework");
r.put("priority", 3);
long new_id = db.insert("todo", null, r);
```

We use an instance of `ContentValues` class to pack all the values for the `insert` method.  

### Updating a record

We use `update` method to update records specified by a condition. The updated values need to be packed in an instance of `ContentValues`. We can specify the conditions and the condition values for the update.

```java
SQLiteDatabase db = helper.getWritableDatabase();
ContentValues r = new ContentValues();
r.put("priority", 2);
int n_rows = db.update("todo",  // table name
        r,                      // a record
        "_id = ?",              // conditions
        new String[] { "1" }    // values for the conditions
);
```

The `update` method returns the number of updated records.

### Deleting a record

We use `delete` method to remove a specified record from the database table as shown below.

```java
SQLiteDatabase db = helper.getWritableDatabase();
int n_rows = db.delete("todo", "_id = ?", new String[]{ "1" });
```

The `delete` method also returns the number of deleted records.

## ListView

`ListView` is a widget for displaying a list of items. We can use it to display the records obtained from a database table.

Each `ListView` uses an adapter to manage items to be displayed. Different types of adapters are provided in the Android SDK. We use `SimpleCursorAdapter` to display a list of records in a database table.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    ToDoDBHelper helper = new ToDoDBHelper(this);
    SQLiteDatabase db = helper.getReadableDatabase();
    Cursor cursor = db.rawQuery("SELECT * FROM todo ORDER BY priority DESC;", null);

    SimpleCursorAdapter adapter = new SimpleCursorAdapter(this,
            android.R.layout.simple_list_item_1, // A textview
            cursor, // cursor to a data collection
            new String[] {"title"}, // column to be displayed
            new int[] {android.R.id.text1}, // ID of textview to display
            0);

    ListView lv = (ListView)findViewById(R.id.listView);
    lv.setAdapter(adapter);
}
```

## Handling Item Clicks

We can use `setOnItemClickListener` and `setOnItemLongClickListener` method to specify callback functions when the user clicks or long-clicks on one of the items displayed in the listview.

To make it simple, we can make the activity class implement both `OnItemClickListener` and `OnItemLongClickListener`.

```java
public class MainActivity extends ActionBarActivity
        implements AdapterView.OnItemClickListener,
                   AdapterView.OnItemLongClickListener {

    ToDoDBHelper helper;
    SimpleCursorAdapter adapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        helper = new ToDoDBHelper(this);
        SQLiteDatabase db = helper.getReadableDatabase();
        Cursor cursor = db.rawQuery(
                "SELECT * FROM todo ORDER BY priority DESC;",
                null);

        adapter = new SimpleCursorAdapter(this,
                android.R.layout.simple_list_item_1,
                cursor,
                new String[] {"title"},
                new int[] {android.R.id.text1},
                0);

        ListView lv = (ListView)findViewById(R.id.listView);
        lv.setAdapter(adapter);
        lv.setOnItemClickListener(this);
        lv.setOnItemLongClickListener(this);
    }

    @Override
    public void onItemClick(AdapterView<?> parent, View view,
                            int position, long id) {
        Log.d("todo", id + " is clicked");
    }

    @Override
    public boolean onItemLongClick(AdapterView<?> parent, View view,
                                   int position, long id) {
        SQLiteDatabase db = helper.getWritableDatabase();

        int n = db.delete("todo",
                "_id = ?",
                new String[]{Long.toString(id)});

        if (n == 1) {
            Toast t = Toast.makeText(this.getApplicationContext(),
                    "Successfully deleted the selected item.",
                    Toast.LENGTH_SHORT);
            t.show();

            // retrieve a new collection of records
            Cursor cursor = db.rawQuery(
                    "SELECT * FROM todo ORDER BY priority DESC;",
                    null);

            // update the adapter
            adapter.changeCursor(cursor);
        }
        db.close();
        return true;
    }

    ...
}
```

To update the listview after deleting a record, we create a new cursor and use `changeCursor` method to update the listview.

## Exercise



1.
