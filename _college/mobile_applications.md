---
title: "Mobile Applications Formal Element"
excerpt: "This is the complete report I submitted for my Mobile Applications module in DT080B"
---

# Introduction

The purpose of this formal element was to design an application for Android Devices that meets the following requirements:

- The user will use the app to take a photo of the room
- The user will then be able to superimpose a series of straight lines with double ended arrows onto the image
- The lines will be drawn using finger touch
- The user will also be able to add dimensions to each line

 In this report, I will outline how I have achieved this by describing the background design of the application, followed by a description of the code.
 
 
# Table of Contents

* TOC
{:toc}


# Background

The application will be designed in Android using Android Studio. This is an IDE maintained by Google specifically for the development of Android Apps.

It will consist of two activities; an activity represents a &quot;screen&quot; in an Android application. Activities can be swapped between and pass messages between each other using intents. In this application, the main\_activity activity will use an intent to take a picture of an image, it will pass in a URI object into this intent, which is where the photo will be saved. It will then start another activity with the URI passed to it, draw\_lines. Once draw\_lines is finished, the imageView will be updated with the new image with lines drawn on it.

The main activity consists of a textView (an object in Android used to display text), a button (used to execute code on press) and an ImageView (used to display an image on screen). This activity will, on button press, start an intent for any application that can take photos, it will pass in the URI object (a location to store the photo at) as an extra; An intent is a way to start other activities in Android and pass data between them, an extra is how these messages are stored, accessed by using keys, which is a string used to identify the extra.

This activity will then start another intent for draw\_lines, it will pass in a URI again which contains a link to the image. Once draw\_lines has passed back in the URI after it is finished drawing, main\_activity will then display the new image with lines.

The second activity, draw\_lines, will display an image using an ImageView, which will then be drawable using a Canvas and Paint object. It will also feature an EditText field, for user-entered dimensions, and a button, for saving the image and exiting back to main\_activity, returning the URI of the new image with it.

The main\_activity class will also implement the OnClickListener interface, this means it must implement the onClick(View v) method. This is used to determine when an element on the screen is clicked on by the user. For the main\_activity we want this on the &quot;Launch Camera&quot; button. Inside onClick we then have an if statement checking if it was pressed. If it was, we execute the code inside.

The draw\_lines class implements the OnClickListener interfaces as well as the OnTouchListener, meaning it must implement onClick(View v) and onTouch(View v). onClick(View v) functions similarly to main\_activity, in that is used to determine when a button is pressed. Then an  onTouch(View v) method is implemented. This is used to check when the user touches the screen and then removes his finger from the screen. In this class, we get the coordinates of when the user touches the screen, when he removes his finger we get the coordinates for that also. Then we draw the line between these two coordinates, draw the text from the EditText field in the middle of this line and finally add the arrowheads to it.

The main\_activity class will also request permissions, which is required for Android Marshmallow and above, it does this by calling a request\_permissions() method, which uses Android&#39;s built in methods to determine if we have permission to save images or not. If we don&#39;t it will prompt the user to grant the app permissions.

The layout will be constructed via XML, which is the language Android uses for displaying elements on the screen. Using this I will design the two layouts of the activities to be simple and understandable.

# Flow of the Application
Figure 1, flowchart of app

![flow](/assets/images/college/flow.png){:class="img-responsive"}

# Overview
![app1](/assets/images/college/app1.png){:class="img-responsive" .align-center}

The main activity is the screen viewable upon opening the app, 
it features an imageview, along with a button to launch the camera Intent.

![app2](/assets/images/college/app2.png){:class="img-responsive" .align-center}

Once the photo is captured, the draw\_lines class is started, with the image displayed in the imageview.

![app3](/assets/images/college/app3.png){:class="img-responsive" .align-center}

The user is then able to input the dimensions in the field at the bottom, which will be applied to any line drawn.

![app4](/assets/images/college/app4.png){:class="img-responsive" .align-center}

Once the user has hit the finished button, this image is then saved, 
the main\_acitivity screen is brought up again with the new image displayed.

# Main Activity

## XML

The code for this layout is as follows:
    
    {% highlight xml %}
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="me.sdunbar.cameradistance.MainActivity">

    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/intro"
        android:layout_margin="3dp"
        android:textStyle="bold"
        android:id="@+id/textView"/>

    <Button
        android:id="@+id/capture_btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/capture"
        android:layout_below="@+id/textView"
        android:layout_alignLeft="@+id/textView"
        android:layout_alignStart="@+id/textView" />

    <ImageView
        android:id="@+id/picture"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:minHeight="300dp"
        android:minWidth="300dp"
        android:contentDescription="@string/picture"
        android:background="@drawable/pic_border"
        android:layout_below="@+id/capture_btn"
        android:layout_alignLeft="@+id/capture_btn"
        android:layout_alignStart="@+id/capture_btn"
        android:scaleType="fitXY"/>

    </RelativeLayout>
    {% endhighlight %}


## Imports

    {% highlight java %}
    import android.Manifest;
    import android.content.pm.PackageManager;
    import android.graphics.BitmapFactory;
    import android.os.Environment;
    import android.support.v4.app.ActivityCompat;
    import android.support.v4.content.ContextCompat;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.content.ActivityNotFoundException;
    import android.content.Intent;
    import android.graphics.Bitmap;
    import android.net.Uri;
    import android.provider.MediaStore;
    import android.view.View;
    import android.view.View.OnClickListener;
    import android.widget.Button;
    import android.widget.ImageView;
    import android.widget.Toast;
    import java.io.File;
    import java.io.FileNotFoundException;
    import java.io.InputStream;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    {% endhighlight %}


These imports allow the use of various methods within the class.

## Main Class
    
    {% highlight java %}
    public class MainActivity extends AppCompatActivity implements OnClickListener {
    {% endhighlight %}

This line defines the start of the block of code making up the class MainActivity, we also implement the OnClickListener interface to allow us to use the button.

    {% highlight java %}
    final int CAMERA_CAPTURE = 1;
    public Uri picUri;
    int APP_REQUEST = 69;
    static final String EXTRA_MESSAGE  =  "me.sdunbar.intenttest.myKey";
    Bitmap thePic;
    ImageView mImageView;
    {% endhighlight %}


These variable declarations are done at the top, to be able to use them throughout the class.

### OnCreate()

    {% highlight java %}
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
      //retrieve a reference to the UI button
      Button captureBtn = (Button)findViewById(R.id.capture_btn);
      //handle button clicks
      captureBtn.setOnClickListener(this);
      mImageView = (ImageView)findViewById(R.id.picture);

      request_permissions();
    }
    {% endhighlight %}


This instantiates the button and imageview, sets the onClickListener for the button, as well as starts the request\_permissions() method, which is used to allow us to save to external storage.

### Request\_permissions()

    {% highlight java %}
    private void request_permissions(){
        if (ContextCompat.checkSelfPermission(this,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
                != PackageManager.PERMISSION_GRANTED) {
            if (ActivityCompat.shouldShowRequestPermissionRationale(this,
                    Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
            } else {
                ActivityCompat.requestPermissions(this,
                        new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                        2);
            }
        }
    }
    {% endhighlight %}

This block of code was available from Android&#39;s official documentation, it first checks if we have permissions to write to external storage, if not, it prompts the user for permission.

### onClick()

    {% highlight java %}
    public void onClick(View v) {
      if (v.getId() == R.id.capture_btn) {
          try {
              //use standard intent to capture an image
              Intent captureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
              picUri = Uri.fromFile(getOutputMediaFile());
              captureIntent.putExtra(MediaStore.EXTRA_OUTPUT, picUri);

              //we will handle the returned data in onActivityResult
              startActivityForResult(captureIntent, CAMERA_CAPTURE);
          }
          catch(ActivityNotFoundException anfe){
              //display an error message
              String errorMessage = "Whoops - your device doesn't support capturing images!";
              Toast toast = Toast.makeText(this, errorMessage, Toast.LENGTH_SHORT);
              toast.show();
          }
      }
    }
    {% endhighlight %}


This is the onClick handler, if the capture button is pressed, we declare and instantiate an Intent called CaptureIntent, this will use ANY application on the phone that can respond to this request. We also pass in a URI, defined from getOutputMediaFile(), we add this as an extra into captureIntent so that the application will save the image at the URI given to it. We then start this intent with startActivityForResult and the request code CAMERA\_CAPTURE.

The error handling in the catch is for if no application can respond to the intent request. It will pop up a toast (sliding notification from the bottom of the screen) to alert the user that no camera app is available.

### getOutputMediaFile()

    {% highlight java %}
    private File getOutputMediaFile(){
        File mediaStorageDir = new File(Environment.getExternalStoragePublicDirectory(
               Environment.DIRECTORY_PICTURES), "CameraDemo");


        if (!mediaStorageDir.exists()){
            if (!mediaStorageDir.mkdirs()){
                return null;
            }
        }

        String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String imageFileName = timeStamp;

        return new File(mediaStorageDir.getPath() + File.separator +
                imageFileName + ".png");
    }
    {% endhighlight %}


This is used to determine where to store the saved file and what to call it, first we create a file by calling:

    {% highlight java %}
    Environment._getExternalStoragePublicDirectory_(Environment._DIRECTORY\_PICTURES_)
    {% endhighlight %}

This returns the location that Android has determined to be the public folder for storing pictures, the other variable &quot;CameraDemo&quot; allows us to create a folder called CameraDemo for storing our own images.

    {% highlight java %}
    if (!mediaStorageDir.exists()){
            if (!mediaStorageDir.mkdirs()){
                return null;
            }
        }
    }
    {% endhighlight %}


This allows us to check if the folder exists and to attempt to create it if it does not.

    {% highlight java %}
    String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String imageFileName = timeStamp;

        return new File(mediaStorageDir.getPath() + File.separator +
                imageFileName + ".png");
    {% endhighlight %}


This generates a timestamp of the current time and formats it. We then return a file object with the path, filename and the extension .png.

### onActivityResult()

    {% highlight java %}
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
      if (resultCode == RESULT_OK) {
          //user is returning from capturing an image using the camera
          if(requestCode == CAMERA_CAPTURE){

              try {
                  InputStream image_stream = getContentResolver().openInputStream(picUri);
                  thePic = BitmapFactory.decodeStream(image_stream );
              } catch (FileNotFoundException e) {
                  e.printStackTrace();
              }


              //retrieve a reference to the ImageView
              ImageView picView = (ImageView)findViewById(R.id.picture);
              //display the returned image
              picView.setImageBitmap(thePic);

              //start intent to draw lines
              Intent intent = new Intent(this, Draw_lines.class);
              intent.putExtra(EXTRA_MESSAGE, picUri.toString());
              startActivityForResult(intent, APP_REQUEST);
          }
          else if (requestCode == APP_REQUEST){
      Bundle extras = data.getExtras();
      Uri picUri = Uri.parse(extras.getString(EXTRA_MESSAGE));

      try {
          InputStream image_stream = getContentResolver().openInputStream(picUri);
          thePic = BitmapFactory.decodeStream(image_stream );
      } catch (FileNotFoundException e) {
          e.printStackTrace();
      }


      //retrieve a reference to the ImageView
      ImageView picView = (ImageView)findViewById(R.id.picture);
      //display the returned image
      picView.setImageBitmap(thePic);
    }
    {% endhighlight %}


In this block of code, we first check if the resultCode was RESULT\_OK, If it was, we check if the requestCode was CAMERA\_CAPTURE. If these are both true, we create an InputStream of the picUri and create a bitmap from it. Supposedly, creating it from an InputStream instead of decoding directly saves massively on memory usage, especially with images as big as the ones that will be taken. This block will print the stacktrace in console if it cannot find the file.

We then get a reference to the imageView and display the image on it.

After this, we create a new intent for the Draw\_lines class, we then hand it the URI in extras with the key EXTRA\_MESSAGE. Then, it is started for result with the request code APP\_REQUEST. Once the Draw\_lines class is finished, it returns the URI as a string, which is then parsed here and displayed on the imageview.

# Draw Lines class

## XML

The code for this layout is as follows:

    {% highlight xml %}
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <ImageView
        android:id="@+id/picture"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@+id/finBtn" />

    <Button
        android:text="Finished"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/finBtn"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentEnd="true"
        android:layout_marginRight="27dp"
        android:layout_marginEnd="27dp" />

    <EditText
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:inputType="text"
        android:text="Name"
        android:ems="10"
        android:layout_alignParentBottom="true"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_marginLeft="15dp"
        android:layout_marginStart="15dp"
        android:id="@+id/editText" />

    </RelativeLayout>
    {% endhighlight %}

## Main Class

    {% highlight java %}
    public class Draw_lines extends Activity implements OnTouchListener, View.OnClickListener {
        ImageView imageView;
        Canvas canvas;
        Paint paint, textPaint;
        float downx = 0, downy = 0, upx = 0, upy = 0;
        Intent intent;
        Bitmap thePic;
        Button capture;
        int[] viewCoords;
        Uri picUri;
        static final String EXTRA_MESSAGE  =  "me.sdunbar.intenttest.myKey";
        EditText et;
        float fixx, fixy;
        File file;
    {% endhighlight %}


Here, we also implemenet OnTouchListener, this allows us to check for finger presses on the screen instead of just if a button has been pressed, we also declare a number of variables at the start, much like main\_activity.



### onCreate()

    {% highlight java %}
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_draw_lines);

        imageView = (ImageView) this.findViewById(R.id.picture);
        et = (EditText) this.findViewById(R.id.editText);

        Display currentDisplay = getWindowManager().getDefaultDisplay();
        int dw = currentDisplay.getWidth();
        int dh = currentDisplay.getHeight();

        intent = getIntent();
        capture = (Button)findViewById(R.id.finBtn);

        Bundle extras = intent.getExtras();
        picUri = Uri.parse(extras.getString(EXTRA_MESSAGE));

        try {
            InputStream image_stream = getContentResolver().openInputStream(picUri);
            thePic = BitmapFactory.decodeStream(image_stream);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        thePic = getResizedBitmap(thePic, dw, dh);

        canvas = new Canvas();
        canvas.setBitmap(thePic);

        paint = new Paint();
        paint.setColor(Color.BLACK);
        paint.setStrokeWidth(7.0f);
        paint.setTextSize(20);

        textPaint = new Paint();
        textPaint.setColor(Color.WHITE);
        textPaint.setStrokeWidth(7.0f);
        textPaint.setTextSize(50);

        imageView.setImageBitmap(thePic);

        imageView.setOnTouchListener(this);
        capture.setOnClickListener(this);
    }
    {% endhighlight %}

Since this will be started as an intent, once we&#39;ve set the content view as usual and declared the imageView, editText, display etc. we begin parsing the Uri passed in. We do this by creating a Bundle called extras and then parsing the string that was stored with the key EXTRA\_MESSAGE.

With this key, we create an inputstream again to create the bitmap which we have named thePic. We then call the getResizedBitmap() method which will be explained below.

Then we create the canvas and call setBitmap(thePic), this allows us to draw on and modify it.

We then create two Paint() objects, one for the line and the other for text.

Finally we set the imageView to thePic, so as to see the image we are drawing on. Then we set the onClickListener and onTouchListener as required.

### getResizedBitmap()

    {% highlight java %}
    public Bitmap getResizedBitmap(Bitmap bm, int maxHeight, int maxWidth) {

        float scale = Math.min(((float)maxHeight / bm.getWidth()), ((float)maxWidth / bm.getHeight()));

        Matrix matrix = new Matrix();
        matrix.postScale(scale, scale);

        return Bitmap.createBitmap(bm, 0, 0, bm.getWidth(), bm.getHeight(), matrix, true);
    }
    {% endhighlight %}


This is the code used to scale the bitmap, which was created by user &quot;Kevin&quot; at [http://stackoverflow.com/questions/4837715/how-to-resize-a-bitmap-in-android](http://stackoverflow.com/questions/4837715/how-to-resize-a-bitmap-in-android)

I then modified his code somewhat, to pass in my own dimensions. It takes in a bitmap and a max height and width. It then creates a float representing the aspect ratio of the image, creating a Matrix object based on this, it then returns a new bitmap with this scaling applied.

### onTouch()

    {% highlight java %}
    public boolean onTouch(View v, MotionEvent event) {
        int action = event.getAction();
        switch (action) {
            case MotionEvent.ACTION_DOWN:
                downx = event.getX();
                downy = event.getY();
                break;
            case MotionEvent.ACTION_MOVE:
                break;
            case MotionEvent.ACTION_UP:
                upx = event.getX();
                upy = event.getY();
                canvas.drawLine(downx, downy, upx, upy, paint);
                fillArrow(canvas, downx, downy, upx, upy);

                float textx = (downx + upx) / 2;
                float texty = (downy + upy) / 2;

                canvas.drawText(et.getText().toString(), textx, texty, textPaint);
                imageView.invalidate();
                break;
            case MotionEvent.ACTION_CANCEL:
                break;
            default:
                break;
        }
        return true;
    }
    {% endhighlight %}


This code handles when we touch the imageview, when the finger is pressed down, we get the coordinates, when the finger is released we grab the coordinates for that also.

With these coordinates, we draw a line, and then call fillArrow() to add arrowheads to the ends of the line.

We then average the X and Y coordinates of the start and end. We call drawText with the text from the editText, along with the coordinates and the textPaint object. We then invalidate the imageView to refresh it.

### onClick()

    {% highlight java %}
    public void onClick(View v) {
        if (v.getId() == R.id.finBtn) {
            // Here, thisActivity is the current activity
            FileOutputStream out = null;
            String path = Environment.getExternalStorageDirectory().toString();
            try {
                File mediaStorageDir = new File(Environment.getExternalStoragePublicDirectory(
                        Environment.DIRECTORY_PICTURES), "CameraDemo");

                String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
                String imageFileName = timeStamp;

                file = new File(mediaStorageDir.getPath() + File.separator +
                        imageFileName + "_lines.png");

                out = new FileOutputStream(file);
                thePic.compress(Bitmap.CompressFormat.PNG, 100, out); // bmp is your Bitmap instance
                // PNG is a lossless format, the compression factor (100) is ignored
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                try {
                    if (out != null) {
                        out.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            Intent result = new Intent();

            picUri = Uri.parse(file.toURI().toString());
            result.putExtra(EXTRA_MESSAGE, picUri.toString());
            setResult(Activity.RESULT_OK, result);
            finish();
        }
    }
    {% endhighlight %}


This code handles the button press. First we create a FileOutputStream, then similarly to main\_acitivity, we determine the public pictures directory and use our own folder CameraDemo.

We then create a time stamp to use as the image name, exactly as we did in main\_activity, however, this time instead of appending .png, we append lines.png, to differentiate the two images.

We then &quot;compress&quot; the image and write it out to the file, since we are using png, it is not actually compressed and the quality factor of 100 has no impact.

Finally, we create an intent object, parse the Uri from file and store the string representation of this with the key EXTRA\_MESSAGE. We then setResult of RESULT\_OK and finish().

### fillArrow()

    {% highlight java %}
    private void fillArrow(Canvas canvas, float x0, float y0, float x1, float y1) {

      paint.setStyle(Paint.Style.FILL);

      float deltaX = x1 - x0;
      float deltaY = y1 - y0;
      double distance = Math.sqrt((deltaX * deltaX) + (deltaY * deltaY));
      float frac = (float) (1 / (distance / 30));

      float point_x_1 = x0 + ((1 - frac) * deltaX + frac * deltaY);
      float point_y_1 = y0 + ((1 - frac) * deltaY - frac * deltaX);

      float point_x_2 = x1;
      float point_y_2 = y1;

      float point_x_3 = x0 + ((1 - frac) * deltaX - frac * deltaY);
      float point_y_3 = y0 + ((1 - frac) * deltaY + frac * deltaX);

      float x_4 = x0 - deltaX + ((1 - frac) * deltaX + frac * deltaY);
      float y_4 = y0 - deltaY + ((1 - frac) * deltaY - frac * deltaX);
      float point_x_4 = x0 + (x0 - x_4);
      float point_y_4 = y0 + (y0 - y_4);

      float point_x_5 = x0;
      float point_y_5 = y0;

      float x_6 = x0 - deltaX + ((1 - frac) * deltaX - frac * deltaY);
      float y_6 = y0 - deltaY + ((1 - frac) * deltaY + frac * deltaX);
      float point_x_6 = x0 + (x0 - x_6);
      float point_y_6 = y0 + (y0 - y_6);

      Path path = new Path();
      Path path2 = new Path();
      path.setFillType(Path.FillType.EVEN_ODD);
      path2.setFillType(Path.FillType.EVEN_ODD);

      path.moveTo(point_x_1, point_y_1);
      path.lineTo(point_x_2, point_y_2);
      path.lineTo(point_x_3, point_y_3);
      path.close();

      path2.moveTo(point_x_6,point_y_6);
      path2.lineTo(point_x_5,point_y_5);
      path2.lineTo(point_x_4, point_y_4);
      path2.close();

      canvas.drawPath(path, paint);
      canvas.drawPath(path2, paint);
    }
    {% endhighlight %}


This is the code for fillArrow() which I have gotten from Pierre. It uses the start and end coordinates of the line to draw 2 arrowheads on both ends. Unfortunately, I am not familiar enough with the math involved to explain further how this works.

# Conclusion

This formal element was intended to give us the skills required to design an Android application that could open a camera, save the image and then draw lines on said image with dimensions. Through this report I believe I have adequately demonstrated how to construct such an application.
