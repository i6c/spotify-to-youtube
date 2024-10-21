# Move Spotify playlists to YouTube Music

If you're trying to move from Spotify to YouTube Music, you can use this python script to take your playlists from Spotify and import them into YouTube Music.
It's _definitely_ not perfect... I ended up with a couple pretty weird songs in my imported playlists.
But it's about the best I think I can do, given the tools available.

# You gotta do some stuff first

Go export your playlists from Spotify.
This python script assumes you've used [exportify](https://github.com/watsonbox/exportify) to create CSV backups of your Spotify playlists.
If you're using a different method, you'll probably end up with a differently formatted file.
In which case, you'll need to create your own `load_spotify_playlist` function.

You also need to go through the setup instructions for [ytmusicapi](https://ytmusicapi.readthedocs.io/en/latest/setup.html) to make an auth file.
If you're having trouble with this, [my blog post](https://dev.to/dizzyspi/creating-a-spotify-to-youtube-music-playlist-converter-f39) on making the code in this repo might be helpful to you.

# How to use this script

Setup:

```
virtualenv -p python3 venv
source venv/bin/activate
pip install ytmusicapi
```

Run:

```
python main.py -h
usage: main.py [-h] [-p PLAYLIST] [-d DESCRIPTION] [-l] auth file

Import a Spotify playlist to YouTube Music

positional arguments:
  auth                  ytmusicapi auth file
  file                  Exported Spotify playlist CSV file

optional arguments:
  -h, --help            show this help message and exit
  -p PLAYLIST, --playlist PLAYLIST
                        Playlist name
  -d DESCRIPTION, --description DESCRIPTION
                        Playlist description
  -l, --likes           Add songs to your liked songs instead of a YouTube
                        Music playlist
```

# Extended help by i6c

citing this api docs https://ytmusicapi.readthedocs.io/en/stable/setup/browser.html


First all you need to do is go to youtube music and search for something with your network tab open. Look for a request that has "browser" in the URL path (preferably using  firefox) and rightclick the request tab itself, then go to copy > request headers. Then activate your venv and all of that, install ytmusicapi via pip and run `ytmusicapi browser`

It will ask you to put the request headers all you gotta do is right click then press ctrl + D it should look something like this

```
{
    "accept": "*/*",
    "accept-encoding": "gzip, deflate",
    "accept-language": "en-US,en;q=0.5",
    "alt-used": "music.youtube.com",
    "authorization": " ",
    "connection": "keep-alive",
    "content-encoding": "gzip",
    "content-type": "application/json",
    "cookie": "very long string ......................................................................",
    "dnt": "1",
    "origin": "https://music.youtube.com",
    "priority": "u=0",
    "referer": "https://music.youtube.com/search?q=example",
    "user-agent": "ua",
    "x-goog-authuser": "0",
    "x-goog-visitor-id": "id_%3D%3D",
    "x-origin": "https://music.youtube.com",
    "x-youtube-bootstrap-logged-in": "true",
    "x-youtube-client-name": "67",
    "x-youtube-client-version": "1.00"
}
```

if your json file looks something similar to that you should be good to continue and clone this project. then run this code in your console to get the program to run properly

`python3 main.py -p NAME YOU WANT THE PLAYLIST TO BE CALLED ON YOUTUBE -d YOUTUBE PLAYLIST DESCRIPTION browser.json path/to/exported/playlist.csv`

# Examples

Takes all the songs in liked.csv and adds them to your liked songs in YouTube Music.
```
$ python main.py auth_file.json liked.csv --likes
```

Takes all the songs in `energy_techno.csv` and adds them to the playlist "Energy Techno" (creates a new playlist if one does not already exist).
```
$ python main.py auth_file.json energy_techno.csv --playlist "Energy Techno"
```

Takes all the songs in `workout.csv` and adds them to the playlist "Workout" with description "Songs to get me PUMPED" (creates a new playlist if one does not already exist).
```
$ python main.py auth_file.json workout.csv --playlist "Workout" --description "Songs to get me PUMPED"
```

# Closing remarks
I've put in some error handling and what are hopefully useful messages, but I put this together in literally a day.
If you have problems, feel free to open an issue.
But I can't promise I'll get to it in a timely fashion whatsoever.
