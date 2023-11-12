# Wi-Fi 6 Intel AX210 on macOS Sonoma

macOS Sonoma removed drivers for Broadcom Wi-Fi cards found in Mac models prior to 2017. One of the affected cards is the Fenvi T-919, widely used in Hacks. OCLP developers published a fix that allows these Wi-Fi to work in Sonoma, adding this feature to the root patches that OCLP can apply. In order to apply root patches, OCLP requires macOS to run with some relaxed security features: SecureBootModel disabled and SIP partially disabled. This still represents a certain loss of security in macOS, as some users have noted.

Here I propose a model of Intel Wi-Fi card that by default lacks support but can be used in Sonoma thanks to the work of the OpenIntelWireless site. This is the Intel AX210S PCIe WiFi 6E card. This card can work with regular macOS security conditions without needing to relax Apple Secure Boot or SIP. It may be interesting for those who lack Wi-Fi in macOS Sonoma or for those who want to keep the security of their system without resorting to OCLP patches.

### Hardware
 
The card can be purchased in 2 different ways:

* fully assembled by _Ziyituod_: WiFi 6E Intel AX210S PCIe
* in 2 parts, card itself on the one hand (WiFi 6E Intel AX210 NGW Bluetooth Card for Laptop with M.2/NGFF Connector) and adapter on the other (PCIE X1 to M.2/NGFF A+E Key Adapter for WiFi Bluetooth Module).


