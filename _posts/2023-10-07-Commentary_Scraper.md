# Data Usage and a commentary Scraper

With the data from the video scraping, you saved the YouTube ID of the videos, and with this data, you can create a commentary scraper for the specific video. 
To do this, you need the YouTube ID of the video from the video scraping. 
Here's the code:
import csv
from googleapiclient.discovery import build

# API-Schlüssel einfügen
API_KEY = 'YOUR_API_KEY'

# YouTube Data API 
youtube = build('youtube', 'v3', developerKey=API_KEY)

# Save commentarys in .csv
def get_video_comments_and_save(video_id, filename):
    # Kommentare abrufen
    response = youtube.commentThreads().list(
        part='snippet',
        videoId=video_id,
        maxResults=100
    ).execute()

    comments = []
    
    # extract commentaries
    for item in response['items']:
        comment = item['snippet']['topLevelComment']['snippet']['textDisplay']
        like_count = item['snippet']['topLevelComment']['snippet']['likeCount']
        creator = item['snippet']['topLevelComment']['snippet']['authorDisplayName']
        created_at = item['snippet']['topLevelComment']['snippet']['publishedAt']
        comments.append({'Comment': comment, 'Likes': like_count, 'Creator': creator, 'Created At': created_at})

    # save csv
    with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
        fieldnames = ['Comment', 'Likes', 'Creator', 'Created At']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        writer.writeheader()

        for comment in comments:
            writer.writerow(comment)

# example
get_video_comments_and_save('VIDEO_ID', 'comments.csv')

With this Python code, you can save all comments in a .csv file that you can use in Excel or similar programs. 
The code also saves the likes of the comments and the author. This allows you to get a good overview of the comments and reactions. 
It's useful if you want to analyze the reactions or if you're searching for specific topics in the comments. 
I want to do an analysis with this data, but I'm still searching for a topic.
