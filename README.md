# Flashing ESPHome onto Shelly Plus Plug S V2

This guide will walk you through the steps necessary for flashing ESPHome onto a Shelly Plus Plug S V2.

# :warning: WARNING! :warning:
People have reported, that flashing Tasmota to Shellys running Firmware version v1.4.2 get bricked. If possible, do NOT upgrade the shelly firmware to 1.4.0 or above. 1.3.3 has been reported to work for converting to Tasmota. But devices running Shelly firmware <1.4.0 get bricked when trying to flash Tasmota versions >= 14.0.0. Tasmota version 13.4.1 was reported to work [https://github.com/kadam12g/ESPHome-Shelly-Plus-Plug-S/issues/1](here) by @datenzar. (Thanks again!) Both the Shelly and Tasmota firmware can be found in the files folder. The shelly firmware was not obtained from Shelly, and I did not test it, so use it at your own risk. If your device gets bricked after flashing Tasmota, it may be possible to recover it using the method described [https://github.com/tasmota/mgos32-to-tasmota32/issues/75#issuecomment-2379974905](here).

### Important Notes Before You Begin:
1. **Internet Access Required:** The Shelly Plug needs internet access during the initial setup to update the factory firmware to the latest version. As of now, there's no workaround for this requirement, which is generally not problematic for most setups.

2. **Network Configuration:** This guide assumes the presence of a dedicated IoT network where your Home Assistant instance and other IoT devices reside. The Shelly Plug will be configured to connect to this network. 
   - If you have a **single network** setup, you can use your primary network for all steps without issue.
   - If you use **multiple networks** or have a different setup, you'll need to adjust the instructions to fit your specific network configuration.

### Disclaimer and Liability
- **Warranty Information:** Flashing custom firmware like ESPHome may **void your device’s warranty**. Proceed with caution and understand that while this guide aims to provide detailed instructions, firmware updates can potentially lead to device malfunctions or voiding of warranties.
- **Risk of Device Bricking:** While I have made every effort to ensure the accuracy and effectiveness of this guide, there is always a risk associated with flashing firmware that could result in "bricking" your device, making it non-functional. **I am not responsible for any damage or loss** that may occur as a result of following this guide. Please proceed at your own risk.
- **Intent of This Guide:** This guide is created to assist others in integrating their Shelly Plus Plug S with ESPHome to the best of my knowledge and abilities. It is intended as a helpful resource for the ESPHome and HomeAssistant community.

### Additional Resources and Alternative Guides

While this guide provides a detailed, step-by-step approach to flashing ESPHome onto the Shelly Plus Plug S V2, experienced users may prefer a more streamlined set of instructions. For those familiar with device flashing and looking for less granularity:

- **Tasmota GitHub Guide**: The steps outlined in the [Tasmota GitHub repository](https://github.com/tasmota/mgos32-to-tasmota32) provide a concise version of the Tasmota flashing process. This guide is suitable for users who are comfortable with firmware updates and may not need detailed explanations at each step. If you choose to follow the instructions on the Github, you can continue with this guide from step 20.

- **Video Tutorial**: For a visual walkthrough, you can refer to the detailed video guide available [here](https://www.youtube.com/watch?v=_Xv1U5Fz2Y4), which covers the Tasmota flashing process. If you are done with the steps showed in the video, you can continue with this guide from step 20.


# Step-by-Step Guide:
1. **Power Up the Device**: Plug in your Shelly Plug.

2. **Access the Device's AP**: Connect to the Access Point (AP) automatically created by the Shelly Plug.

3. **Configure Wi-Fi**:
   - Navigate to `http://192.168.33.1`.
   - Click on **Configure Wi-Fi settings**.
   - Ensure **Enable WiFi network** is checked.
   - Connect it to the same network your computer is on, which should also have internet access.

   **Resetting Network Settings**:
   - If you need to change the network settings, press and hold the button on the plug for 10 seconds within the first 60 seconds after plugging it in. The red lights will blink faster once reset. The AP will then reappear allowing you to reconfigure the network settings.

4. **Network Connection Confirmation**:
   - The plug should flash red and then blue, indicating a successful connection to the specified network.

5. **Identify Device IP Address**:
   - The IP address assigned to your Shelly Plug within your network will be displayed on the page where you entered the Wi-Fi credentials. Note this IP address for future access, especially since the initial AP may not spawn after a reboot. Alternatively, you can locate this IP address in your router's device list later.

6. **Navigate to Firmware Settings**:
   - Access the settings by navigating to **Settings > Firmware** on the device's web interface.

7. **Firmware Update Caution**:
   - **Important**: Do not upgrade to firmware versions >=1.10, as these do not support OTA updates and will make it impossible to continue with this guide. If prompted, update only to the recommended stable version that supports OTA. For example, version 1.3.0 is acceptable.

8. **Perform Firmware Update**:
   - Click on **Update to stable** if the suggested version is appropriate. After the update completes, click **Refresh page now** or manually navigate to the device's IP address on your network.

9. **Power Cycle the Device**:
   - To ensure the firmware update is properly applied, unplug the device and plug it back in. This step helps confirm that the latest firmware persists.

10. **Download Firmware**:
    - Visit the [Tasmota Releases page](https://github.com/tasmota/mgos32-to-tasmota32/releases) and download the latest zip file ending in `PlusPlugS`. You may need to expand the list by clicking on `show more`.
11. **Prepare for Firmware Update**:
    - Return to the device's web interface and navigate to **Settings > Firmware**.
    - Verify that the firmware version displayed at the bottom of the page is `1.x.x`, where `x.x` is less than `10.0`.

12. **Upload and Install Firmware**:
    - Drag and drop or use the file selector to upload the zip file you downloaded.
    - Click on **Update from file** to start the firmware update process.

13. **Monitor for Tasmota AP**:
    - After updating, a Tasmota Wi-Fi access point (AP) should appear. If the device reboots back into the Shelly firmware instead, repeat steps 11 and 12 until the Tasmota AP appears.

14. **Connect to Tasmota AP**:
    - Join the Tasmota Wi-Fi network created by the device.

15. **Configure Wi-Fi Settings**:
    - If the captive portal doesn't automatically open, manually navigate to `http://192.168.4.1/`.
    - Configure the device to connect to the same network your computer is on, ensuring it has internet access. Click **Save**.

16. **Access Device on New Network**:
    - The device should automatically redirect you to its new IP address. If it doesn’t, verify it has connected to your network and manually access the device via its IP address.

17. **Crucial Configuration to Avoid Bricking**:
    - **Important:** Failure to perform this step may brick the device if it loses power.
    - In the web interface, go to **Configuration > Auto-configuration**.
    - Select the correct hardware profile (Shelly Plus Plug S), click **Apply configuration**, and confirm in the popup dialog.

18. **Initiate Partition Migration**:
    - Navigate to `Consoles` > `Partition Wizard` > `Start Migration`.
    - Do not interact with the device for at least 3 minutes to ensure the process completes uninterrupted.

19. **Optional: Extend Filesystem**:
    - To maximize storage space for future firmware updates, consider resizing the filesystem.
    - Go to `Tools` > `Partition Wizard` > `Resize FS To Max`.

20. **Transition to ESPHome**:
    - **Warning:** You now have the latest Tasmota firmware installed. If you are comfortable with Tasmota and it meets your needs, consider continuing with it as switching to ESPHome is considerably more complex. Read the rest of the guide before attempting to proceed.
    - If you specifically require ESPHome and are confident in your technical skills, particularly with firmware building and command line usage, proceed with the next steps. Note that the following instructions assume a higher level of technical expertise.

21. **Verify System Requirements**:
    - Ensure Python 3.9 or higher is installed, along with `ensurepip`. Check by running:
      ```bash
      python --version
      python -m ensurepip --version
      ```
    - Confirm at least 1 GB of free disk space is available on your system.

22. **Clone ESPHome Repository**:
    - Clone the ESPHome repository and navigate into the directory:
      ```bash
      git clone https://github.com/esphome/esphome.git
      cd esphome
      ```

23. **Switch to [this](https://github.com/esphome/esphome/pull/5535) Specific Pull Request**:
    - Fetch and check out the specific pull request needed for your setup:
      ```bash
      git fetch origin pull/5535/head:pr-5535
      git checkout pr-5535
      ```

24. **Run Setup Script**:
    - Execute the setup script. This step may take some time, especially if dependencies need to be downloaded:
      ```bash
      ./script/setup
      ```
    - If you encounter any dependency errors, attempt to resolve them before proceeding.

25. **Activate Virtual Environment**:
    - Once the setup is complete, activate the virtual environment created by the setup script:
      ```bash
      source venv/bin/activate
      ```

26. **Create Configuration File**:
    - Create a new file named `test.yaml` in the ESPHome directory. Do not change the file name. Use the content provided below, replacing placeholders with actual values:
      ```yaml
      esphome:  
        name: test  
        platformio_options:  
          board_build.flash_mode: dio  
      esp32:  
        board: esp32doit-devkit-v1  
        framework:  
          type: esp-idf  
          platform_version: 6.4.0  
          version: 5.1.1  
      wifi:  
        manual_ip:  
          static_ip: {{IP_for_the_device}}  
          gateway: {{GW_IP}}  
          subnet: 255.255.255.0  
          dns1: {{Usually_GW_IP}}  
        id: wifiId  
        ssid: {{Your_SSID}}  
        password: {{Your_PASSWORD}}  
        ap:  
          ssid: SHELLY-ESPHOME  
          password: asdfasdf  
      ota:  
        unprotected_writes: True # This is mandatory if you want to flash the partition table or bootloader!     
      logger:  
      web_server:  
      button:
      ```

27. **Configure Network Settings**:
    - Replace the placeholders in `test.yaml` with your actual network settings.
    - Ensure the static IP, gateway, and DNS settings are correct for your network environment. Use the same IP address that the device currently uses in its Tasmota configuration if accessible.
    - The `ap` block configures a fallback access point. Modify these settings as needed or leave them as defaults for troubleshooting connectivity issues.

28. **Compile the Firmware**:
    - Compile the ESPHome firmware by running:
      ```bash
      python -m esphome compile test.yaml
      ```

29. **Upload Firmware to Device**:
    - After compilation, locate the firmware binary at `./.esphome/build/test/.pioenvs/test/firmware.bin`.
    - Navigate to the Tasmota Web UI, select **Firmware Upgrade**, upload the `.bin` file, and start the upgrade.

30. **Checksum Error Handling**:
    - If you encounter a checksum error during upload, verify that you are uploading the correct binary file.

31. **Post-Flash Considerations**:
    - **Important:** The device is now running ESPHome, but this state is temporary. Updates via OTA at this stage will not persist across reboots because the firmware is only in the boot partition.

32. **Make Firmware Persistent**:
    - To permanently write ESPHome to the flash memory, allowing for future OTA updates, use:
      ```bash
      python -m esphome -v upload-factory-ota test.yaml
      ```

33. **Integration with Home Assistant**:
    - In Home Assistant, go to the ESPHome integration and add a new device, named `test`, but opt to download the YAML file instead of installing it directly. This will allow you to use the command line tool to flash it to the device, as it does not have the Home Assistant API key in it yet.

34. **Prepare the Configuration File**:
    - Modify the configuration file to include essential settings such as the API key, OTA password, and network credentials. This configuration is crucial for integrating the device into your Home Assistant environment.
    - Example configuration:
      ```yaml
      esphome:
        name: test
        friendly_name: test

      esp32:
        board: esp32doit-devkit-v1
        framework:
          type: esp-idf
          platform_version: 6.4.0
          version: 5.1.1

      logger:

      api:
        encryption:
          key: "{{API_KEY}}"

      ota:
        password: "{{OTA_PW}}"

      wifi:
        ssid: "{{SSID}}"
        password: "{{PASSWORD}}"
        ap:
          ssid: "Shelly"
          password: "IfJwQEwt7dCO"

      captive_portal:
      ```

35. **Backup and Replace the Configuration File**:
    - **Important**: Before replacing the `test.yaml` file in your ESPHome directory, save a backup of the original configuration. This can be helpful for troubleshooting or if you need to revert to the initial settings.
    - Replace the initial `test.yaml` file with the new configuration file you customized, ensuring to keep the filename as `test.yaml`. The OTA update process uses the filename `test` to look up the hostname and IP address of the device, and renaming it prematurely could cause the update process to fail.

36. **Recompile with Final Settings**:
    - Compile the final firmware with the updated `test.yaml` configuration:
      ```bash
      python -m esphome compile test.yaml
      ```

37. **Secure the OTA Password**:
    - Before flashing, securely store the OTA password. This ensures that you can continue to perform OTA updates if the configuration file is ever lost or corrupted.

38. **Final Flashing**:
    - Flash the compiled firmware onto your device by executing:
      ```bash
      python -m esphome upload test.yaml
      ```
    - Once this process is complete, the device should appear in Home Assistant, ready for further setup and customization.

39. **Device Renaming and Final Configuration**:
    - After the device is successfully integrated and shows up in Home Assistant, rename the hostname from `test` to something more descriptive and appropriate for its use or location in your home. This can be done within the Home Assistant ESPHome integration panel.
    - Adjust any final device settings as needed. For additional configuration examples and templates, you can refer to [ESPHome Devices](https://devices.esphome.io/devices/Shelly-Plus-Plug-S).

## Credits and Acknowledgments

I extend my deepest gratitude to the individuals and communities whose contributions have made this guide possible:

- **[Tasmota Team](https://github.com/tasmota/mgos32-to-tasmota32)**: Special thanks to the Tasmota team for their comprehensive guide on converting Shelly devices to Tasmota, which is a crucial step to making this transition to ESPHome possible.

- **[angelnu](https://github.com/esphome/esphome/pull/5535)**: My appreciation goes to angelnu, whose pull request has significantly facilitated the process of flashing ESP32 devices with a safeboot partition layout. Their guide has been instrumental in shaping the procedures outlined here.

- **Home Assistant Community**: I am thankful for the wealth of information shared by the community on the [Home Assistant Forum](https://community.home-assistant.io/t/shelly-plus-plug-s-esphome/544316). The discussions and insights from various members have greatly enriched this guide and provided practical tips that have been integrated into the instructions provided.

This guide was developed with the intent to assist and educate others in the ESPHome and Home Assistant community, building on the collective knowledge and experience shared by these resources. A special thanks to everyone who has participated in relevant discussions and shared their experiences; your contributions are greatly appreciated and have helped in refining this guide to better serve the community.

