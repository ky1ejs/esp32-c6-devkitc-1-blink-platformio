# ESP32-C6-DevKitC-1 Blink Example

I've had quite a bit of trouble building for the ESP32-C6-DevKitC-1 using either the ESP-IDF or PlatformIO. This repository contains a minimal example that should work with PlatformIO.

## The issues
### ESP-IDF
I could get ESP-IDF to build, flash and monitor the code just fine. But when it came to adding Arduino code as per the guidance for Arduino Core as an ESP-IDF component, I couldn't figure out how to get things to compile and include in my main.cpp.

The other thing I disliked about ESP-IDF is that if I wanted to add arduino libs, I had to download them by hand and then overwrite their CMakeLists.txt files to work with ESP-IDF. 

Lastly, ESP-IDF was extremely slow to build and would frequently build from scratch.

### PlatformIO
I followed the vanila set up guide for PlatformIO, but whenever I flashed a basic program onto my board, I'd get an error like: 

```
E (74) esp_image: Failed to fetch app description header!
E (79) boot: Factory app partition is not bootable
E (83) boot: No bootable app partitions in the partition table
```

I hit my head agains this problem for hours... until I found these two links on the internet:
* https://community.platformio.org/t/esp32-c6-espidf-error-when-using-components-folder-led-strip-h/37006/3
* https://github.com/platformio/platform-espressif32/issues/1243


[This reply here specfically](https://github.com/platformio/platform-espressif32/issues/1243#issuecomment-1836305435) said to point the platformio.ini platform option to a [specifc repo](https://github.com/tasmota/platform-espressif32/releases/). I did this, and it worked! I then reverted the platform field back to the default `espressif32` and it still worked. I'm not sure why it continued to work; I have two theories: either there's something cached on my machine now as a result of the first build, or the changes that using the other platform made to the sdkconfig fixed the set up... I have noticed that the default sdkconfig was asserting that the ESP32-C6 had 2MB of flash, but the ESP32-C6-DevKitC-1 has 8MB of flash... so maybe the default just has some things that are out of sync.

# Arduino
If you wanna get Arduino running, apparently something like this is the way:
https://github.com/platformio/platform-espressif32/issues/1225#issuecomment-1977825930