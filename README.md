# Joplin Web App

This repository deploys the Joplin web client to https://app.joplincloud.com/ with GitHub pages.

The web app source code can be found in [the main Joplin repository](https://github.com/laurent22/joplin).


## FAQ

### What is it?

Joplin Web is Joplin Mobile, running in a web browser.

### Where is my data stored?

Like Joplin Mobile, Joplin Web is local-first. Notes and attachments are stored locally on your computer, but can optionally be synced with one of the supported sync targets. As a result, Joplin Web can be used offline.

### What browsers does it support?

The Joplin web app works best in recent versions of Chrome and Safari. It can also be used in Firefox, however, it may take a very long time to start.

Some features are available only on certain platforms:
- File system sync[^1] (as of July 2024):
	- ✅ Chrome (desktop)
	- ❌ Chrome (Android)
	- ❌ Safari
	- ❌ Firefox
- Share note content[^2] (as of July 2024):
	- ✅ Safari
	- ✅ Chrome (Android)
	- ❌ Chrome (Desktop)
	- ❌ Firefox
- Insert images from a camera (as of July 2024):
	- ✅ Safari
	- ✅ Chrome (Android)
	- ❌ Chrome (Desktop)
	- ❌ Firefox
- Drop images and files from another app (as of July 2024):
	- ✅ Chrome (Desktop), Safari, Firefox (Desktop)
	- ❌ Chrome (Android)


[^1]: Requires [support for showDirectoryPicker](https://caniuse.com/?search=showDirectoryPicker).
[^2]: Requires [Web Share API support](https://caniuse.com/?search=web%20share%20api).



## Self-Hosting & Joplin Server Sync

If you deploy this web app to your own domain via GitHub Pages (or another static host), the sync target restriction normally placed on `app.joplincloud.com` is automatically lifted. You will be able to select **Joplin Server** as a sync target.

However, your web browser will block the connection unless your Joplin Server allows **CORS** (Cross-Origin Resource Sharing) for your web app's domain.

### How to allow CORS for your Web App

**Option A: The Environment Variable (Recommended)**
The simplest way to allow your web app to connect is to set the `USER_CONTENT_BASE_URL` environment variable on your Joplin Server to match your web app's domain. 

For example, if your web app is hosted at `web.yourdomain.com`:
```env
USER_CONTENT_BASE_URL=https://yourdomain.com
```
*Note: Joplin Server natively allows CORS for any subdomain of the `USER_CONTENT_BASE_URL`. Be aware that setting this variable will change the URLs generated when you use the "Publish Note" / sharing feature in Joplin.*

**Option B: Reverse Proxy Headers**
If you cannot use the environment variable, you must configure the reverse proxy sitting in front of your Joplin Server (e.g., Traefik, Nginx, Caddy) to forcefully inject the CORS headers into the response.

Example required headers:
```http
Access-Control-Allow-Origin: https://web.yourdomain.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, PATCH, OPTIONS
Access-Control-Allow-Headers: *
```
*(If you are using Traefik, it is highly recommended to use the dedicated `accessControlAllowOriginList` CORS middleware to properly intercept browser `OPTIONS` preflight requests).*
