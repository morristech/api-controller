# api-controller
Run GET,POST and DOWNLOAD API on Android. Library takes care of Cookie Management and http caching.

```java
ApiController APIc = new ApiController(this);//Calling Library

//GET : parmetes need to be only string
String response  = APIc.GetRequest(String URL,HashMap<String,String> parameters);

//POST : parmeters can be a string or a file, see example below
String response  = APIc.PostRequest(String URL,HashMap<String,String> parameters);

//DOWNLOAD BACKGROUND
String destination  = APIc.download_file(String URL,String AbsoluteDestination);
//If you dont add the extension in AbsoluteDestination then extension will added based in MimeType of the downloaded file
//return the downloaded path, creates the folder if does not exist.

//DOWNLOAD WITH NOTIFICATION
String destination  = APIc.download_file_notify(String URL,String AbsoluteDestination);
//Shows progress in notification bar and then closes it on finishing
//return the downloaded path, creates the folder if does not exist.
```

ANDROID MANIFEST
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```


**GET**
```java
ApiController APIc = new ApiController(this);
new Thread( new Runnable() {
  @Override
  public void run() {
      //Network Operation
      HashMap<String,String> params = new HashMap<String, String>();
      params.put("get_request1", "1");
      params.put("get_request2", "2");
      params.put("get_request3", "3");
      final String result  = APIc.GetRequest("http://nashapp.in/request.php",params);
      new Handler(Looper.getMainLooper()).post(new Runnable() {
          @Override
          public void run() {
              //UI Operation using network data
              alertDialog("Result", result);
          }
      });
  }
}).start();
```

**POST**

You can post files and variables together.
When posting files give the value as 
```java
"file://"+file.getAbsolutePath()
```
You cannot POST multiple files with the same key as described in http://php.net/manual/en/features.file-upload.multiple.php
```java
ApiController APIc = new ApiController(this);
new Thread( new Runnable() {
  @Override
  public void run() {
      //Network Operation
      HashMap<String,String> params = new HashMap<String, String>();
      params.put("post_message1", "1");
      params.put("post_message2", "2");
      params.put("post_message3", "3");
      params.put("post_file", "file:///storage/emulated/0/Pictures/Screenshots/Screenshot_2016-04-01-22-13-25.png");
      params.put("post_file1", "file:///storage/emulated/0/Pictures/Screenshots/Screenshot_2016-04-01-22-13-25.png");
      params.put("post_message4", "4");
      params.put("post_message5", "5");
      params.put("post_message6", "6");
      final String result  = APIc.PostRequest("http://nashapp.in/request.php",params);
      new Handler(Looper.getMainLooper()).post(new Runnable() {
          @Override
          public void run() {
              //UI Operation using network data
              alertDialog("Result", result);
          }
      });
  }
}).start();
```

**DOWNLOAD BACKGROUND**
```java
ApiController APIc = new ApiController(this);
 new Thread( new Runnable() {
  @Override
  public void run() {
      final String result  = APIc.download_file("http://nashapp.in/socialsignin.png", Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS)+"/socialsignin.png");
      new Handler(Looper.getMainLooper()).post(new Runnable() {
          @Override
          public void run() {
             //UI Operation using network data
              alertDialog("Result", result);
          }
      });
  }
}).start();
```

**DOWNLOAD NOTIFY**
```java
ApiController APIc = new ApiController(this);
new Thread( new Runnable() {
  @Override
  public void run() {
      final String result  = APIc.download_file_notify("http://nashapp.in/test.txt", Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS) + "/test.txt");
      new Handler(Looper.getMainLooper()).post(new Runnable() {
          @Override
          public void run() {
              //UI Operation using network data
              alertDialog("Result", result);
          }
      });
  }
}).start();
```

You can use http://nashapp.in/request.php to verify validity of the request variables.

**ALERT DIALOG** if you need to verify values before parsing
```java
public void alertDialog(String title,String Message){
        AlertDialog alertDialog = new AlertDialog.Builder(this).create();
        alertDialog.setTitle(title);
        alertDialog.setMessage(Message);
        alertDialog.setButton(AlertDialog.BUTTON_NEUTRAL, "OK",
                new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                    }
                });
        alertDialog.show();
    }
```