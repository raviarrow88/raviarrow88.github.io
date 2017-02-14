---
layout: post
title:  "Retrieving the Playlist videos via playlistId using youtube APIs"
categories: Python Django DRF
---

In my previous post, I have discussed about the searching  youtube videos using youtube API by giving string as the
input. Now the scenario is like, script should display the playlist videos using playlistId.

how to check for playlistId in youtube url ?

Example Url : https://www.youtube.com/watch?v=xRX6ZI_P-LA&list=**PLckPQEKYlbxebubMWdjcGR7K_ukm35ZjN**

The highlighted part in the url is playlistId.

First step is to create a service account key from console.developers.google.com

https://developers.google.com/identity/protocols/application-default-credentials

 
 youtube_playlist.py
 
    from googleapiclient.discovery import build
    from oauth2client.client import GoogleCredentials


    import os
    GOOGLE_CRED_PATH = os.path.join('./', "client_secret.json")
    os.environ.setdefault("GOOGLE_APPLICATION_CREDENTIALS", GOOGLE_CRED_PATH)



    credentials = GoogleCredentials.get_application_default()

    def youtube_search(playlistId):
        youtube = build('youtube', 'v3', credentials=credentials)
        search_response= youtube.playlistItems().list(
            playlistId= playlistId,
            part="snippet",
        ).execute()
    
    nextPageToken = search_response.get('nextPageToken')
    while('nextPageToken' in search_response):
        nextPage = youtube.playlistItems().list(
            playlistId = playlistId,
            part = "snippet",
            pageToken = nextPageToken,
        ).execute()

        search_response['items'] = search_response['items'] + nextPage['items']

        if 'nextPageToken' not in nextPage:
            search_response.pop('nextPageToken',None)
        else:
            nextPageToken = nextPage['nextPageToken']

        return search_response
    
    
    id = 'PLckPQEKYlbxebubMWdjcGR7K_ukm35ZjN'
    result = youtube_search(id)
    
    print result
 

Try it out ..cheers :)

