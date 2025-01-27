# Incredibox-IOS
An Incredibox's IOS Progressive Web App made from a source code hacked from the Incredibox Android version. It supports **IOS**. 

You can run it on apache, nginx and any other web server. 

Except for the audio assets, the IOS version looks similar to the PC version and the Android version. 

All the differences seem to be just `js/main.min.js`, `js/index.min.js` and all audio assets. 

### Why we can't use the [Incredibox (hacked source code)](https://github.com/DarkReaper231/Incredibox) on Safari for IOS? 
Because Safari for IOS doesn't support to decord `.ogg` files. In Incredibox Android version and Windows version, the audios files are `.ogg`. But in IOS version, the audios files are `.mp3`. The source code were hacked from the Incredibox Android version, so Safari won't play the audio files normally. 
![ Safari for IOS doesn't support to decode .ogg files](https://user-images.githubusercontent.com/83176414/185727489-8aa38d97-00d9-43e7-a38c-655c8eb0855f.png)

## But why it work on some websites like`Wiktionary.org`? 
`Wiktionary.org` actually load the `.ogg` files along with these two scripts: 
~~~
ogv-worker-audio.js
ogv-decoder-audio-vorbis.js
~~~
The scripts are part of the [`ogv.js`](https://github.com/brion/ogv.js/) JavaScript library. It use `JavaScript` to decord the `.ogg` files. So we can play it on `Wiktionary.org`. You can read more on [there](https://stackoverflow.com/questions/38581887/safari-doesnt-play-ogg-files-so-how-does-it-work-on-wiktionary-org). 

# What did I do? 
I changed the audio assets type to `mp3` by the [`FFmpeg`](https://github.com/FFmpeg/FFmpeg) and these code in `js/main.min.js` and `js/index.min.js`. 
~~~js
target = "mobile",
sndtype = "mp3",
osname = "ios",
appTotalVersion = appCN && "ios" == osname ? 4 : 8,
~~~
Safari supports `.mp3`, so you can play Incredibox on Safari by it. 
## How to know the audio assets type in different versions?
You can use unzip software like [7-zip](https://www.7-zip.org/) to extrat the audio assets from `.apk` 
~~~shell
$ unzip -q incredibox.apk -d ./apk-extract
$ cd ./apk-extract/assets/www/asset-v8/sound/
$ ls
ogg
~~~
and `.ipa`. 
~~~shell
$ unzip -q incredibox.ipa -d ./ipa-extract
$ cd ./ipa-extract/Payload/incredibox.app/www/asset-v8/sound/
$ ls
-metronome.wav  mp3 
~~~

Incredibox for Windows based on [electron](https://github.com/electron/electron), so you can use [asar](https://github.com/electron/asar) to extract the `app.asar` file.
~~~shell
$ unzip -q incredibox.Appx -d ./Appx-extract
$ cd ./Appx-extract/app/resources
$ ls
app.asar  electron.asar
$ npm install -g asar
$ asar extract app.asar ./extract
$ cd ./extract/app/asset-v8/sound
$ ls
ogg
~~~
# IOS PWA support
I add these to `index.html` and `app.html`, so now it support IOS PWA.
~~~html
<link rel="icon" type="png" sizes="512x512" href="./pwa-files/touch-icon512.png">
<link rel="manifest" href="./pwa-files/manifest.webmanifest">
<meta name='apple-mobile-web-app-capable' content='yes'>
<meta name='apple-mobile-web-app-title' content='Incredibox'>
<meta name='apple-mobile-web-app-status-bar-style' content='black-translucent'>
<link href='./pwa-files/touch-icon.png' rel='apple-touch-icon'>
~~~
Now it look really like Incredibox for IOS. XD

![PWA](https://user-images.githubusercontent.com/83176414/185727186-3383df2c-8b82-4b43-8f69-4fc3f23f7590.png)

Unfortunately, orientation is not supported on IOS PWA. So the PWA can't keep landscape. We need to keep the phone landscape or we can't play the game. 
~~~json
//IOS PWA doesn't support
"orientation": "landscape",
~~~
[![orientation](https://user-images.githubusercontent.com/83176414/185727432-177d9086-3967-4265-9b74-a47318467122.png)](https://caniuse.com/?search=orientation)
