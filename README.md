
<p align="center">
  <img width="200" src="hugflix.png">
</p>

<p align="center">Your selfhosted Netflix-like that downloads Series automatically thanks to <a href="https://github.com/Jackett/Jackett">Jackett</a>, <a href="https://github.com/FlareSolverr/FlareSolverr">FlareSolverr</a>, <a href="https://github.com/Sonarr/Sonarr">Sonarr</a>, <a href="https://github.com/deluge-torrent/deluge">Deluge</a> and <a href="https://github.com/jellyfin/jellyfin">Jellyfin</a>. Built using <a href="https://www.docker.com">Docker</a>.</p>

## Dependencies

* Docker on your computer or server
* Git to clone this repository

## Run

1. Clone this repo
2. Go in the HugFlix folder with your terminal 
3. Type 'make run' to launch the docker containers (if you don't have make on your computer, run 'docker compose up -d'). You can also stop the containers with 'make stop' (idem, run 'docker compose down')
4. Configure your own HugFlix by following the configuration steps hereafter.

Links to services:
* Jackett : http://localhost:9117
* Sonarr : http://localhost:8989
* Deluge : http://localhost:8112
* Jellyfin : http://localhost:8096/

## Jackett Configuration

### FlareSolverr configuration

FlareSolverr helps bypassing Cloudflare protection on indexer website

1. Go to http://localhost:9117
2. Go down the page and fill 'FlareSolverr API URL' with http://host.docker.internal:8191
3. Click 'Apply server settings' (blue button)

### Indexer configuration

1. Go to http://localhost:9117
2. Click on 'Add Indexer' -> Search your favorite indexer (e.g. YggTorrent) -> Click on the wrench
3. Fill 'Username' and 'Password' then click 'Okay'

## Sonarr configuration

### Indexer configuration

1. Go to http://localhost:8989 
2. Go to 'Settings' -> 'Indexers' -> Click on the + icon
3. Click on 'Torznab'
4. Choose a name
5. Go back to Jackett, click on 'Copy Torznab Feed' for your indexer and paste it into 'URL' on Sonarr. Change 'localhost' in the url by 'host.docker.internal'
6. Copy the api key from Jackett homepage (upper right corner), and paste it into 'API KEY' on Sonarr
7. On Sonarr, in 'Categories', select 'TV'
8. Click on 'Save'

--> Go to [Deluge Configuration](#deluge-configuration) and then come back to finish configuring Sonarr

### Downloader configuration

1. Go to http://localhost:8989
2. Go to 'Settings' -> 'Download Clients' -> Click on the + icon
3. Click on 'Deluge'
4. Choose a name
5. In 'Host', change 'localhost' to 'host.docker.internal'
6. Set the password you defined while configuring Deluge
7. Click on 'Save'

### Media Management configuration

1. Go to http://localhost:8989
2. Go to 'Settings' -> 'Media Management'
3. Tick the 'Rename Episodes' box
4. Click on 'Save Changes' on the top of the page

--> Go to [Label configuration](#label-configuration) to finish Deluge configuration

## Deluge Configuration

### Password and plugins configuration

1. Go to http://localhost:8112
2. Type 'deluge' and then click 'Login'
3. Click on 'Yes' to change the default Password
4. If a 'Connection Manager' window appears, select the first line and click on 'Connect'
5. A 'Preferences' window will open, in the 'Interface' tab and in the 'WebUI Password' section, type the old password (deluge) and your new password then click 'Apply'. Close the window.
6. Open 'Preferences' again (top of the page). Go to 'Plugins' section and tick 'Label', then click 'Apply' and close the window.

--> Go back to [Downloader Configuration](#downloader-configuration)

### Label configuration

1. Go to http://localhost:8112
2. In the 'Labels' section on the left (under 'Filters'), a new label 'tv-sonarr' has appeared
3. Click right on it -> 'Label Options'
4. In the 'Queue' tab, tick 'Apply queue settings', 'Auto Managed' and 'Stop seed at ratio', and set the ratio to 0
5. Click 'Ok'

### Download folder configuration

1. Go to http://localhost:8112
2. Click on 'Preferences'
3. In the 'Downloads' tab, change the 'Download to' field to '/Downloads'
4. Click on 'Apply'

### Inbound port configuration

1. Go to http://localhost:8112
2. Click on 'Preferences'
3. In the 'Network' tab, untick 'Use Random Port' and set the field to '6881' then click 'Apply'

## Jellyfin configuration

### First launch

1. Go to http://localhost:8096
2. Choose your prefered language for the interface and create an user acccount
3. On the next page, click on the + icon to add the media library
4. In the media type, select 'TV Shows'
5. Add a folder by cliking on the + icon next to 'Folders' and select '/Series', then click 'OK'
6. Select your prefered language for the shows and your country
7. You can tune the other parameters and finally click on 'OK'
8. On the next page, you can choose the language for the metadata of your library
9. On the following page, you can leave the parameters like that and go to the final page.
10. Jellyfin is now configured and you can login with the user account you've configured before.

### Add TheTVDB plugin
1. Go to http://localhost:8096 and open the menu on the upper left corner 
2. Go to 'Dashboard' -> 'Plugins' -> 'Catalog' and click on 'TheTVDB'
3. Click on 'Install'
4. Restart Jellyfin with 'make stop' and 'make run' in the terminal 

### Use the TheTVDB plugin for your media library
1. Go to http://localhost:8096 and open the menu on the upper left corner 
2. Go to 'Dashboard' -> 'Libraries' -> Click on the 3 dots corresponding to your library and select 'Manage Library'
3. Tick TheTVDB each time you see it in the different sections and click 'OK'

## Adding a show for the first time on Sonarr

1. Click on 'Series' -> 'Add New'
2. Search for your favorite show and click on it
3. In 'Root Folder', click on 'Add a new path' and select the folder 'Series' (by clicking on it and then ok). You need to do that only for the first show you add.
4. You can select which episodes to search for in the 'Monitor field'
5. You can select the quality you want in the 'Quality Profile' field
6. You can tune the other parameters and then click on 'Add'

Sonarr will automatically search for the episodes on your indexer, Deluge will download them and move them to Hugflix/data/Series. The episodes will be available on Jellyfin to watch them.

## TO DO 

- [ ]  Add Radarr to download Movies

