---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: Shutterfly and Mac OS X Extensions
datePublished: '2016-03-18T21:57:55.607Z'
dateModified: '2016-03-18T21:57:43.477Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2016-03-18-shutterfly-and-mac-os-x-extensions.md
published: true
url: shutterfly-and-mac-os-x-extensions/index.html
_type: Article

---
Shutterfly and Mac OS X Extensions
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/8df439ae-832d-492e-a03c-3e5bb9bcdd8b.jpg)

Recently, a client asked about the Shutterfly Extension for Apple's new Photos app- on some albums, the extension would not appear under the 'Share' menu, whether under the File menu, via contextual menu, or via the share icon in the upper right corner of Photos.

When selecting individual photos, it invariably appeared.

The hypothesis then became that some conditional was being evaluated to determine when the extension should or should not be shown. 

Pulling apart the ShutterflyShareApp Extension, we found there was a key inside one of the plist files that limited the extension to only activating if there were 100 or less photos selected. This was quickly confirmed by testing within Photos that as soon as one selected the 101st photo, the extension did not appear.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/fa6c435f-f2a5-471a-816d-79c9282bde90.png)

We changed this key to 500, and upon restarting Photos, the extension was present for albums with between 100 and 500 photos.

Case closed. Thank you to Michael Hofer for his eagle eyes, and Daniel Mulroy for the hypothesis.