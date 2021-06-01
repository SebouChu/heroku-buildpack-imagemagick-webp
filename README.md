imagemagick-from-source-buildpack
===========================

Use [ImageMagick](www.imagemagick.org) from source inside a Heroku/Scalingo environment. This buildpack will download the library, build it from source and install it along side your app. Your ```$PATH``` and ```$LD_LIBRARY_PATH``` will also be updated to use this version of ImageMagick instead of the default Heroku/Scalingo one, which may _not_ have support of some formats (like WebP).

Since this buildpack is building ImageMagick from source your first deploy will take a very long time (around 10 mins). However after building once the installed library is stored in a cache directory and will be used for all future deploys. If for any reason you want to force a rebuild of ImageMagick, please check out:
- For Heroku, the [heroku-repo](https://github.com/heroku/heroku-repo) plugin.
- For Scalingo, [clear the deployment cache](https://doc.scalingo.com/platform/deployment/cache) with the CLI tool.

## Usage

### Heroku

```
heroku buildpacks:add --index 1 https://github.com/SebouChu/imagemagick-from-source-buildpack.git
```

### Scalingo

Create a `.buildpacks` file inside your app and add the buildpack like this:
```bash
https://github.com/SebouChu/imagemagick-from-source-buildpack.git
# Other buildpacks...
https://github.com/Scalingo/ruby-buildpack
```

The last line is for a Ruby application, but it can be any other buildpack you want. If it is not working with yours, please report a bug.

**NOTE**: The ImageMagick binary included in the `scalingo-18` stack does not support WebP natively. Here is what to do:

Add the [APT buildpack](https://doc.scalingo.com/platform/deployment/buildpacks/apt) before this buildpack in the `.buildpacks` file.
```bash
https://github.com/Scalingo/apt-buildpack.git
https://github.com/SebouChu/imagemagick-from-source-buildpack.git
# ...
```

Create an `Aptfile` file inside your app and add the WebP library:
```
libwebp-dev
```

## Verify Installation

You can verify that ImageMagick was built correctly by running the following command:
- Heroku : `heroku run convert --version`
- Scalingo : `scalingo run convert --version`

If the output matches the version specified in `bin/compile`, you're all set.

To verify the WebP support, run the following command:
- Heroku : `heroku run identify -list format`
- Scalingo : `scalingo run identify -list format`

If the output includes `WEBP`, you're all set.

## LICENSE - "MIT License"

Copyright (c) 2021 Sébastien Gaya, https://sebastiengaya.fr
Copyright (c) 2014 Nick Jensen, https://nrj.io
Copyright (C) 2012 Heroku, Inc.

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.