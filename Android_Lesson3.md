# Lesson 3: Intents

This is a nice application that we've created in Lesson 2, but it's a bit bland. It only has one page (`Activity`) and we know that any app worth anything has at least 2 pages...

Creating a new `Acitivty` is easy as 1-2-3
![Imgur](https://i.imgur.com/pmIeiCQ.png)

I named my new one `SecondActivity`.
![Imgur](https://i.imgur.com/Rt1JbCt.png)
> Android Studio will auto-generate a Layout XML file for us, with a very basic layout in there. This is great because the IDE organizes and gets us bootstrapped pretty quickly. The backwards compatibility option is optional.

Replace `activity_second.xml` with this:
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_second"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    tools:context="com.lessons.jtj.lessonapplication.SecondActivity">

    <TextView 
        android:id="@+id/secondText"
        android:layout_width="match_parent" 
        android:layout_height="match_parent"
        android:text="YOU REACHED THE SECOND ACTIVITY!"
        android:textSize="50dp"
    />

</RelativeLayout>
```
Go ahead and create a member variable for `secondText` id inside of `SecondActivity.java` like we did in Lesson 2.

Let's go back to `MainActivity.java`. Let's set it up where when we click on the `Button` object, it will take us to the `SecondActivity`

For this, we use the `Intent` class. `Intent`s specify...well, intents that we have for the app to do. In this case, we "intend" to go to the next `Activity` in our application.

```
@Override
    public void onClick(View v) {
        numTimesClicked++;
        mTextView.setText("You clicked the button " + numTimesClicked + " times!");
        
        Intent intent = new Intent(getApplicationContext(), SecondActivity.class);
    }
```
> Intent objects takes two parameters: a `Context` object, and a `Class` object. In our case, `Context` will always be `getApplicationContext()`, and the `Class` object will be the Activity that we want to route to. In our case, we want to route to `SecondActivity`. 

We can go to the next `Activity` with `startActivity(Intent)` function!
```
Intent intent = new Intent(getApplicationContext(), SecondActivity.class);
startActivity(intent);
```
![Imgur](https://i.imgur.com/PqNr2Ez.png)
Look back to the `Activity Lifecycle` from Lesson 1:
![Imgur](https://i.imgur.com/HuDvnYV.png)
> When we switched to the `SecondActivity`, the `MainActivity` was moved to the `Paused/Stop` state (since another activity (`SecondActivity`) came into the foreground. When we go back, we go to the `Resumed` state, or if the app is killed due to memory restraint, it will be `Killed`.

You can also send information between `Activity`s. Let's send the number of times we clicked the button in `MainActivity` to the `SecondActivity`

We do this with `putExtra` function inside of the `Intent` object.
![Imgur](https://i.imgur.com/lcT47HH.png)
The IDE shows us how many different things we can send to an intent (many more than is listed). In our case, we are going to send an `int` with the name `num_clicked`.
> We can name it anything, but when we access it inside of `SecondActivity`, we must access it with the same `key` that we specify when we put it inside the `Intent` extras!

Inside of `SecondActivity`, we can access the `Intent` we sent from `MainActivity` using the `getIntent()` function, and then retrieve the `Extras` that we sent and use them as we please.

```
public class SecondActivity extends AppCompatActivity {

    private TextView mSecondTextView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        mSecondTextView = (TextView) findViewById(R.id.secondText);

        Intent receivedIntent = getIntent();
        
        // the second parameter -1 is a default value that is assigned in case we can't find 'num_clicked'
        int numTimesClicked = receivedIntent.getIntExtra("num_clicked", -1);

        if (numTimesClicked == -1) {
            int redColor = ContextCompat.getColor(getApplicationContext(), android.R.color.holo_red_dark);
            mSecondTextView.setTextColor(redColor);
            mSecondTextView.setText("Uh oh! Something went wrong!");
        } else {
            int blueColor = ContextCompat.getColor(getApplicationContext(), android.R.color.holo_blue_dark);
            mSecondTextView.setTextColor(blueColor);
            mSecondTextView.setText("You clicked the button " + numTimesClicked + " times!!");
        }
    }
}
```
I added some pizzazz to `SecondActivity`. You will see when you run this code, the number of times we clicked this button will be shown on the second activity!

> Try it! Misspell the `key` inside of `SecondActivity` when we try to get the int value, and see what happens! I'm sure you can follow the code!

That's the basics of intents!



