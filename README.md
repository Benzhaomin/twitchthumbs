# TwitchThumbs

Serve thumbnails of Twitch profile images. Thumbs are generated on-demand and cached.

# Install

Clone the repo, give write access to your server/php-fpm on the `generated` folder
and you're good to go.

## Clean URLs

It's better to have clean URLs to work well with all kinds of proxies.
You can turn the default `?src=profileimage.png` into a cleaner `/profileimage.png`.

An Apache .htaccess file is already there, it does some surface validation to just 404
on invalid URLs.

For Lighttpd you can use the following:

```
url.rewrite-once = (
  "^/([a-zA-Z0-9%\-_\.]+.(jpg|jpeg|png))$" => "/index.php?src=$1"
)
```

# Configuration

You can just edit the top of index.php to change the path to the folder where generated
thumbs are put and to change the size of the thumbnail.

# Usage

Simply pass the filename of the profile image and you'll get a raw thumbnail back.

For instance:

```
<img src="http://twitchthumbs/nicktron-profile_image-ef3f5b4d21b16f4d-300x300.jpeg">

```

Exception: users with a defaut profile image use `xarth/404_user_300x300.png`,
don't just pass `404_user_300x300.png`, you need to urlencode the full string.

# Behind the scenes

```
if we didn't cache this thumbnail:
  make 100% sure the request is for a Twitch profile image (to avoid any sort of exploit)
  load the image from Twitch
  generate a local thumbnail

if the browser already has the thumbnail:
  tell him to use that, thumbs never expire

return the raw thumbnail

```

# License

> TwitchThumbs, a simple script to generate thumbnails of Twitch profile images.
> Copyright (C) 2015 Benjamin Maisonnas

> This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License version 3 as published by
the Free Software Foundation.

> This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

> You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/gpl-3.0.en.html>.
