
# HugFlix

Your selfhosted Netflix-like that downloads Series automatically thanks to Jackett, FlareSolverr, Sonarr and Deluge. Build using Docker.

## Dependencies

* Docker on your computer or server
* Git to clone this repository

## Run

1. Clone this repo
2. Go in the HugFlix folder with your terminal 
3. Type 'make run' to launch the docker containers (if you don't have make on your computer, run 'docker compose up -d'). You can also stop the containers with 'make stop' (idem, run 'docker compose down')
4. Configure your own HugFlix by following the configuration steps hereafter.

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
6. Copy the api key (without the ending point) from Jackett homepage (in Adding a Jackett indexer in Sonarr or Radarr section), and paste it into 'API KEY' on Sonarr
7. On Sonarr, in 'Categories', select 'TV'
8. Click on 'Save'

--> Go to [Deluge Configuration](#deluge-configuration) and then come back to finish configuring Sonarr

### Downloader configuration

1. Go to http://localhost:8989
2. Go to 'Settings' -> 'Download Clients' -> Click on the + icon
3. Click on 'Deluge'
4. Choose a name
5. Change 'Host' to 'host.docker.internal'
6. Set the password you defined while configuring Deluge
7. Click on 'Save'

--> Go to [Label configuration](#label-configuration) for Deluge to finish configuration

## Deluge Configuration

### Password and plugins configuration

1. Go to http://localhost:8112
2. Type 'deluge' and then click 'Login'
3. Click on 'Yes' to change the default Password
4. A window will open, in the 'WebUI Password' section, type the old password (deluge) and your new password then click 'Apply'
5. In the same window (Preferences), go to 'Plugins' section and tick 'Label', then click 'Apply'

--> Go to back to [Downloader Configuration](#downloader-configuration)

### Label configuration

1. Go to http://localhost:8112
2. Click on 'Preferences'
3. In the 'Labels' section on the left, a new label 'tv-sonarr' has appeared
4. Click right on it -> 'Label Options'
5. In the 'Folders' tab, tick 'Apply folder settings' and 'Move completed to', then fill the field with '/Series' and click 'Ok'

### Download folder configuration

1. Go to http://localhost:8112
2. Click on 'Preferences'
3. In the 'Downloads' tab, change the 'Download to' field to '/Downloads'
4. Click on 'Apply'

### Inbound port configuration

1. Go to http://localhost:8112
2. Click on 'Preferences'
3. In the 'Newtork' tab, untick 'Use Random Port' and set the field to '6881' then click 'Apply'

## Adding a show for the first time on Sonarr

1. Click on 'Series' -> 'Add New'
2. Search for your favorite show and click on it
3. In 'Root Folder', click on 'Add a new path' and select the folder 'Series' (by clicking on it and then ok). You need to do that only for the first show you add.
4. You can select which episodes to search for in the 'Monitor field'
5. You can select the quality you want in the 'Quality Profile' field
6. You can tune the other parameters and then click on 'Add'

Sonarr will automatically search for the episodes on your indexer, Deluge will download them and move them to Hugflix/data/Series.

## TO DO 

- [ ]  Add Radarr to download Movies

