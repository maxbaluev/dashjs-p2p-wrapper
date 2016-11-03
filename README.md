# dashjs-p2p-wrapper

This module wraps [`dash.js`](https://github.com/Dash-Industry-Forum/dash.js) to bootstrap it with [the Streamroot P2P module](http://streamroot.io).

It provides a bundle that extends the [`dash.js`](https://github.com/Dash-Industry-Forum/dash.js) `MediaPlayer` constructor to create a fully configured player which will use the Streamroot P2P module, giving you the exact same API.
You can integrate this bundle with minimal changes in your application. **The bundled version of dash.js is v2.3.0**.

It also provides a wrapper that allows you to create/configure a player with a specific version of [`dash.js`](https://github.com/Dash-Industry-Forum/dash.js). **Minimum supported version of dash.js is v2.2.0**.

### Install via npm
You can install the artifacts distributed as NPM modules:

For the wrapper with dash.js included:

```
npm install streamroot-dashjs-p2p-bundle
```

For the wrapper without dash.js:

```
npm install streamroot-dashjs-p2p-wrapper
```

In your application import/require the package you want to use as in the example like

```javascript
import DashjsP2PBundle from 'streamroot-dashjs-p2p-bundle';
```

or

```javascript
import DashjsWrapper from 'streamroot-dashjs-p2p-wrapper';
```

### Build

#### Prerequisites

First of all, make sure you are using a Node.js version >= 6.0.0

Since the building uses a Ruby script, you need Ruby to be installed on your machine. On most Linux distros and on macOS, it's installed by default, but for Windows you need to install it [manually](https://www.ruby-lang.org/en/).

#### Clone this repo

```
git clone https://github.com/streamroot/dashjs-p2p-wrapper.git
```

#### Install library dependencies

```
npm install
```

#### Build the library

##### Unix

```
npm run build
```

The results will be in the destination folders `dist/wrapper` and `dist/bundle`.

##### Windows

```
scripts\clean.rb
scripts\dashjs.rb
scripts\dist_wrapper.rb
scripts\dist_bundle.rb
```

The results will be in the destination folders `dist\wrapper` and `dist\bundle`.

To build and compile-watch development/debug versions use ```npm run wrapper_dev``` or ```npm run bundle_dev``` for wrapper and bundle respectively.

### Tests

For running unit tests (in node.js), use

```
npm test
```

### Example

#### Bundle

Include the bundle build in your app.

```html
<head>
    <!-- path to dashjs-p2p-bundle build -->
    <script src="dashjs-p2p-bundle.js"></script>
</head>
```

Use the MediaPlayer bundle factory provided by `dashjs-p2p-bundle` to create dash.js player.

```javascript
var p2pConfig = {
    streamrootKey: YOUR_STREAMROOT_KEY_HERE
};

var player = DashjsP2PBundle.MediaPlayer().create(p2pConfig);

var videoElementId = "videoPlayer";
var videoElement = document.getElementById(videoElementId);
var manifestURL = "example.mpd";
var autoStart = true;
player.initialize(videoElement, url, autoStart);
```

#### Wrapper

Include the wrapper build and dash.js build in your app.

```html
<head>
    <!-- path to dashjs-p2p-wrapper build -->
    <script src="dashjs-p2p-wrapper.js"></script>

    <!-- dash.js build of your choice starting from version 2.2.0 and higher -->
    <script src="dash.all.debug.js"></script>
</head>
```

Create a dash.js MediaPlayer instance. Create wrapper instance, passing dash.js player, `p2pConfig` and `liveDelay` value as constructor params. Recommended `liveDelay` value is 30 seconds.

```javascript
var player = dashjs.MediaPlayer().create();

var p2pConfig = {
    streamrootKey: YOUR_STREAMROOT_KEY_HERE
};

var liveDelay = 30;
var dashjsWrapper = new DashjsWrapper(player, p2pConfig, liveDelay);
var videoElementId = "videoPlayer";
var videoElement = document.getElementById(videoElementId);
var manifestURL = "example.mpd";
var autoStart = true;
player.initialize(videoElement, manifestURL, autoStart);
```

To see full sample code and extended possibilities of how to use this module, take a look at the code in the `example` directory.

### Configuration

Specify your `streamrootKey` in the `p2pConfig` object. If you don't have it, go to [Streamroot's dashboard](http://dashboard.streamroot.io/) and sign up. It's free. You can check other `p2pConfig` options in the [documentation](https://streamroot.readme.io/docs/p2p-config).

### Statistics

#### Bundle

No statistics available yet.

#### Wrapper

A `stats` object is available on a `DashjsWrapper` instance and contains the following properties:

- `cdn`: cdn downloaded (cumulated bytes).
- `p2p`: p2p offloaded from cdn (cumulated bytes).
- `upload`: p2p uploaded (cumulated bytes).
- `peers`: real time connected peers count.

### Run demos

Start a local server like `http-server` or `node-static` in the project root, on port 8080 if you like.

Now visit <http://localhost:8080/example/bundle.html> for bundle demo or <http://localhost:8080/example/wrapper.html> for wrapper demo.

To see some p2p traffic open several browser tabs/windows playing the same manifest (so there will be peers to exchange p2p traffic).

### Player integration

The module is integrated in the following players:

* [Video.js 5](https://github.com/videojs/video.js/) through [videojs5-dashjs-p2p-source-handler](https://github.com/streamroot/videojs5-dashjs-p2p-source-handler)
