## xulBlock Advanced (xBA)

xBA is a fork of [uBlock Origin](https://github.com/gorhill/uBlock) (uBO) for browsers that support [XUL](https://en.wikipedia.org/wiki/XUL) add-ons.

I've used xBA on [Pale Moon](http://www.palemoon.org/), [Basilisk](http://www.basilisk-browser.org/), and [Waterfox](https://www.waterfoxproject.org/). It should work on other browsers too.

### xBA is only for [Advanced user mode](https://github.com/gorhill/uBlock/wiki/Advanced-user-features)

One purpose of xBA is to enhance the Advanced user experience, most prominently with changes to the panel and the ability to spoof Referers. These and other changes are covered in detail below.

xBA is not just for current Advanced users, though. It's also a good choice for regular uBO users with a desire to learn Advanced mode. In particular, xBA enables you to quickly view all domain connectivity, which will facilitate the learning process.

## Issues
* If you suspect an xBA bug, check the [Known problems](https://github.com/joey04/xulBlock-Advanced/wiki/Known-problems) wiki page and the issue tracker to see if it's already documented.
* Read [CONTRIBUTING](https://github.com/joey04/xulBlock-Advanced/blob/master/CONTRIBUTING.md) before filing any issue here.

As an Advanced user, you are expected to figure out all website breakage by yourself; don't ask for help here. If the cause is a 3rd-party filter list problem, contact the list maintainer.

## Table of Contents
* [Why this fork](#why-this-fork)
* [What's different from uBO](#whats-different-from-ubo)
  * [Referer spoofing](#http-referer-spoofing)
  * [The Panel](#the-panel)
  * [Hotkeys](#hotkeys)
  * [Popup blocking](#blocking-popups)
  * [CSP Report blocking](#blocking-csp-reports)
  * [Other changes](#other-changes)
* [How to Install it](#how-to-install-it)

## Why this fork
I mentioned earlier that one purpose of xBA is to enhance the Advanced user experience. Another purpose is to maintain the greatness that uBO already attained as a XUL add-on. I personally want to keep using xBA for as long as there are browsers that support it. After all, I use it along with a dozen other XUL add-ons to enhance my overall web experience. I have no interest in downgrading to WebExtensions or similar for my primary browser.

I first modified uBO in 2016 to improve my own Advanced usage, but I didn't think to make a publicly-available fork until January 2018. By then, I'd already accumulated a number of changes and Mozilla had recently removed its support of XUL add-ons in Firefox 57. Starting around mid-2017, gorhill's focus shifted to the new WebExt version of uBO for Firefox, which is understandable for a number of reasons, including WebExt having much in common with the Chrome version. As a result, XUL uBO was effectively mothballed, as shown [here](https://github.com/gorhill/uBlock/wiki/Firefox-WebExtensions/1f950bc8d0bfcd55b281549b89e102575924c0ba#future-of-ubolegacy), [here](https://github.com/gorhill/uBlock/issues/3306), and [here](https://github.com/gorhill/uBlock/issues/3464).

This is why [uBO 1.14.16](https://github.com/gorhill/uBlock/releases/tag/1.14.16), released in October 2017, was an easy choice as the fork point for xBA:
* It's the last version before major refactoring started to land and cause XUL regressions. Prior to the first release of xBA, I used my modified 1.14.16 for three months without any problems. It's a stable, reliable codebase.
* uBO had already reached a very high level of robustness with an impressive suite of capabilities: dynamic, static, cosmetic, inject, etc. Any scenario I encountered could be handled with the appropriate choice of rules.

## What's different from uBO
There are important changes in xBA, but the _vast majority_ of behavior remains unchanged, especially from the fork point of [uBO 1.14.16](https://github.com/gorhill/uBlock/releases/tag/1.14.16).

### HTTP Referer spoofing
This is the one feature of uMatrix I wanted that wasn't already in uBO, so I added it. See [its wiki page](https://github.com/joey04/xulBlock-Advanced/wiki/HTTP-Referer-spoofing) for all info.

### The Panel
xBA's Panel has significant changes, all described in [its wiki page](https://github.com/joey04/xulBlock-Advanced/wiki/The-Panel). Be sure to read it before starting out on xBA.

### Hotkeys
* `Alt+P` for the Panel, as described [here](https://github.com/joey04/xulBlock-Advanced/wiki/The-Panel#hotkey-access)
* `Ctrl+Shift+L` to open the Logger for the current tab

There is no option to change these bindings; for that you need a separate hotkey manager extension, e.g. Menu Wizard.

### Blocking popups
By default, xBA uses the uBO behavior for popup blocking rules: a newly opened popup window must have a different URL from its opener before uBO will filter it. I added an [Advanced setting](https://github.com/joey04/xulBlock-Advanced/wiki/Advanced-settings) to allow a second mode of behavior: close the newly opened popup window as soon as possible regardless of its URL. See [here](https://github.com/gorhill/uBlock/issues/3133) for a discussion of this.

`mustWaitForPopupURLchange` is the setting. It's `true` by default, so set it to `false` to get the new behavior.

**Important note:** Only `*$popup,domain=` rules benefit from the new mode, whereas `no-popups` per-domain rules remain stuck on the default mode. (See [here](https://github.com/gorhill/uBlock/issues/3164#issuecomment-338967952) for a brief explanation why.) Thus for my own xBA usage, I have a `*$popup,domain=a|b|c` rule for the sites I want to block them without an initial connection to a 3rd-party server.

### Blocking CSP Reports
[CSP Report blocking](https://github.com/gorhill/uBlock/wiki/Dashboard:-Settings#block-csp-reports) was refactored for uBO 1.14.18. But xBA doesn't need this, as it already can block any CSP Report with either a type `$other` rule or a URL pattern-matching rule.

I block all of them with: `*$other,domain=~behind-the-scene`

I've done this for a long time in Pale Moon without any problems. CSP Reports have been virtually all of the non-bts `$other` traffic I've seen; the few non-CSPs appear to be stray requests worth blocking anyways. However, behind-the-scene (bts) is a completely different story; I don't whitelist it, so I quickly learned I needed the exception to not block essential requests like file downloads.

### Other changes
xBA enforces a strict word-boundary policy for prefix1 rules, as detailed [here](https://github.com/gorhill/uBlock/issues/3011).

xBA's SQLite database is located in your profile base directory, not uBO's sub-directory. The  filename is the same.

## How to Install it
If you have uBO installed, either disable or remove it before installing xBA. Don't run uBO and xBA at the same time because strange things may happen. (Likewise, disable AdBlock Plus or any other blocker extension. Only use one blocker at a time.)

Download the `xulBlock-Advanced.xpi` file from the [Releases page](https://github.com/joey04/xulBlock-Advanced/releases). Then drag the xpi file into the browser window. The install dialog should appear. If that's not working, open the `about:addons` page (Tools->Add-ons) then drag it into that window.

I recommend exiting or rebooting the browser after the install. A fresh browser session is the only true guarantee of a clean start.

### Migrating uBO settings
It should be as simple as exporting all of your uBO settings in the Settings tab of the Dashboard then importing this text file into xBA.

If you run into problems, though, a second option is to save your filters and rules as text files in the My filters and My rules Dashboard tabs. Then import those to the same tabs in xBA. You'll have to manually set everything else, but that's not a big deal.
