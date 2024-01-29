<img src="./screenshots/photoview-logo.svg" height="92px" alt="photoview logo" />

[![License](https://img.shields.io/github/license/viktorstrate/photoview)](./LICENSE.md)
[![GitHub contributors](https://img.shields.io/github/contributors/viktorstrate/photoview)](https://github.com/viktorstrate/photoview/graphs/contributors)
[![Docker Pulls](https://img.shields.io/docker/pulls/viktorstrate/photoview)](https://hub.docker.com/r/viktorstrate/photoview)
[![Docker builds](https://github.com/photoview/photoview/actions/workflows/build.yml/badge.svg?branch=master)](https://github.com/photoview/photoview/actions/workflows/build.yml)
[![codecov](https://codecov.io/gh/photoview/photoview/branch/master/graph/badge.svg?token=AATZKC93F7)](https://codecov.io/gh/photoview/photoview)

![screenshot](./screenshots/timeline.png)

Photoview is a simple and user-friendly photo gallery that's made for photographers and aims to provide an easy and fast way to navigate directories, with thousands of high-resolution photos.

You configure Photoview to look for photos and videos within a directory on your file system. The scanner automatically picks up your media and start to generate thumbnail images to make browsing super fast.

When your media has been scanned they show up on the website, organised in the same way as on the filesystem.

> If you have questions regarding setup or development,
feel free to join the Discord server https://discord.gg/jQ392948u9

## Demo site

Visit https://photos.qpqp.dk/

Username: **demo**
Password: **demo**

## Contents

- [Demo site](#demo-site)
- [Main features](#main-features)
- [Supported Platforms](#supported-platforms)
- [Why yet another self-hosted photo gallery](#why-yet-another-self-hosted-photo-gallery)
- [Getting started - Setup with Docker](#getting-started---setup-with-docker)
- [Set up development environment](#setup-development-environment)
- [Sponsors](#sponsors)

## Main features

- **Closely tied to the file system**. The website presents the images found on the local filesystem of the server, directories are mapped to albums.
- **User management**. Each user is created along with a path on the local filesystem, photos within that path can be accessed by that user.
- **Sharing**. Albums, as well as individual media, can easily be shared with a public link, the link can optionally be password protected.
- **Made for photography**. Photoview is built with photographers in mind, and thus supports **RAW** file formats, and **EXIF** parsing.
- **Video support**. Many common video formats are supported. Videos will automatically be optimized for web.
- **Face recognition**. Faces will automatically be detected in photos, and photos of the same person will be grouped together.
- **Performant**. Thumbnails are automatically generated and photos first load when they are visible on the screen. In full screen, thumbnails are displayed until the high resolution image has been fully loaded.
- **Secure**. All media resources are protected with a cookie-token, all passwords are properly hashed, and the API uses a strict [CORS policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

## Supported platforms

- [Docker](https://hub.docker.com/r/viktorstrate/photoview/)
- [Arch Linux Aur](https://aur.archlinux.org/packages/photoview)
- [Unraid](https://forums.unraid.net/topic/103028-support-photoview-corneliousjd-repo/)
- EmbassyOS: [announcement](https://start9labs.medium.com/new-service-photoview-72ee681b2ff0), [repo](https://github.com/Start9Labs/embassyos-photoview-wrapper)
- [YunoHost](https://github.com/YunoHost-Apps/photoview_ynh)

## Why yet another self-hosted photo gallery

There exists a lot of open-source self-hosted photo galleries already. Here are some, just to mention a few.

- [Piwigo](https://github.com/Piwigo/Piwigo)
- [LibrePhotos](https://github.com/LibrePhotos/librephotos)
- [Photoprism](https://github.com/photoprism/photoprism)
- [Lychee](https://github.com/LycheeOrg/Lychee)

So why another one?
I love taking photos, and I store all of them on my local fileserver.
This is great because I can organize my photos directly on the filesystem so it's easy to move them or take backups. I want to be able to control where and how the photos are stored.

The problem is however that RAW images are extremely tedious to navigate from a fileserver, even over the local network.

My server holds a lot of old family pictures, that I would like my family to have access to as well.
And some of the pictures I would like to easily be able to share with other people without the hassle of them having to make an account first.

Thus I need a solution that can do the following:

- A scan based approach that automatically organises my photos
- Support RAW and EXIF parsing
- Have support for multiple users and ways to share albums and photos also publicly
- Be simple and fast to use

All of the photo galleries can do a lot of what I need, but no single one can do it all.

## Getting started - Setup with Docker

> This section describes how to get Photoview up and running on your server with Docker.
> Make sure you have Docker and docker-compose installed and running on your server.
> `make` should be installed as well if you'd like to use provided `Makefile`, which is optional (see step 4 for more details).
> `7zz` should be installed in case, you'd like to use it in scope of the backup scenario instead of the default .tar.xz format. Read the comment in the `Makefile`, located in the `backup` section for more details.

1. Clone this repo to the folder on your server, where you expect to host the Photoview internal data (database and cache files).
   - `cd <path to the folder on your server, where you expect to host the Photoview internal data>`
   - `git clone https://github.com/kkovaletp/photoview.git .`
   - `git checkout NOT-FOR-MERGE-rebuild-photoview-image`
   - `git pull`
2. Rename `docker-compose.example.yml` and `example.env` files and remove the `example` from their names (so, you need to have `.env` and `docker-compose.yml` files).
3. Open these files and the `Makefile` in a text editor and read them. Modify where needed according to the documentation comments to properly match your setup. It is better to go through the files in the next order: `.env`, `docker-compose.yml`, and `Makefile`.
4. In case, you don't have `make` installed in your system or don't want to use it for the Photoview management activities, you could use the same commands from the `Makefile` and run them in your shell directly, or create your own scripts. Make sure to apply or replace the variables from your `.env` first in this case. `Makefile` is provided just for your convenience and simplicity, but is optional.
5. Start the server by running the following command (or corresponding sequence of commands from the `Makefile`):

```bash
make all
```

If the endpoint or the port hasn't been changed in the `docker-compose.yml` file, Photoview can now be accessed at http://localhost:8000

> NOTE:
> This sequence will build the Photoview image from local sources replacing the original
> `viktorstrate/photoview:latest` image, published on the docker hub by the repo owner.
> This is a workaround of the fact that the project is not actively maintained,
> and that image is pretty outdated, having some vulnerabilities and old software.
> If you want to use the original image, do not build the local one
> (exclude the `build` step from the sequence in the `Makefile`)
> and remove the local `viktorstrate/photoview:latest` image.
> Next time you start the service, the original image will be pulled from the docker hub.

### Initial Setup

If everything is set up correctly, you should be presented with an initial setup wizard, when accessing the website the first time.

![Initial setup](./screenshots/initial-setup.png)

Enter a new username and password.

For the photo path, enter the path in the docker container where your photos are located.
This can be set from the `docker-compose.yml` file under `api` -> `volumes`.
The default location is `/photos`

A new admin user will be created, with access to the photos located at the path provided under the initial setup.

The photos will have to be scanned before they show up, you can start a scan manually, by navigating to `Settings` and clicking on `Scan All`

## Set up development environment

### Local setup

1. Install a local mysql server, and make a new database
2. Rename `/api/example.env` to `.env` and update the `MYSQL_URL` field
3. Rename `/ui/example.env` to `.env`

### Start API server

Make sure [golang](https://golang.org/) is installed.

Some C libraries are needed to compile the API, see [go-face requirements](https://github.com/Kagami/go-face#requirements) for more details.
They can be installed as shown below:

```sh
# Ubuntu
sudo add-apt-repository ppa:strukturag/libheif
sudo add-apt-repository ppa:strukturag/libde265
sudo apt-get update
sudo apt-get install libdlib-dev libblas-dev libatlas-base-dev liblapack-dev libjpeg-turbo8-dev libheif-dev
# Debian
sudo apt-get install libdlib-dev libblas-dev libatlas-base-dev liblapack-dev libjpeg62-turbo-dev libheif-dev
# macOS
brew install dlib libheif

```

Then run the following commands:

```bash
cd ./api
go install
go run server.go
```

### Start UI server

Make sure [node](https://nodejs.org/en/) is installed.
In a new terminal window run the following commands:

```bash
cd ./ui
npm install
npm start
```

The site can now be accessed at [localhost:1234](http://localhost:1234).
And the graphql playground at [localhost:4001](http://localhost:4001)

## Sponsors

<table>
<tr>
  <td>
    <a href="https://github.com/ericerkz">
      <img src="https://avatars.githubusercontent.com/u/79728329?v=4" height="auto" width="100" style="border-radius:50%"><br/>
      <b>@ericerkz</b>
    </a>
  </td>
  <td>
    <a href="https://github.com/robin-moser">
      <img src="https://avatars.githubusercontent.com/u/26254821?v=4" height="auto" width="100" style="border-radius:50%"><br/>
      <b>@robin-moser</b>
    </a>
  </td>
  <td>
    <a href="https://github.com/Revorge">
      <img src="https://avatars.githubusercontent.com/u/32901816?v=4" height="auto" width="100" style="border-radius:50%"><br/>
      <b>@Revorge</b>
    </a>
  </td>
  <td>
    <a href="https://github.com/deexno">
      <img src="https://avatars.githubusercontent.com/u/50229919?v=4" height="auto" width="100" style="border-radius:50%"><br/>
      <b>@deexno</b>
    </a>
  </td>
  <td>
    <a href="https://github.com/FKrauss">
      <img src="https://avatars.githubusercontent.com/u/4820683?v=4" height="auto" width="100" style="border-radius:50%"><br/>
      <b>@FKrauss</b>
    </a>
  </td>
  <td>
    <a href="https://github.com/jupblb">
      <img src="https://avatars.githubusercontent.com/u/3370617?v=4" height="auto" width="100" style="border-radius:50%"><br/>
      <b>@jupblb</b>
    </a>
  </td>
</table>
