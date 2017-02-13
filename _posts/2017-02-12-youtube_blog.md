---
layout: post
title:  "YouTube API to access the videos,Channels and playlists using Python Language."
categories: Python Django,DRF,MongoDB,AngularJs & Bootstrap
---



 [Logo]: https://www.google.co.in/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&cad=rja&uact=8&ved=0ahUKEwia9fOirIfSAhXMKY8KHfE8DbUQjRwIBQ&url=http%3A%2F%2Fdiylogodesigns.com%2Fblog%2Fyoutube-logo%2F&psig=AFQjCNG2wghsP2QRJCegISu7C4sPY3TcCQ&ust=1486878523331952
 
 First of all, what is an **API**(Application Program Interface)?

 API can be defined as the set of routines,protocols and tools for building for a Software.In layman's terms, a piece of software(eg:website or operating software) allows 
 other computer programs to interact with it.
 
 
 In this post, we will see how to access the youtube videos, playlists and channels using Youtube APIs.
 
 Language Used : Python 
 
 Steps to be followed:
 
 Step 1: Create a **Google Account** to access the Google Developers Console.
 
 Step 2: create a service account key from console.developers.google.com
 
 Step 3: Create a [default credentials] to get authorization to use in calling Google APIs (https://developers.google.com/identity/protocols/application-default-credentials)
 
 Step 4: client_secret.json will be created/generated which authenticates the user.
 
 Step 5: Export the credentials file by using 
    
  In python console:
    
    
    import credentials
    
    export GOOGLE_APPLICATION_CREDENTIALS="./client_secret.json"
    
  
 Step 6: Python code for the Youtube API
    
    from googleapiclient.discovery import build
    from oauth2client.client import GoogleCredentials


    credentials = GoogleCredentials.get_application_default()

    def youtube_search(kw=None ,max_results=20):
        youtube = build('youtube', 'v3', credentials=credentials)
        search_response= youtube.search().list(
            q=kw,
            part="id,snippet",
            maxResults= max_results
        ).execute()

    videos = []
    channels = []
    playlists = []

    for search_result in search_response.get("items", []):
        t = {}
        if search_result["id"]["kind"] == "youtube#video":
            t['link'] = "https://www.youtube.com/watch?v=%s"%search_result['id']['videoId']
            t['title'] = search_result['snippet']['title']
            videos.append(t)

        elif search_result["id"]["kind"] == "youtube#channel":
            t['link']= "https://www.youtube.com/channel/%s"%search_result["id"]["channelId"]
            t['title']= search_result["snippet"]["title"]
            channels.append(t)

        elif search_result["id"]["kind"] == "youtube#playlist":
            t['link']="https://www.youtube.com/playlist?list=%s"%search_result["id"]["playlistId"]
            t['title']=search_result["snippet"]["title"]
            playlists.append(t)

    result = {}
    result['videos']=videos
    result['channels']=channels
    result['playlists']=playlists
    return result

for simple Usage call:

    result = youtube_search(kw='python tutorials')
    print result
    
    
    
 ###References:
 
 * https://developers.google.com/youtube/v3/getting-started
 
 * https://developers.google.com/identity/protocols/application-default-credentials
 
 
All the Best ...:)

