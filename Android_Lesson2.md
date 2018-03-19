# Lesson 2: Our first simple app

Well, you made it this far, so obviously these lessons aren't boring you too much!

In Lesson 1, we created an application and ran it. It was kind of lame, I know. Let's fix it! (not really)

### Android Layouts (XML)
If you're familiar at all with HTML/CSS, you'll feel right at home here. If not, don't worry because XML is easier than HTML/CSS, so you should still feel right at home :)

Let's open up `activity_main.xml` inside of `app/res/layout`

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    tools:context="com.lessons.jtj.lessonapplication.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"/>
</RelativeLayout>
```
Right now, it's pretty simple. We have our `RelativeLayout` which has a `layout:width` and `layout:height` of `match_parent`, which in this case, the parent is the whole screen. There is some padding also added in there. 
### Layouts
Layouts (such as `RelativeLayout`, `LinearLayout`, etc. are containers/wrappers for `Views`, which are objects like `Buttons`, `ListViews`, `EditTexts` (text boxes), etc. They are like specially coded `div` if you are familiar with HTML at all. `divs` are like containers for multiple `views`, it's good for grouping and such. 

> Actually, `Layouts` are `Views` in and of themselves, but they are special `Views` that can contain other `Views`

> In code, I believe all Layouts, such as `RelativeLayout` and `LinearLayout` inherit from the base class `AbsoluteLayout`

### Views  
Inside of the `RelativeLayout`, is a simple `TextView`. It's job is to display text. `Views` come in all different shapes and sizes, and what we are really interested in:

> In code, all `Views`, such as `TextView` and `Button` inherit from the base class `View.java`

Let's begin by replacing `RelativeLayout` with a `LinearLayout`. A `RelativeLayout` allows you to place multiple `View`s inside, with their positioning in the layout **relative** to other `Views` inside of the same hierarchy.

For example:
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
        ...>
    <TextView
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@+id/text"
        android:text="RIGHT BUTTON!"/>
</RelativeLayout>
```
![Imgur](https://i.imgur.com/nSjGVWv.png)

I added a `Button` tag after the `TextView`. As you see, I assigned an `android:id` to the `TextView` with id of `text`. (Note the syntax for assigning an `id` to a `View`). 
>  When we assign `id` to our views inside of the XML, Android builds and quantifies these into integers that are accessible from the `R.java` class (R == resources). Resources are further broken down into categories, such as `layout`, `drawables` (icons, images, etc), and `assets` (text files, sound clips) and a few more as well. We'll utilize this class in Java code

When adding a `Button`, inside of a `RelativeLayout` I am able to place it in the layout relative to any other `View` inside the same hierarchy. In this case, I placed the `Button` to the right of (`toRightOf`) the `View` with the id of `@+id/text`. Obviously it doesn't look all that great, but we can fix that later with different layout qualifiers (we won't). 

There are other layout views, such as `LinearLayout`, my favorite one :) Let's replace our layout XML with this:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:orientation="vertical"
    tools:context="com.lessons.jtj.lessonapplication.MainActivity">

    <TextView
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="RIGHT BUTTON!"/>
</LinearLayout>
```
> Be careful to replace the `tools:context` tag with your `<package name>.MainActivity`

Big changes here: we replaced the `RelativeLayout` with the `LinearLayout`, which organizes `Views` automatically in horizontal or veritcal fashion (specified by the `android:orientation` tag inside of the `LinearLayout`.

Also, inside the `Button`, we removed the `layout:toRightOf` parameter (although Android is smart enough to ignore this inside if the `View` is inside a `LinearLayout`
![Imgur](https://i.imgur.com/2jkyVBC.png)
The button should probably not say "Right Button!" anymore :)

# Part 2: Layout -> Java Code
Let's jump back into Java to wire up the `Views` to some code, so we can interact with them! Let's go back to `MainActivity.java`

```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

We can add a `TextView` and `Button` objects as private, member variables of this class. 
> Any view that is meant to be interactable should be added as a member variable. The IDE should prompt for any necessary imports

```
public class MainActivity extends AppCompatActivity {

    private TextView mTextView;
    private Button mButton;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

We should initialize our `Views` when the activity is created! (in `onCreate()`, duh!)
```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    mTextView = (TextView) findViewById(R.id.text);
    mButton = (Button) findViewById(R.id.button);
}
```
> The inherited method `findViewById()` returns a View object. We need to `cast` these returned View objects as the view we want. 

We set our content view `setContentView` to the layout we plan to use for this `Activity` (using `R.layout`, so `findViewById()` will find resources based on `id`s that we assigned inside that Layout. Again, we utilize the `R` class to associate the view's `id` with it's `Object` counterpart (using `R.id`)

So, to recap, we now have our layout specified to contain a `TextView` and a `Button`, aligned vertically inside of a `LinearLayout`, and now we have instantiated our Java objects that represent the `TextView` and `Button`. Let's do some interactions!

Inside of `onCreate()`, we can also specify interactions, such as "what should we do when the user clicks the button?" Let's set up something simple that will change the text of the `TextView` whenever we click the `Button`

> There are a lot of `event listeners` in Android (or any UI-based programming language, really) that listen for various user events, such as clicks, scrolls, drags, taps, etc. We can set listeners to any `View` that will respond to a certain event and act upon it. It's self explanatory:

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    mTextView = (TextView) findViewById(R.id.text);
    mButton = (Button) findViewById(R.id.button);

    mButton.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            
        }
    });
}
```
(A lot of that code was auto-generated by the IDE, don't be intimidated by a lot of syntax)

So, we are now set up so that whenever we click on `mButton`, it will run the code inside of `onClick`!

```
mButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        mTextView.setText("You clicked the button!");
    }
});
```
![Imgur](https://i.imgur.com/cQANgUe.png)

Here's a code snippet that adds more life to the application

```
public class MainActivity extends AppCompatActivity {

    private TextView mTextView;
    private Button mButton;
    private int numTimesClicked = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mTextView = (TextView) findViewById(R.id.text);
        mButton = (Button) findViewById(R.id.button);

        mButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                numTimesClicked++;
                mTextView.setText("You clicked the button " + numTimesClicked + " times!");
            }
        });
    }
}
```
![Imgur](https://i.imgur.com/RPU5rDB.png)
 
 







