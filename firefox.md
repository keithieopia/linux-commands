# linux-guides: firefox

<img align="right" alt="Firefox Logo" src="https://raw.githubusercontent.com/keithieopia/linux-guides/master/assets/firefox.png">

> de-bloat and secure firefox, plus add-ons that don't suck

[Firefox](https://www.mozilla.org/en-US/firefox/new/) is far from the perfect browser. This guide will help you setup Firefox with saner defaults, remove bloat, and help put Firefox back in competition with other browser choices like Chromium.

#### Table of Contents
* [General Security](#security)
    * [Firejail](#firejail)
    * [Multiple Profiles](#multi-profiles)
* [Add-ons / Extensions](#addons)
    * [Security](#addons-security)
    * [Ad-block / Tracking](#addons-adblock)
    * [Downloads](#addons-downloads)
    * [Website Development](#addons-webdev)
    * [Miscellaneous](#addons-misc)
    * [Unrecommended](#addons-unrecommended)
* [uBlock Origin Filters](#adblock-filters)
* [about:config Tweaks](#about-config)


## General Security
<a name="security"></a>
No non-sense best practices you can implement now to greatly increase your browser's security and privacy!


### Firejail
<a name="firejail"></a>
I run Firefox under [Firejail](https://firejail.wordpress.com/), which is a
security sandbox that helps protect against browser exploits and zero-day
vulnerabilities. This is similar to how Google Chrome's (and thus Chromium's)
built-in security works. There's hardly any performance hit, and using it is as
simple as running `firejail firefox`. The question then becomes, why not use it?


### Multiple Profiles
<a name="multi-profiles"></a>
It is a good idea to create and use multiple Firefox profiles, but you don't
need it to be complicated. For instance, I use three profiles:

* Home
* Work
* Untrusted

A "untrusted" profile is probably something any Firefox user should consider implementing. Use it to visit new websites or potentially shady links. Consider setting the Privacy settings to erase private data (such as cookies) on close.


### Shameless Plug
If you would benefit from fully encrypted Firefox profiles, the [cryptfox](https://github.com/keithieopia/bin/blob/master/cryptfox) script in my [bin](https://github.com/keithieopia/bin) repo does just that.


## Add-ons / Extensions
<a name="addons"></a>
I highly recommend installing all the add-ons in the "Security" and "Adblock / Tracking" categories. At the minimum, consider reading the "Unrecommended Extensions" and the given rational if you have one of those installed.


### Security
<a name="addons-security"></a>
  * [Ageless](https://addons.mozilla.org/en-US/firefox/addon/ageless/): Bypass
    mandatory login for restricted videos on YouTube. Note that YouTube's
    filtering has gotten ridiculously strict and arbitrary in the last few years.
  * [HTTPS Everywhere](https://addons.mozilla.org/en-US/firefox/addon/https-everywhere/):
    Prevent spying by using HTTPS by default where supported.
  * [NoScript Security Suite](https://addons.mozilla.org/en-US/firefox/addon/noscript/):
    Prevent several types of JS attacks such as XSS. *Note: I deviate from the
    recommended defaults and globally allow scripts*
  * [Decentraleyes](https://addons.mozilla.org/en-US/firefox/addon/decentraleyes/):
    Locally cache common libraries and resources provided by CDNs such as Cloudflare, jQuery, Yandex, and Baidu.
  * [Self-Destructing Cookies](https://addons.mozilla.org/en-US/firefox/addon/self-destructing-cookies/): Auto delete cookies after navigating elsewhere or closing it's tab.


### Ad-blocking / Tracking
<a name="addons-adblock"></a>
  * [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)
    <sup>[filters](#adblock-filters)</sup>:
    Adblocker that's extremely lightweight, fast, and more powerful than "Adblock Plus"
  * [Redirect Bypasser](https://addons.mozilla.org/en-US/firefox/addon/redirectbypasser/):
    Exposes the direct link to a resource when it is encoded or obfuscated in a
    wrapper link (example: href="dubious-website.com?site=good-website.com")
  * [Pure URL](https://addons.mozilla.org/en-US/firefox/addon/pure-url/): Remove
    tracking code attached to links by websites that use Google Adsense.

### Downloads
<a name="addons-downloads"></a>
  * [Flashgot Mass Downloader](https://addons.mozilla.org/en-US/firefox/addon/flashgot/):
    Download media (including flash videos) from websites, even if they're
    semi-obfuscated. Integrates well with DownThemAll! and other managers
  * [DownThemAll!](https://addons.mozilla.org/en-US/firefox/addon/downthemall/):
    A download manager that supports per-site queuing. Can also mass-download  
    resources matching a certain pattern on a web page (like file extension).
  * [DownThemAll! AntiContainer](https://addons.mozilla.org/en-US/firefox/addon/downthemall-anticontainer/):
    Allows DownThemAll! to see hidden resources similar to how Flashgot can. I
    prefer using DownThemAll!'s interface to Flashgot's when possible.


### Website Development
<a name="addons-webdev"></a>
* [Firebug](https://addons.mozilla.org/en-US/firefox/addon/firebug/): An all-in-one
  suite that allows you to edit CSS & HTML and debug JS live in the browser
* [Live HTTP Headers](https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/):
  See the actual protocol traffic exchanged between the browser and a web server


### Miscellaneous
<a name="addons-misc"></a>
  * [Bookmark Deduplicator](https://addons.mozilla.org/en-US/firefox/addon/bookmark-deduplicator/):
    Search and remove duplicate bookmarks
  * [Reddit Enhancement Suite](https://addons.mozilla.org/en-US/firefox/addon/reddit-enhancement-suite/):
    Endless scrolling on [reddit](https://www.reddit.com/) plus a slew of many other
    useful features
  * [Tab Mix Plus](https://addons.mozilla.org/en-US/firefox/addon/tab-mix-plus/):
    Prevent duplicate tabs, remove the blank tab created on file download, and better
    session restore handling.
  * [YouTube ALL HTML5](https://addons.mozilla.org/en-US/firefox/addon/youtube-all-html5/):
    Force YouTube's HTML5 interface (better buffering), set a desired video bitrate,
    and turn off annotations.


### Unrecommended
<a name="addons-unrecommended"></a>
I don't recommend these add-ons due to various reasons, thus they're un-linked to
prevent accidental install:

  * **uBlock**: Not to be confused with "uBlock Origin". The original developer
    of uBlock, looking to semi-retire from the project, gave control to a new
    developer. The new developer did some unscrupulous things, warranting the old
    developer to return to create and maintain "uBlock Origin". Read about it
    [here](http://tuxdiary.com/2015/06/14/ublock-origin/),
    [here](https://www.reddit.com/r/ublock/comments/32mos6/ublock_vs_ublock_origin/),
    and [here](https://www.reddit.com/r/chrome/comments/32ory7/ublock_is_back_under_a_new_name/);
    *warning: needless online drama*
  * **Adblock Plus**: While it use to be the go to about 10 years ago, it has
    since then been monetized by a company, which introduced several
    [controversial design decisions](https://en.wikipedia.org/wiki/Adblock_Plus#Controversy_over_ad_filtering_and_ad_whitelisting).
    Speaking on technical merits, uBlock Origin is more lightweight and has more
    features so you should consider that instead.
  * **Adblock Edge**: Basically the same thing as Adblock Plus, but forked before
    the "Acceptable Ads" addition. It's OK, but still uses slightly more system
    resources than uBlock Origin.
  * **Disconnect**: It's literally [built into](https://support.mozilla.org/en-US/kb/tracking-protection-pbm)
    Firefox now. Disconnect's blocking list is also available by default in
    uBlock Origin.
  * **Ghostery**: There's a lot of online drama about this one, but the gist is
    that it's closed source and owned by an ad company. If you enable the
    optional "Ghost Rank" or anonymized reporting features, they'll collect and
    sell your data (somewhat of a no brainer). Firefox's built-in Disconnect
    basically does the same thing, as well as uBlock Origin.
  * **I don't care about cookies**: There's nothing wrong with this add-on, but
    if you use uBlock Origin, there's a built-in list that does the same thing:
    "*EU: Prebake - Filter Obtrusive Cookie Notices*"

#### A Word on NoScript
Some dislike NoScript due to a old spat the author had with Adblock Plus (EasyList in reality). The author stated he was wrong and apologized on multiple sites. Since there has been no further incident, I found the apology to be sufficient and currently recommend using NoScript.


## uBlock Origin Filters
<a name="adblock-filters"></a>
These are the filters that I have enabled in uBlock Origin, all of them can be
set from within uBlock Origin's Dashboard, under "3rd-party filters".

**Note:** *uBlock does recognize ABP auto-subscribe links, but it's safer and
thus a better practice to enable these filters from within uBlock Origin*

### Ads
  * **uBlock filters**: Default rules for uBlock
  * **uBlock filters - Unbreak**: Don't disable this, it fixes breaks caused by the
    other filters
  * **EasyList**: Adblock Plus's default list
  * **Peter Lowe's Ad and tracking server list**: One of the first lists that used
    the OS's `hosts` file before browser based ad blockers where a thing

### Anti-Adblock
It's ridiculous we've gotten to this point. But here we are:

  * **Adblock Warning Removal List**: Stop sites asking you to turn on your ad block.
  * **Anti-Adblock Killer | Reek**: Slightly more sinister, some sites employ an
    *Anti-Adblock* JS script which completely prevents access their site... this
    is the counter for that.

### Privacy / Trackers
  * **uBlock filters - Privacy**: Default privacy list for uBlock
  * **EasyPrivacy**: Default privacy list for "Adblock Plus"

### Annoyances
  * **Fanboy's Annoyance List**: Unclutters pages that have social media share
    buttons and obtrusive overlay popups
  * **EU: Prebake - Filter Obtrusive Cookie Notices**: Just like it sounds, stop
    those annoying cookie notices mandated by EU law.

### Malware
  * **uBlock filters - Badware risks**
  * **Malware Domain List**
  * **Malware Domains**
  * **Spam404**: Online scams

### Unneeded Filters
  * *Fanboy's Social Blocking List*: all of these rules are included in *Fanboy's
    Annoyance List*

## about:config Tweaks
<a name="about-config"></a>
Improve performance, security, and privacy with these [about:config](http://kb.mozillazine.org/About:config) setting tweaks. Care has been taken to ignore default and obsolete preferences included in similar lists found elsewhere.


| Preference Name                            | Value   | Description                                           |
| ------------------------------------------ | ------- | ------------------------------------------------------|
| beacon.enabled                             | false   | disable Beacon API (privacy)                          |
| browser.cache.use_new_backend              | 1       | enable HTTP cache (performance)                       |
| browser.tabs.remote.autostart              | true    | enable Electrolysis (performance / security)          |
| camera.control.face_detection.enabled      | false   | no. just... *no*. (privacy / bloatware)               |
| datareporting.healthreport.uploadEnabled   | false   | disable sending data to Mozilla (privacy)             |
| datareporting.policy.dataSubmissionEnabled | false   | *same as above*                                       |
| device.sensors.enabled                     | false   | disable Sensors API (privacy)                         |
| dom.battery.enabled                        | false   | disable Battery API (privacy)                         |
| dom.event.clipboardevents.enable           | false   | disable access to clipboard (privacy / security)      |
| extensions.pocket.api                      | *blank* | disable Pocket (bloatware)                            |
| extensions.pocket.enabled                  | false   | *same as above*                                       |
| extensions.pocket.site                     | *blank* | *same as above*                                       |
| geo.enabled                                | false   | disable geo-location (privacy)                        |
| geo.wifi.uri                               | *blank* | *same as above*                                       |
| media.peerconnection.enabled               | false   | disable WebRTC (privacy / security / bloatware)       |
| network.dns.disableIPv6                    | true    | US ISP's IPv6 support is spotty at best (performance) |
| network.dns.disablePrefetch                | true    | disable prefetching (security / privacy)              |
| network.http.pipelining                    | true    | enable HTTP pipelining (performance)                  |
| network.http.pipelining.ssl                | true    | *same as above*                                       |
| network.http.proxy.pipelining              | true    | *same as above*                                       |
| network.http.referer.XOriginPolicy         | 1       | send referrer only if base domain matches (privacy)   |
| network.http.referer.spoofSource           | true    | sent the target page as the referrer                  |
| network.http.referer.trimmingPolicy        | 2       | referrer sends everything but the path                |
| network.http.sendRefererHeader             | 1       | send referrer only on clicked links (privacy)         |
| network.prefetch-next                      | false   | disable prefetching (security / privacy)              |
| privacy.trackingprotection.enabled         | true    | enable built-in tracking protection (privacy)         |
| reader.parse-on-load.enabled               | false   | disable Reader View (bloatware)                       |
| social.directories                         | *blank* | disable social sharing (bloatware)                    |
| social.remote-install.enabled              | false   | *same as above*                                       |
| social.share.activationPanelEnabled        | false   | *same as above*                                       |
| social.shareDirectory                      | *blank* | *same as above*                                       |
| social.toast-notifications.enabled         | false   | *same as above*                                       |
| social.whitelist                           | *blank* | *same as above*                                       |

---

*This is part of the [linux-guides](https://github.com/keithieopia/linux-guides) series. For more information, such see the [README](https://github.com/keithieopia/linux-guides/blob/master/README.md).*
