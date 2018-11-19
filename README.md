# dockerhub-build-status-image

[![](https://dockerbuildbadges.quelltext.eu/status.svg?organization=niccokunzmann&repository=dockerhub-build-status-image)](https://hub.docker.com/r/niccokunzmann/dockerhub-build-status-image/builds/)  
`https://dockerbuildbadges.quelltext.eu/status.svg?organization=niccokunzmann&repository=dockerhub-build-status-image`

Show status badges of your dockerhub automated build in your README.md (like those for travis).

As soon as an image is here ![](https://img.shields.io/docker/build/mariobehling/loklak.svg), it can be used from [shields.io](http://shields.io/).

Architecture
------------

- SVG image
  - Pulls JS file
    - JS file includes list of status servers (since dockerhub does not allow crossorigin requests)
- Status servers
  - Form
    - A python package
    - A docker container (to use the badge :) )
    - A heroku deploy
  - May serve the svg file but better if they do not. To provide more fault tolerance, see the JS file

API
---

### Server

- `GET /build/<organnization>/<repository>`  
  `GET /build/<organnization>/<repository>?tag=<tag>`  
  Get the build status of an automated build.
  - `organization` is the dockerhub organization. Examples: `library` and `mariobehling`
  - `repository` is the repository in this organization. Examples: `nginx` and `loklak`
  - `tag` is optional, it is `latest` by default. Examples: `latest`

  Headers:
  - `Access-Control-Allow-Origin: *`

  Result:
  - In case the request had an error:  
    `{"request":"error","description":<text>}`  
    Where `text` is the error description.
  - In case all went fine:  
    `{"request":"ok", status:<build status>}`  
    The `build status` is
    - Negative for an error. Example: `-1`
    - Positive for success. Example: `1`
    - It gets taken like from [this example](https://hub.docker.com/v2/repositories/library/nginx/)

- `GET /source`  
  Get the source code.

- `GET /status.svg`  
  See [status.svg][status]

### status.svg
[status]: #statussvg

Parameters:
- `organization` is the name of the dockerhub organization.
  If it is left out, the name will be `library`.
- `repository` is the name of the repository. This must be given.
- `tag` is the name of the tag to use.
  If it is left out, the tag will be `latest`.
- `text` is the text to show on the badge.
  If it is left out, the text will be `Docker`.

Examples:
- ![](https://dockerbuildbadges.quelltext.eu/status.svg?organization=niccokunzmann&repository=dockerhub-build-status-image)
  `https://dockerbuildbadges.quelltext.eu/status.svg?organization=niccokunzmann&repository=dockerhub-build-status-image`
- ![](https://niccokunzmann.github.io/dockerhub-build-status-image/status.svg?organization=mariobehling&repository=loklak)
  `https://niccokunzmann.github.io/dockerhub-build-status-image/status.svg?organization=mariobehling&repository=loklak`
  If you have JavaScript enabled, this will ask for several servers.
  This is more fault in case servers go down.

Badge Servers
-------------

You can contribute a badge server to this list here and in [status.js](status.js):

- https://dockerbuildbadges.quelltext.eu/status.svg (daily update, docker)

Contribute
----------

As said, you can contribute a server or write your own. The API is open.
Contribute by solving [issues](https://github.com/niccokunzmann/dockerhub-build-status-image/issues).
I created the most basic version.
Have a look and show this project some love and improve it <3

Keywords
--------

- Status images for dockerhub automated builds
- Build status badge for docker images
- SVG badges

Reading
-------

- API
  - https://forums.docker.com/t/docker-hub-api-documentation/9091/3
  - https://hub.docker.com/v2/repositories/mariobehling/loklak/buildhistory/?page_size=100
  - https://hub.docker.com/v2/repositories/mariobehling/loklak/autobuild/
- This repository is part of the [first-timers-only](https://github.com/search?utf8=%E2%9C%93&q=label%3Afirst-timers-only+is%3Aopen&type=Issues&ref=searchresults) issue series - for people who want to get into open-source.
- [shields.io issue](https://github.com/badges/shields/issues/886)

---

The repository is maintained at <https://github.com/niccokunzmann/dockerhub-build-status-image/>
