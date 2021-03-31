# albums

A simple POC of a UI and an API working through Docker.

## Introduction

This repository contains two submodules (more info later) and some Docker set-up to get it all working.

Here are some pointers for both of these submodules:

- `albums-api`:
    - Written in Laravel 8, using a JSON:API package to help with standardising output to clients
    - Some PHPUnit testing was included - however test coverage isn't high
    - A GitHub Action is available to run the tests when the branch is pushed to the remote
    - The code is formatted following the PSR-2 and PSR-12 coding standards

- `albums-ui`:
    - Written in Vue.js 2, using `vue-router`, `tailwindcss` and `sass`
    - Not a lot of code for the front-end meaning it compiles pretty quickly

There is no `env` package being used on the front-end therefore all URI/URLs to the API have been hard-coded to `http://api.albums.test`.

## Getting Started

To run the application on your machine, firstly clone the entire repository using the following command:

```shell
git clone --recurse-submodules git://github.com/joshmurrayeu/albums.git
```

or if you're on an older version of `git`:

```shell
git clone --recursive git://github.com/joshmurrayeu/albums.git
```

Next, change directory to the repository directory. Run this command to build the docker images and run them in the background:

```shell
docker-compose up -d --build
```

Note: `--build` isn't actually necessary however it's a habit of mine always to add it to this command.

Once the container has been built (or when all 5 services are running), you'll need to migrate the database and run the seeders in order to populate the fake data. Run these
commands:

```shell
docker exec -it albums_albums-api_1 /bin/bash
cd ../albums-api
php artisan migrate --seed
```

If you get a `Error: No such container` error, run `docker ps` to find the name of the `albums_albums-api` container and use this name instead of the one
above (`albums_albums-api_1`).

If you're using Windows, you may need to put these values in your `hosts` file:

```text
127.0.0.1 api.albums.test
127.0.0.1 ui.albums.test
```

Finally, open a web browser and navigate to http://ui.albums.test/. Hopefully you should see:

![screenshot-of-homepage.png](screenshot-of-homepage.png)

## Structure

The structure of the application is a little different from other forms of POCs like this.

I have decided to have the API code in one repository (`albums-api`) and the UI in another (`albums-ui`). This repository (`albums`) brings them both together
through [Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

## Known Issues

- [FE] [Minor] When on a page such as the users view or albums view pages, when refreshing, a web-server error will be returned (most likely a 404). This occurs due to the FE being
  a SPA and the web-server not catching all paths and rerouting this to the FE application.

## Questions?

Let me know :) 