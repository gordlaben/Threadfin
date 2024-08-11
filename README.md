<div align="center" style="background-color: #111; padding: 100;">
    <a href="https://github.com/marcelGoerentz/Threadfin"><img width="285" height="80" src="html/img/threadfin.png" alt="Threadfin" /></a>
</div>
<br>

# Threadfin
## M3U Proxy for Plex DVR and Emby/Jellyfin Live TV. Based on xTeVe.

You can follow the old xTeVe documentation for now until I update it for Threadfin. Documentation for setup and configuration is [here](https://github.com/xteve-project/xTeVe-Documentation/blob/master/en/configuration.md).

#### Donation
[Paypal/Me](https://paypal.me/MarcelGoerentz)

## Requirements
### Plex
* Plex Media Server (1.11.1.4730 or newer)
* Plex Client with DVR support
* Plex Pass

### Emby
* Emby Server (3.5.3.0 or newer)
* Emby Client with Live-TV support
* Emby Premiere

### Jellyfin
* Jellyfin Server (10.7.1 or newer)
* Jellyfin Client with Live-TV support

--- 

## Threadfin Features

* New Bootstrap based UI
* RAM based buffer instead of File based
* HTTPS Webserver

#### Filter Group
* Can now add a starting channel number for the filter group

#### Map Editor
* Can now multi select Bulk Edit by holding shift
* Now has a separate table for inactive channels
* Can add 3 backup channels for an active channel (backup channels do NOT have to be active)
* Alpha Numeric sorting now sorts correctly
* Can now add a starting channel number for Bulk Edit to renumber multiple channels at a time
* PPV channels can now map the channel name to an EPG
* Removed old Threadfin buffer option, since FFMPEG/VLC will always be a better solution

## xTeVe Features

#### Files
* Merge external M3U files
* Merge external XMLTV files
* Automatic M3U and XMLTV update
* M3U and XMLTV export

#### Channel management
* Filtering streams
* Channel mapping
* Channel order
* Channel logos
* Channel categories

#### Streaming
* Buffer with HLS / M3U8 support
* Re-streaming
* Number of tuners adjustable
* Compatible with Plex / Emby / Jellyfin EPG

---

## CLI arguments

These are the currently available command line arguments:

| arg        | type    | description                                             | example                                     |
|:-----------|:--------|:--------------------------------------------------------|:--------------------------------------------|
| -h         | bool    | prints the help and don't start the service             | -h                                          |
| -dev       | bool    | activates the developer mode                            | -dev                                        |
| -config    | string  | sets the path to the root config folder                 | -config=~./.threadfin                       |
| -port      | integer | sets the port for the webserver (also for https)        | -port=34400                                 |
| -useHttps  | bool    | switches the webserver to https                         | -useHttps                                   |
| -restore   | string  | restores the settings from the given filepath           | -restore=/path/to/file/threadfin_backup.zip |
| -gitBranch | string  | sets the branch from which the program is to be updated | -gitBranch=beta                             |
| -debug     | integer | sets the debug level                                    | -debug=3                                    |
| -info      | bool    | prints the system info                                  | -info                                       |

---

## Docker Image
[Threadfin](https://hub.docker.com/r/mgoerentz/threadfin)

* Docker compose example

```
version: "2.3"
services:
  threadfin:
    image: mgoerentz/threadfin
    container_name: threadfin
    ports:
      - 34400:34400
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=America/Los_Angeles
    volumes:
      - ./data/conf:/home/threadfin/conf
      - ./data/temp:/tmp/threadfin:rw
    restart: unless-stopped
```

---

### Threadfin Beta branch
New features and bug fixes are only available in beta branch. Only after successful testing they are merged into the main branch.

**It is not recommended to use the beta version in a production system.**  

With the command line argument `branch` the Git Branch can be changed. Threadfin must be started via the terminal.  

#### Switch from master to beta branch:
```
threadfin -branch=beta

...
[Threadfin] GitHub:                https://github.com/Threadfin/Threadfin
[Threadfin] Git Branch:            beta [Threadfin]
...
```

#### Switch from beta to master branch:
```
threadfin -branch=main

...
[Threadfin] GitHub:                https://github.com/Threadfin/Threadfin
[Threadfin] Git Branch:            main [Threadfin]
...
```

When the branch is changed, an update is only performed if there is a new version and the update function is activated in the settings.  

---

## Build from source code [Go / Golang]

#### Requirements
* [Go](https://golang.org) (go1.18 or newer)

#### Dependencies
* [go-ssdp](https://github.com/koron/go-ssdp)
* [websocket](https://github.com/gorilla/websocket)
* [avfs](github.com/avfs/avfs)

#### Build
1. Download source code
2. Install dependencies
```
go mod tidy && go mod vendor
```
3. Build Threadfin
```
go build threadfin.go
```

4. Update web files (optional)

If TypeScript files were changed, run:

```sh
tsc -p ./ts/tsconfig.json
```

Then, to embed updated JavaScript files into the source code (src/webUI.go), run it in development mode at least once:

```sh
go build threadfin.go
threadfin -dev
```

---

## Fork without pull request :mega:
When creating a fork, the Threadfin GitHub account must be changed from the source code or the update function disabled.
Future updates of Threadfin would update your fork.

threadfin.go - Line: 29
```Go
var GitHub = GitHubStruct{Branch: "main", User: "Threadfin", Repo: "Threadfin", Update: true}

/*
  Branch: GitHub Branch
  User:   GitHub Username
  Repo:   GitHub Repository
  Update: Automatic updates from the GitHub repository [true|false]
*/

```


