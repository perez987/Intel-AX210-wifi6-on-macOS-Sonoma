# Wi-Fi 6 Intel AX210 on macOS Sonoma

macOS Sonoma removed drivers for Broadcom Wi-Fi cards found in Mac models prior to 2017. One of the affected cards is the Fenvi T-919, widely used in Hacks. OCLP developers published a fix that allows these Wi-Fi to work in Sonoma, adding this feature to the root patches that OCLP can apply. In order to apply root patches, OCLP requires macOS to run with some relaxed security features: SecureBootModel disabled and SIP partially disabled. This still represents a certain loss of security in macOS, as some users have noted.

Here I propose a model of Intel Wi-Fi card that by default lacks support but can be used in Sonoma thanks to the work of the OpenIntelWireless site. This is the Intel AX210S PCIe WiFi 6E card. This card can work with regular macOS security conditions without needing to relax Apple Secure Boot or SIP. It may be interesting for those who lack Wi-Fi in macOS Sonoma or for those who want to keep the security of their system without resorting to OCLP patches.

### Hardware
 
The card can be purchased in 2 different ways:

* fully assembled by _Ziyituod_: WiFi 6E Intel AX210S PCIe
  <p align="left">
  <img style="padding-left:120px;" width="340" src="Card and adapter.png">
  </p>
  </p>

* in 2 parts, card itself on the one hand (WiFi 6E Intel AX210 NGW Bluetooth Card for Laptop with M.2/NGFF Connector) and adapter on the other (PCIE X1 to M.2/NGFF A+E Key Adapter for WiFi Bluetooth Module).
  <p align="left">
  <img style="padding-left:120px;" width="320" src="AX210 card.jpg">
  </p>

### Revert OCLP patch and config.plist changes
 
In config.plist:

* disable kexts (`IOSkywalk.kext`, `IO80211FamilyLegacy.kext` and `AirPortBrcmNIC.kext`)
* disable ``IOSkywalk.kext` blocking
* change `csr-active-config` to 00000000
* change `SecureBootModel` to a value other than Disabled.

From OpenCore-Patcher >> Post-Install Root Patch >> Revert Root Patches.
 
### Installing wifi module
 
The 2 kexts are available on the OpenIntelWireless site. There are 2 ways to install Wi-Fi:

* `itlwm.kext`: uses `IOEthernetController` instead of `IO80211Family` so the connection spoofs as Ethernet even though it works as wifi. It does not use the macOS Wi-Fi menu, instead you have to use the HeliPort application.
* `AirportItlwm.kext`: uses `IO80211Family` so it works like the rest of the system's Wi-Fi connections. It provides minimal Continuity features (Handoff and Universal Clipboard) but appears to have lower stability than `itlwm.kext` and cannot connect to hidden networks. No HeliPort needed.

Both kexts should not be used at the same time, only one of them. I have tried both and they seem to have worked well, in my opinion the stability is very similar although with `AirportItlwm.kext` Photos app sometimes displays the "poor network with synchronization paused" message. The card is well detected, as you can see in Hackintool.

<p align="left">
<img width="740" src="AX210 Hackintool.png">
</p>

I am using HeliPort from [diepeterpan](https://github.com/diepeterpan/HeliPort), it is a fork of the oficial but has performance and interface improvements. From the HeliPort icon in the menu bar you can connect and disconnect Wi-Fi networks as well as set it to be added to the startup items.

<p align="left">
<img width="400" src="HeliPort menu.png">
</p>

### Installing Bluetooth module
 
On Monterey and newer you have to install 3 extensions:

* `IntelBTPatcher.kext` (available in OpenIntelWireless), requires Lilu 1.6.2 >> fixes a bug in bluetoothd by correctly initializing the bluetooth module
* `IntelBluetoothFirmware.kext` (available on OpenIntelWireless), at least version 2.2.0 >> load the firmware to the device and set the device name in USB Host Controller to Bluetooth USB Host Controller
* `BlueToolFixup.kext` (available in Acidanthera's [BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM) package) >> on macOS Monterey `IntelBluetoothInjector.kext`stopped working due to changes made by Apple to the Bluetooth stack.

### Performance
 
The Intel wifi card has performance not so different from the Fenvi with Broadcom. As for the 2 ways to install it, `AirportItlwm.kext` gives a better score.
 
<p align="left">
<img width="540" src="Test itlwm.png">
</p>
<p align="left">
<img width="540" src="Test AirportItlwm.png">
</p>

### Summary
 
This hardware is a valid option for those who do not have Wi-Fi in Sonoma or do not want to apply OCLP root patches. It is not expensive and is easy to install. As a main drawback, the features of the Apple ecosystem are lost (all with `itlwm.kext` and most with `AirportItlwm.kext`). Airdrop does not work in any way and this is the feature that I miss the most with respect to the Fenvi.
