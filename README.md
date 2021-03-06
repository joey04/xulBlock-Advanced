## xulBlock Advanced (xBA)

xBA is a fork of [uBlock Origin](https://github.com/gorhill/uBlock) (uBO) for browsers that support [XUL](https://en.wikipedia.org/wiki/XUL) add-ons.

I've used xBA on [Pale Moon](http://www.palemoon.org/), [Basilisk](http://www.basilisk-browser.org/), and [Waterfox](https://www.waterfoxproject.org/). It should work on other browsers too.

### xBA is only for [Advanced user mode](https://github.com/gorhill/uBlock/wiki/Advanced-user-features)

One purpose of xBA is to enhance the Advanced user experience, with a [panel](https://github.com/joey04/xulBlock-Advanced/wiki/The-Panel) designed to quickly assess all domain connectivity.

xBA is not just for current Advanced users, though. It's also a good choice for regular uBO users with a serious desire to learn Advanced mode.

### Issues
As an Advanced user, you are expected to figure out all website breakage by yourself; don't ask for help here. If the cause is a 3rd-party filter list problem, contact the list maintainer.

Read [CONTRIBUTING](https://github.com/joey04/xulBlock-Advanced/blob/master/CONTRIBUTING.md) before filing any issue here.

## Table of Contents
* [Why this fork](#why-this-fork)
* [What's different from uBO](#whats-different-from-ubo)
  * [Referer spoofing](#http-referer-spoofing)
  * [The Panel](#the-panel)
  * [Hotkeys](#hotkeys)
  * [Popup blocking](#blocking-popups)
  * [CSP Report blocking](#blocking-csp-reports)
  * [Other differences](#other-differences)
* [How to Install it](#how-to-install-it)

## Why this fork
I mentioned earlier that one purpose of xBA is to enhance the Advanced user experience. Its other purpose is to maintain the high quality that uBO already attained as a XUL add-on.

I first modified uBO in 2016 to improve my own Advanced usage, but I didn't think to make a publicly-available fork until January 2018. By then, I'd accumulated a number of changes and Mozilla had recently removed its support of XUL add-ons in Firefox 57. Starting around mid-2017, gorhill's focus shifted to the new WebExtension version of uBO for Firefox, which is understandable for a number of reasons, including WebExt having much in common with the Chrome version. As a result, the legacy XUL version was effectively mothballed, as shown [here](https://github.com/gorhill/uBlock/wiki/Firefox-WebExtensions/1f950bc8d0bfcd55b281549b89e102575924c0ba#future-of-ubolegacy), [here](https://github.com/gorhill/uBlock/issues/3306), and [here](https://github.com/gorhill/uBlock/issues/3464).

This is why [uBO 1.14.16](https://github.com/gorhill/uBlock/releases/tag/1.14.16), released in October 2017, was an easy choice as the fork point for xBA.
* It's the last version before major refactoring started to land and cause XUL regressions.
* uBO had already reached a high level of quality with a robust suite of capabilities: dynamic, static, cosmetic, inject, etc.

Prior to releasing xBA, I used my modified 1.14.16 for three months in Pale Moon without any problems. It's a stable, reliable codebase.

## What's different from uBO
This section covers all the ways that xBA differs from uBO.

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

### Other differences
xBA enforces a strict word-boundary policy for prefix1 rules, as detailed [here](https://github.com/gorhill/uBlock/issues/3011).

xBA's SQLite database is located in your profile base directory, not uBO's sub-directory. The filename is the same.

Some minor items are covered in the [issue tracker](https://github.com/joey04/xulBlock-Advanced/issues?q=is%3Aissue).

## How to Install it
If you have uBO installed, either disable or remove it before installing xBA. Don't run uBO and xBA at the same time because strange things may happen. (Likewise, disable AdBlock Plus or any other blocker extension. Only use one blocker at a time.)

Download the `xulBlock-Advanced.xpi` file from the [Releases page](https://github.com/joey04/xulBlock-Advanced/releases). Then drag the xpi file into the browser window. The install dialog should appear. If that's not working, open the `about:addons` page (Tools->Add-ons) then drag it into that window.

I recommend exiting or rebooting the browser after the install. A fresh browser session is the only true guarantee of a clean start.

### Migrating uBO settings
It should be as simple as exporting all of your uBO settings in the Settings tab of the Dashboard then importing this text file into xBA.

If you run into problems, though, a second option is to save your filters and rules as text files in the My filters and My rules Dashboard tabs. Then import those to the same tabs in xBA. You'll have to manually set everything else, but that's not a big deal.
