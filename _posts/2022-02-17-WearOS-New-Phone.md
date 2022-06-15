---
title: WearOS Pair to New Phone Without Reset
tags: WearOS Android
article_header:
  type: cover
  image: 
    src: /assets/images/2015/11/background_v2-16.png
license: false
---

I was not happy the default procedure to change phones was a complete reset.  I found [this](https://www.androidpolice.com/how-to-pair-your-wear-os-watch-with-another-phone-without-resetting-it):

In summary:

-   Enable developer mode
-   Enable ADB Debugging via wifi
-   Find the IP address

```console
IP="192.168.0.15:5555"
adb connect ${IP}
adb shell "pm clear com.google.android.gms && reboot"
adb connect ${IP}
adb shell am start -a android.bluetooth.adapter.action.REQUEST_DISCOVERABLE
```

The connect via the WearOS app on the new phone.
