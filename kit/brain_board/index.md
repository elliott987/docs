---
layout: page
title: Brain Board
---

{% comment %}
The below will automatically calculate the latest version number, to be used when mentioning the version in this page.
{% endcomment %}

{% assign latest_version = site.data.kit_versions | first %}
{% assign latest_version = latest_version.version %}

Brain Board
===========

<img src="{{ site.baseurl }}/images/content/kit/brain.png" alt="A photo of a brain board" title="A brain board" class="right" style="max-width: 50%"/>
The Brain Board provided with your kit is what runs the code you write and controls the other peripheral boards. It consists of a [Raspberry Pi 4B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/) and a Student Robotics KCH HAT.

The LEDs on the HAT display the current status of the robot and can be used to help debug your robot. There are also 3 RGB (Red, Green, and Blue) LEDs that you can control from your code.

## Board Diagram

![brain board assembly diagram]({{ site.baseurl }}/images/content/kit/brain-diagram.png "The Brain Board Assembly")

### Indicators

| LED              | Meaning
|------------------|-------------------------
| 5V Power         | The Brain Board is powered
| 12V Power        | The KCH is powered
| Reverse Polarity | The 12V power is reversed
| Boot Progress    | Progress Bar for Brain Boot Progress
| Code             | A USB containing code is plugged in
| Comp             | The Robot is in Competition Mode
| WiFi             | The Robot WiFi hotspot is running
| ♥ (Heartbeat)    | Blinks when the Brain is running
| Start            | The Robot is waiting to start
| OK               | Shows the code status, see below table

### OK LED

The OK LED shows the status of your code using different colours.

| OK LED Colour | Meaning
|---------------|--------------------------
| Off           | No code available
| Cyan          | Your code is starting
| Yellow        | Your code is running
| Magenta       | Your code has been killed
| Green         | Your code has finished without errors
| Red           | Your code has crashed

## Powering the Brain Board

Your Brain Board will not power on unless it's connected to the "L2" port on the Power Board.
This is because the rest of the power outputs are disabled until the usercode runs.

## Flashing the SD card

The SD card is located on the underside of the board underneath the green power connector. Grab the SD card with your fingers and simply pull it out of the slot.

To fully update your Brain Board's software, or refresh it if you think it's not working correctly, you can flash our SD card image onto the microSD card in your Brain Board.

To update the SD card, you'll need to download our image from the [updates page]({{ site.baseurl }}/kit/brain_board/robot_os). The latest version is `{{ latest_version }}`.
The flashing procedure is identical to flashing Raspberry Pi images.

### Etcher

We recommend using [Etcher](https://etcher.io), as it's simple to use, and available on Windows, macOS and Linux. If you're familiar with Raspberry Pis or other similar boards and have flashed images before with a different tool, that should also work.

![Etcher example]({{ site.baseurl }}/images/content/kit/etcher.png)

<div class="info" markdown="1">
If you choose to use a tool other than Etcher, you may need to extract the `srobo-robot-{{ latest_version }}.img.xz` to `srobo-robot-{{ latest_version }}.img`. There are many tools available for this, e.g. [7-zip](https://www.7-zip.org/).
</div>

### Flashing

1. Open Etcher and select the `srobo-robot-{{ latest_version }}.img.xz` file you downloaded
2. Select your SD card from the devices window
3. Click 'Flash!'
4. When the flash is complete you should safely eject the SD card.<br>
   Your computer may complain that the SD card is no longer readable, however
   this is expected as the data being written to the SD card is not in a format
   that either Windows or macOS can natively understand.

## Robot Settings

Some of the features on your robot are configured using a settings file, called `robot-settings.toml`. This file is automatically created on your USB drive the first time that you run any code on your robot.

You can edit the settings file using your IDE or any text editor.

The robot settings file contains the following settings:

| Setting Name          | Description                                | Default Value      |
|-----------------------|--------------------------------------------|--------------------|
| `team_tla`            | Three Letter Acronym (TLA) for your team.  | Randomly generated |
| `wifi_psk`            | Password for the Robot WiFi                | Randomly generated |
| `wifi_region`         | Region Identifier for the WiFi             | `GB`               |
| `wifi_enabled`        | Enables the WiFi                           | `true`             |
| `usercode_entrypoint` | The entry point to your Python code        | `robot.py`         |

If your `robot-settings.toml` is not valid, it will be automatically regenerated with valid settings. This will reset any settings you have changed back to their default values. We therefore recommend that you do not change your settings file before a competition match.

In software version `2023.1.0` and later, an error file will be generated if your settings are invalid: `robot-settings-error.txt`. This file contains a message explaining the problem and a copy of your old settings.


## Firewall

<div class="info">
This documentation refers to a feature which is only available from software version `2023.1.0` and later.
</div>

The Brain Board has a [network firewall](https://en.wikipedia.org/wiki/Firewall_(computing)) that prevents access to applications and services on the Brain Board unless it is required.

If you are running code that uses networking, you will need to use a port from the range that is allocated for competitor use:

| Protocol | Start of Range | End of Range |
|----------|----------------|--------------|
| TCP      | 7000           | 8999         |
| UDP      | 7000           | 8999         |
