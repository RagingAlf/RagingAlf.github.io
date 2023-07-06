# Update of the Code
i updated the Code a bit. Now i can save the Scraped Data into a .csv file and i can also save the title, likes, published date, author and views.
here is the new Code:

import csv
from googleapiclient.discovery import build

# API-Key
API_KEY = 'Your API Key'

# YouTube Data API initializing
youtube = build('youtube', 'v3', developerKey=API_KEY)

# Video search and Save to .csv
def search_videos_and_save(query, filename):
    response = youtube.search().list(
        q=query,
        part='id,snippet',
        type='video',
        maxResults=10
    ).execute()

    # open csv
    with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
        writer = csv.writer(csvfile)
        writer.writerow(['Video ID', 'Title', 'Author', 'Published Date', 'Views', 'Likes'])

        for item in response['items']:
            video_id = item['id']['videoId']
            title = item['snippet']['title']
            author = item['snippet']['channelTitle']
            published_date = item['snippet']['publishedAt']

            # Video-Statistics
            stats_response = youtube.videos().list(
                part='statistics',
                id=video_id
            ).execute()

            
            # Thumbs and viewcount
            if stats_response['items']:
                views = stats_response['items'][0]['statistics'].get('viewCount', 'N/A')
                likes = stats_response['items'][0]['statistics'].get('likeCount', 'N/A')
            else:
                views = 'N/A'
                likes = 'N/A'

            # save to csv
            writer.writerow([video_id, title, author, published_date, views, likes])

# add here your search and save parameter
search_videos_and_save('WitchCraft', 'bla.csv')

I tried to Scrape Tiktok Videos and Commentaries too but there is no API for public use.
