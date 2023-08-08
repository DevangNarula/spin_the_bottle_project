# spin_the_bottle_project
spin the bottle project for truth and dare game
**Create a new Android project and add necessary permissions in the AndroidManifest.xml file.**

**Create an XML layout for your main activity's UI (activity_main.xml):**
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/bottleImageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/bottle_image"
        android:layout_centerInParent="true" />

    <Button
        android:id="@+id/spinButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Spin"
        android:layout_below="@id/bottleImageView"
        android:layout_centerHorizontal="true" />
</RelativeLayout>

**In your MainActivity.java:**

  import android.os.Bundle;
  import android.view.View;
  import android.view.animation.Animation;
  import android.view.animation.RotateAnimation;
  import android.widget.Button;
  import android.widget.ImageView;

public class MainActivity extends AppCompatActivity {

    private ImageView bottleImageView;
    private Button spinButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        bottleImageView = findViewById(R.id.bottleImageView);
        spinButton = findViewById(R.id.spinButton);

        spinButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                spinBottle();
            }
        });
    }

    private void spinBottle() {
        float pivotX = bottleImageView.getWidth() / 2;
        float pivotY = bottleImageView.getHeight() / 2;

        RotateAnimation rotateAnimation = new RotateAnimation(0, 720,
                Animation.RELATIVE_TO_SELF, 0.5f,
                Animation.RELATIVE_TO_SELF, 0.5f);

        rotateAnimation.setDuration(2000);
        bottleImageView.startAnimation(rotateAnimation);
    }
}

**note: Make sure you have a bottle_image.png image in your res/drawable folder.**
 It includes a button that, when clicked, spins the bottle using a rotation animation.

  **additional features:**
  
  import android.media.MediaPlayer;
import android.os.Bundle;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.RotateAnimation;
import android.widget.Button;
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;
import java.util.Random;

public class MainActivity extends AppCompatActivity {

    private ImageView bottleImageView;
    private Button spinButton, resetButton;
    private Random random = new Random();
    private int lastAngle = 0;
    private boolean isSpinning = false;
    private MediaPlayer spinSound, stopSound;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        bottleImageView = findViewById(R.id.bottleImageView);
        spinButton = findViewById(R.id.spinButton);
        resetButton = findViewById(R.id.resetButton);

        spinSound = MediaPlayer.create(this, R.raw.spin_sound);
        stopSound = MediaPlayer.create(this, R.raw.stop_sound);

        spinButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (!isSpinning) {
                    spinBottle();
                }
            }
        });

        resetButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                resetBottle();
            }
        });
    }

    private void spinBottle() {
        isSpinning = true;

        spinSound.start();

        int randomAngle = random.nextInt(360) + 720; // Rotate at least 2 full rounds
        RotateAnimation rotateAnimation = new RotateAnimation(lastAngle, randomAngle,
                Animation.RELATIVE_TO_SELF, 0.5f,
                Animation.RELATIVE_TO_SELF, 0.5f);

        rotateAnimation.setDuration(2000);
        rotateAnimation.setFillAfter(true);
        bottleImageView.startAnimation(rotateAnimation);
        lastAngle = randomAngle;

        rotateAnimation.setAnimationListener(new Animation.AnimationListener() {
            @Override
            public void onAnimationStart(Animation animation) {
                isSpinning = true;
            }

            @Override
            public void onAnimationEnd(Animation animation) {
                isSpinning = false;
                stopSound.start();
                showResult(); // Display the result of the spin
            }

            @Override
            public void onAnimationRepeat(Animation animation) {
            }
        });
    }

    private void showResult() {
        // Implement logic to display the result of the spin
        // For example, change a TextView's text to show the result
    }

    private void resetBottle() {
        bottleImageView.setRotation(0);
        lastAngle = 0;
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        spinSound.release();
        stopSound.release();
    }
}

//add appropriate audio files (spin_sound.mp3 and stop_sound.mp3) to the res/raw folder.
You can replace the showResult() method with your logic to determine what the bottle points to and display the result accordingly.
This extended version of the app should provide a more engaging experience for users.//
