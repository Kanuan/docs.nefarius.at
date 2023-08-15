# Monitoring BthPS3 via Windows' Event Viewer

## Finding BthPS3 events in Windows' Event Viewer

1. Open Windows' start menu then start typing "Event Viewer" to search for it. Once it's found, open it
    - Alternatively, press `win + R` on your keyboard, type `eventvwr` then confirm "OK"
2. On the left side of the Event Viewer, expand "Windows Logs" then select "System"
3. On the right side, select "Filter Current Log", then on "Event Sources" select only `Nefarius BthPS3 Profile Driver` before confirming "OK"
4. The main portion of Event Viewer will now exibit only BthPS3 related events
    - Make sure to use the "Refresh" button in the right side to update the list with new events

![Event Viewer Filtering](../images/bthps3-event-viewer-filtering.png)

![BthPS3 events](../images/bthps3-event-viewer-messages.png)

## Interpreting the events' messages

The answer is: ___it depends___.

You need to verify which `Input API` is supported in whatever you are using and if you want to use DS3 specific functions, like pressure buttons or motion controls. The following table should help you:

| The General message of the event | What it means | 
| :---: | :---: |
| Auto-disabling filter driver option set, filter disabled | BthPS3 had previously detected that a device was trying to connect via its forbidden channels, but their connection was denied either because it had an unsupported name or was a type of device that should not be allowed to connect via the forbidden channels. BthPS3 then disabled its filter in order to allow these denied devices to try reconnecting under normal means | 
| Auto-enabling filter driver option set, re-enabling filter driver in X seconds | BthPS3 had temporarily disabled its filter in order to allow unsupported/denied devices (usually DS4 and DualSense controllers) to try reconnecting to Windows via normal means. Filter is set to re-enable itself after X seconds so supported devices (usually DS3 controllers) are allowed to connect again | 
| Could not assign idle settings: the calling driver is not the device's power policy owner and device not in RAW mode. If DsHidMini is installed and operative this warning can be ignored | Help me, Nef | 
| Device 0xXXXXXXXXXXXX  disconnected with NTSTATUS STATUS_SUCCESS | Controller was instructed to disconnect and did so successfully | 
| Device 0xXXXXXXXXXXXX disconnected with NTSTATUS STATUS_DEVICE_NOT_CONNECTED | It was confirmed that a controller previously instructed to disconnect is not present in the system anymore (as expected) | 
| Device 0xXXXXXXXXXXXX identified as SIXAXIS compatible | A device attempting to connect was identified as a SIXAXIS (standard PS3 controller) based on BthPS3's supported SIXAXIS name list | 
| Device 0xXXXXXXXXXXXX not identified or denied, dropping connection. Consult the online documentation for potential solutions. | It was detected that a device was trying to connect via BthPS3's supported "forbidden channels", but its name was not identified as SIXAXIS or WirelessController compatible, so their connection was dropped. Alternatively, BthPS3 identified the device as one that shouldn't be allowed to connect via the forbidden channels and denied their connection (usually happens with DS4/DualSense controllers trying to reconnect the first time as they should only be allowed to reconnect under the normal channels to prevent issues) | 
| Device 0xXXXXXXXXXXXX reported name: "CONTROLLER-NAME" | The name being reported by the device attempting to connect via BthPS3's supported "forbidden channels" | 
| Device 0xXXXXXXXXXXXX has both L2CAP channels connected and is ready to operate | BthPS3 successfully bridged the device attempting to connect via the supported "forbidden channels" and Windows's standard BT stack | 
| Driver loaded (obj: 0xffffe284e9faddf0, status: STATUS_SUCCESS) | BthPS3 was successfully loaded onto the PC's BT stack (normal to occur when the BT is being enabled) | 
| Driver unloaded (obj: 0x3a76202d36d8) | BthPS3 was successfully unloaded from the PC's BT stack (normal to occur when the BT is being disabled) | 
| Filter enabled successfully | BthPS3' L2CAP PSM patching is enabled (required for PS3 controllers). When the filter is active, devices attempting to connect via some Windows' BT "forbidden channels" are patched by BthPS3 so they are allowed to connect. Otherwise, Windows' standard BT stack will inmediately deny their connection | 
| HID Control Channel connection established | Help me, Nef | 
| HID Interrupt Channel connection established | Help me, Nef | 
| Host radio HCI major version: X | The [LMP version](https://support.microsoft.com/en-us/windows/what-bluetooth-version-is-on-my-pc-f5d4cff7-c00d-337b-a642-d2d23b082793) of the Bluetooth Adapter active on the PC | 
| Successfully created PDO for 0xXXXXXXXXXXXX ("CONTROLLER-NAME") and assigned slot index X | Help me, Nef | 
| Successfully destroyed PDO in slot X (BTHPS3BUS\{53f88889-1aaf-4353-a047-556b69ec6da6}&Dev&VID_054C&PID_0268) | Help me, Nef | 
