+++
date = 2021-08-19T11:30:00Z
description = "Layout Validation is a tool using which you can validate your layouts while you're building them. While you can already see your current UI on several devices using the Android Studio's visual layout editor, Layout Validation allows you to check all those presets simultaneously and with some pretty sick features. "
draft = true
keywords = ["Android", "Android Studio", "Layout Validation"]
math = false
slug = "validate-layout-android-studio"
tags = ["android", "android-studio"]
title = "Validating your layouts ft. Android Studio"
toc = true

+++
When was the last time you had to fix your screen for a bigger font size? The screen doesn't look good on dark mode? or maybe you forgot to take landscape mode into account. These are some bugs that are especially common for me and maybe for a lot of other developers. Fret not! we've got Android Studio to the rescue :)

## Layout Validation

Layout Validation is a tool using which you can validate your layouts while you're building them. While you can already see your current UI on several devices using the Android Studio's visual layout editor, Layout Validation allows you to check all those presets simultaneously and with some pretty sick features. 

Before we move ahead and discuss some of those features, I just wanna let you guys know that Layout Validation is available on Android Studio 4.0+ and can be opened with the Layout Validation tab on the top right or by going to Tools > View > Layout Validation after you've opened your XML file.

## Features

Layout Validation supports a ton of features that are useful for day-to-day UI development. We're gonna discuss them below 👇

### Pixel Devices

This section allows you to check your UI on a large subset of Pixel devices having different screen sizes and resolutions. These are configured with default presets so you can get started ASAP.

![](/uploads/pixel_devices.png)

### Wear OS Devices

This section contains three Wear OS configurations for which you can check your UIs.

![](/uploads/wear_os.png)

### Project Locales

This section allows you to see your UI in all the locales that are available in your application. This would be particularly interesting for anyone who works with a lot of locale-specific apps where you would want to check how the UI looks in all the languages.

![](/uploads/locales.png)

### Custom

This section allows you to create custom presets. This is particularly helpful as you can create a set of devices ranging from different screen sizes to different locales and API levels. For example, you can have a Pixel 1 with a 1080p display and a Nexus 4 with a 720p display, and those can represent a broader range of devices from their generation.

![](/uploads/custom.png)

### Color Blind

This was the most exciting and unique thing for me when I first learned about the Layout Validation tool. Although designers work very hard to make sure that the screen is accessible for everyone, there can be times when they miss out. Color Blind section can help you make sure that you're shipping a product that looks good for everyone.

![](/uploads/color_blind.png)

### Font Sizes

In the end, we have the Font Sizes section, which can help us see how the UI will look when the user has changed the system font size. This can be quite helpful if your app targets an older demographic since they may increase the system font size for reading pleasure.

![](/uploads/font_sizes.png)

## Conclusion

I believe that the Layout Validation tool can reduce the time spent on validating and fixing the layouts by a significant amount. It can also help in making our apps more resilient and accessible. If you want to read more about the Layout Validation tool you can head over to [this page on the Android Developers website](https://developer.android.com/studio/debug/layout-inspector#layout-validation).