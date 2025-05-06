# Feel-Beats
Moodify is a lightweight AI-powered Python bot that recommends YouTube songs based on the user's mood. It uses natural language input and sentiment analysis to detect emotions and suggest music that matches how you feel—happy, sad, calm, romantic, or energetic.
import pandas as pd
from textblob import TextBlob
import webbrowser
import random

# Load the song list from the CSV file
df = pd.read_csv('music_file.csv')
print(df.head())

# Get user's mood from input
user_input = input("How are you feeling today (in English)? ")

# Analyze sentiment
sentiment = TextBlob(user_input).sentiment.polarity

# Determine mood based on sentiment
if sentiment > 0.7:
    mood = 'happy'
elif sentiment < -0.1:
    mood = 'sad'
elif sentiment > 0.5:
    mood = 'calm'
elif sentiment > 0.2:
    mood = 'love'
else:
    mood = 'sad'  # If sentiment is below the threshold for 'calm' and 'love', it's considered 'sad'

# Filter songs by mood
matches = df[df['mood'] == mood]

# Shuffle the matching songs
shuffled_matches = matches.sample(frac=1).reset_index(drop=True)

# Play the first song in the shuffled list
if shuffled_matches.empty:
    print("No songs found for your mood.")
else:
    song = shuffled_matches.iloc[0]
    print(f"\n🎵 Playing '{song['title']}' from {song['movie']} (Mood: {mood})")
    print(f"Opening: {song['youtube']}")
    webbrowser.open_new_tab(song['youtube'])
