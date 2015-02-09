# Cheat Sheet

1. Convert from string to integer
  ```java
  String s = "120";
  int i = Integer.valueOf(s);
  ```

1. Convert from string to double
  ```java
  String s = "3.14";
  double d = Double.valueOf(s);
  ```

1. Convert from integer to string
  ```java
  int i = 120;
  String s = Integer.toString(i);
  ```

1. Convert from double to string
  ```java
  double d = 3.14;
  String s = Double.toString(d);
  ```

1. String formatting
  ```java
  double d = 3.14159;
  int i = 255;
  String s = String.format("%.2f 0x%x", d, i);  // "3.14 0xff"
  ```
