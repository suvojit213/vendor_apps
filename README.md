---

Custom App Integration for Android Build System

This guide covers how to integrate custom apps, like BraveBrowser and Innertune, into the Android build system. It includes setting up the required build files (Android.bp, device.mk, and apps.mk) for adding custom APKs into your Android project.


---

Table of Contents

1. Prerequisites


2. Step 1: Configuring Android.bp


3. Step 2: Updating device.mk


4. Step 3: Modifying apps.mk


5. Step 4: Building the Project


6. Adding More Apps


7. Troubleshooting


8. Conclusion




---

Prerequisites

Before starting, ensure you have:

Android Source Setup: A working Android source tree with the Soong build system.

APK Files: Place BraveBrowser.apk and Innertune.apk in the vendor/apps directory.

Prebuilt APK Handling: Ensure APKs are signed with signature v2. The preprocessed: true setting in Android.bp handles presigned APKs.



---

Step 1: Configuring Android.bp

In Android.bp, define the build rules for each custom app as shown below:

soong_namespace {
    name: "custom_apps",
}

android_app_import {
    name: "BraveBrowser",
    apk: "BraveBrowser.apk",
    presigned: true,
    preprocessed: true,
    dex_preopt: { enabled: false },
    product_specific: true,
}

android_app_import {
    name: "Innertune",
    apk: "Innertune.apk",
    presigned: true,
    preprocessed: true,
    dex_preopt: { enabled: false },
    product_specific: true,
}

> Use preprocessed: true to handle v2 signatures for prebuilt APKs.




---

Step 2: Updating device.mk

To include the custom apps, update device.mk by adding:

$(call inherit-product, vendor/apps/apps.mk)

This links apps.mk to device.mk, enabling the custom apps to be part of the build process.


---

Step 3: Modifying apps.mk

List the custom apps in apps.mk to confirm their inclusion in the build:

PRODUCT_PACKAGES += \
    BraveBrowser \
    Innertune

Listing these apps in PRODUCT_PACKAGES ensures they are picked up during the build.


---

Step 4: Building the Project

Once the configurations are complete, start the build with your preferred command, such as:

m bacon

or make. This compiles the Android project, including the custom apps.


---

Adding More Apps

To integrate additional apps:

1. Define in Android.bp: Add a new android_app_import block for each app, specifying its properties.

android_app_import {
    name: "NewApp",
    apk: "NewApp.apk",
    presigned: true,
    preprocessed: true,
    dex_preopt: { enabled: false },
    product_specific: true,
}


2. Update apps.mk: Include the new app in PRODUCT_PACKAGES.

PRODUCT_PACKAGES += \
    NewApp


3. Place APK: Ensure the APK is in the vendor/apps directory.



Following these steps will include any additional apps in the build configuration.


---

Troubleshooting

Incorrect APK Paths: Double-check APK paths in Android.bp.

Missing Module Error: Ensure each app is listed in PRODUCT_PACKAGES in apps.mk.

Signature Mismatch: Verify that APKs are signed. If they aren’t, set presigned: false.

Build Errors: Review for correct syntax in Android.bp, device.mk, and apps.mk.



---

Conclusion

By following this guide, you can seamlessly integrate and build custom apps in the Android environment, ensuring they are part of the final system image.
