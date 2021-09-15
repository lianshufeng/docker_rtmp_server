## Usage

### To run the server
```
docker run -d -p 1935:1935 -p 8080:8080 lianshufeng/rtmp
```

For Alpine-based Image use:
```
docker run -d -p 1935:1935 -p 8080:8080 lianshufeng/rtmp
```

To run with custom conf file:
```
docker run -d -p 1935:1935 -p 8080:8080 -v custom.conf:/etc/nginx/nginx.conf lianshufeng/rtmp
```
where `custom.conf` is the new conf file for Nginx.

### To stream to the server
 * **Stream live RTMP content to:**
	```
	rtmp://<server ip>:1935/live/<stream_key>
	```
	where `<stream_key>` is any stream key you specify.

 * **Configure [OBS](https://obsproject.com/) to stream content:** <br />
Go to Settings > Stream, choose the following settings:
   * Service: Custom Streaming Server.
   * Server: `rtmp://<server ip>:1935/live`. 
   * Stream key: anything you want, however provided video players assume stream key is `test`

### To view the stream
 * **Using [VLC](https://www.videolan.org/vlc/index.html):**
	 * Go to Media > Open Network Stream.
	 * Enter the streaming URL: `rtmp://<server ip>:1935/live/<stream-key>`
	   Replace `<server ip>` with the IP of where the server is running, and
	   `<stream-key>` with the stream key you used when setting up the stream.
	 * For HLS and DASH, the URLs are of the forms: 
	 `http://<server ip>:8080/hls/<stream-key>.m3u8` and 
	 `http://<server ip>:8080/dash/<stream-key>_src.mpd` respectively.
	 * Click Play.

* **Using provided web players:** <br/>
The provided demo players assume the stream-key is called `test` and the player is opened in localhost. 
	* To play RTMP content (requires Flash): `http://localhost:8080/players/rtmp.html` 
	* To play HLS content: `http://localhost:8080/players/hls.html`
	* To play HLS content using hls.js library: `http://localhost:8080/players/hls_hlsjs.html`
	* To play DASH content: `http://localhost:8080/players/dash.html`
	* To play RTMP and HLS contents on the same page: `http://localhost:8080/players/rtmp_hls.html`

	**Notes:** 

	* These web players are hardcoded to play stream key "test" at localhost.
	* To change the stream source for these players. Download the html files and modify the `src` attribute in the video tag in the html file. You can then mount the modified files to the container as follows:
		```
		docker run -d -p 1935:1935 -p 8080:8080 -v custom_players:/usr/local/nginx/html/players lianshufeng/rtmp
		```
		where `custom_players` is the directory holding the modified html files.
