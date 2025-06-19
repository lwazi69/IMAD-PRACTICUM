# IMAD-PRACTICUM
ST10447204
Lwazi Zondo 

main activity.kt
package com.example.imadpracticum

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.ArrayAdapter
import android.widget.Button
import android.widget.ListView
import android.widget.TextView
import android.widget.Toast
import android.content.Intent

//arrays for the song data
private val songTitles = arrayOf("Magnolia","Nomalizo","Bye Bye","Do I Need Ya")
  private val artistNames = arrayOf("Playboi Carti","Brown Dash","NoName","Freddy")
  private val ratings = arrayOf(4,5,3,5)
  private val comments = arrayOf("Trap","Kwaito","R&B","Rap")

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
//ui components
      val songListView:ListView = findViewById(R.id.songListView)
      val btnAverage:Button = findViewById<Button>(R.id.btnAverage)
        val txtAverage: TextView = findViewById(R.id.txtAverage)
//setting up songs in listview
        val adapter = ArrayAdapter(this,android.R.layout.simple_list_item_1, songTitles)
        songListView.adapter = adapter
//click to open details acctivity
        songListView.setOnItemClickListener { _ , _ , position , _ ->
            val intent = Intent(this , DetailsActivity::class.java)
//passing data using intent extras
            intent.putExtra("title" , songTitles[position])
            intent.putExtra("artist" , artistNames[position])
            intent.putExtra("rating" , ratings[position])
            intent.putExtra("comment" , comments[position])

            startActivity(intent)

        }
  // Calculate and display average rating when button is clicked
        btnAverage.setOnClickListener {
            try {
                var total = 0
                for (rating in ratings) {
                    if (rating !in 1..5) {
                        Toast.makeText(this , "Error:Invalid rating found!" , Toast.LENGTH_LONG).show()
                        return@setOnClickListener
                    }
                    total += rating
                }


            val average = total.toDouble() / ratings.size
            txtAverage.text = "Average Rating: %2f".format(average)

        }catch (e: Exception){
            Toast.makeText(this,"Error calculating average: ${e.message}",Toast.LENGTH_LONG).show()


            }
        }
    }
}

Activity main.xml 

?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- Background Image -->
    <ImageView
        android:id="@+id/mainBackground"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="centerCrop"
        android:src="@drawable/vinyl"
        android:contentDescription="Background"
        android:alpha="0.5" /> <!-- faded background for readability -->

    <!-- Foreground Overlay Layout -->
    <LinearLayout
        android:orientation="vertical"
        android:padding="24dp"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#DD000000"
    android:layout_alignParentTop="true"
    android:layout_alignParentStart="true">

    <!-- Title -->
    <TextView
        android:id="@+id/mainTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="🎵 My Playlist"
        android:textSize="24sp"
        android:textStyle="bold"
        android:textColor="#FFFFFF"
        android:gravity="center"
        android:layout_marginBottom="16dp" />

    <!-- Song List -->
    <ListView
        android:textColor="#FFFFFF"
        android:id="@+id/songListView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:divider="#FFFFFF"
        android:dividerHeight="1dp"
        android:background="@android:color/transparent" />

    <!-- Average Button -->
    <Button
        android:id="@+id/btnAverage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Calculate Average Rating"
        android:textColor="#FFFFFF"
        android:layout_marginTop="16dp"
        android:layout_marginBottom="8dp" />

    <!-- Average Result -->
    <TextView
        android:id="@+id/txtAverage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Average Rating:"
        android:textSize="16sp"
        android:textColor="#FFFFFF"
        android:gravity="center" />
</LinearLayout>
    </RelativeLayout>

    Details Activity.kt

    package com.example.musicplaylist

import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class DetailsActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_details)

        // Find UI components
        val titleText: TextView = findViewById(R.id.detailSongTitle)
        val artistText: TextView = findViewById(R.id.detailArtist)
        val ratingText: TextView = findViewById(R.id.detailRating)
        val commentText: TextView = findViewById(R.id.detailComment)
        val backButton: Button = findViewById(R.id.btnBack)

        try {
            // Get data passed from MainActivity
            val songTitle = intent.getStringExtra("title")
            val artistName = intent.getStringExtra("artist")
            val rating = intent.getIntExtra("rating", -1)
            val comment = intent.getStringExtra("comment")

            // Display song data
            titleText.text = "Title: $songTitle"
            artistText.text = "Artist: $artistName"
            commentText.text = "Comment: $comment"

            // Error check on rating
            if (rating in 1..5) {
                ratingText.text = "Rating: $rating / 5"
            } else {
                ratingText.text = "Rating: Invalid"
                Toast.makeText(this, "Rating must be between 1 and 5.", Toast.LENGTH_LONG).show()
            }

        } catch (e: Exception) {
            Toast.makeText(this, "Something went wrong: ${e.message}", Toast.LENGTH_LONG).show()
        }

        // Back button returns to MainActivity
        backButton.setOnClickListener {
            finish()
        }
    }
}

DETAILS activity.xml

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- Background Image -->
    <ImageView
        android:id="@+id/detailsBackground"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="centerCrop"
        android:src="@drawable/vinyl"
        android:contentDescription="Background"
        android:alpha="0.5" /> <!-- slightly faded background -->

    <!-- Foreground Content with Opaque Background -->
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:background="#DD000000"
        android:padding="24dp"
    android:layout_margin="20dp"
    android:elevation="4dp"
    android:gravity="center">

    <!-- Song Title -->
    <TextView
        android:id="@+id/detailSongTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Title:"
        android:textSize="22sp"
        android:textColor="#FFFFFF"
        android:textStyle="bold"
        android:layout_marginBottom="12dp" />

    <!-- Artist -->
    <TextView
        android:id="@+id/detailArtist"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Artist:"
        android:textSize="18sp"
        android:textColor="#FFFFFF"
        android:layout_marginBottom="12dp" />

    <!-- Rating -->
    <TextView
        android:id="@+id/detailRating"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Rating:"
        android:textSize="18sp"
        android:textColor="#FFFFFF"
        android:layout_marginBottom="12dp" />

    <!-- Comment -->
    <TextView
        android:id="@+id/detailComment"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Comment:"
        android:textSize="18sp"
        android:textColor="#FFFFFF"
        android:layout_marginBottom="24dp" />

    <!-- Back Button -->
    <Button
        android:id="@+id/btnBack"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Back to Playlist"
        android:textColor="#FFFFFF"
        />
</LinearLayout>
    </RelativeLayout>

    strings.xml
    <resources>
    <string name="app_name">MusicPlaylistApp</string>
    <string name="calculate_average">Calculate Average Rating</string>
    <string name="average_rating">Average Rating:</string>
    <string name="back_to_list">Back to List</string>
    <string name="song_details">Song Details</string>
</resources>
