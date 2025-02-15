# Table of Contents

* [Introduction](#pialert)
* [Scan Methodes](#scan-methods)
* [Backend](#backend-back)
* [Frontend](#frontend-front)
* [Installation](#installation)
* [Update](#update)
* Additional information
  * [Screenshots](docs/SCREENSHOTS.md)
  * [Device Management](docs/DEVICE_MANAGEMENT.md)
  * [Bulk Editor](docs/BULKEDITOR.md)
  * [Network Relationship](docs/NETWORK_RELATIONSHIP.md)
  * [Web service monitoring](docs/WEBSERVICES.md)
  * [Uninstall Pi.Alert](docs/UNINSTALL.md)
* [Closing words](#closing-words)


# Pi.Alert
<!--- --------------------------------------------------------------------- --->

WIFI / LAN intruder detector with web service monitoring.

Scan your WIFI/LAN-connected devices and receive alerts for unknown device connections. Get warnings 
if an "always connected" device disconnects. Additionally, assess web service availability by evaluating 
the HTTP status code, SSL certificate, and service response time. Receive notifications if the SSL 
certificate changes, the HTTP status code changes, or if the service becomes unreachable. Other features 
include the detection of unwanted/foreign DHCP servers and device monitoring using the ping command.


![Main screen][main]
[Compare this fork with the main project](docs/VERSIONCOMPARE.md)


### Scan Methods

  - **arp-scan**. The arp-scan system utility is used to search for devices on the network using arp frames.
  - **Pi-hole**. If the Pi-hole DNS server is active, Pi.Alert examines its activity looking for active devices using DNS that have not been detected by other methods.
  - **dnsmasq**. If the DHCP server dnsmasq is active, Pi.Alert examines the DHCP leases (addresses assigned) to find active devices that were not discovered by the other methods.
  - **Fritzbox**. If you use a Fritzbox (a router from the company "AVM"), it is possible to perform a query of the active hosts. This also includes hosts of the guest WLAN and Powerline devices from "AVM".
  - **Mikrotik**. If you use Mikrotik Router as DHCP server, it is possible to read DHCP leases.
  - **UniFi**. If you use UniFi controller, it is possible to read clients (Client Devices)
  - **Web service monitoring**. An HTTP request is sent and the web server's response is processed. If self signed certificates are used, no validation of the certificate is performed.
  - **ICMP monitoring**. A "ping" is sent to a manually specified IP/hostname/domain name and the response is evaluated
  - **DHCP Server Scan**. Nmap is used to send DHCP requests into the network to detect unknown (rogue) DHCP servers.

### Backend (back)

The backend is started at regular intervals via cronjobs of the user who installed Pi.Alert.

The system scans the network to detect connected devices using various scanning methods as described above. 
It also verifies the accessibility of web services and notifies about any changes in SSL certificates. 
The gathered information is stored in the database. Detected changes are reported via email and/or other services 
such as [Pushsafer](https://www.pushsafer.com/), [Pushover](https://pushover.net/), NTFY, Gotify, and Telegram through [shoutrrr](docs/SHOUTRRR.md), as well as to the Frontend. Automated 
tasks for cleaning up and optionally backing up the database are also performed. Additionally, optional speed tests of the Internet 
connection can be conducted. Users can utilize the [pialert-cli](docs/PIALERTCLI.md) tool to configure login, password, and other 
settings conveniently.

### Frontend (front)

A configurable login feature is available to prevent unauthorized access, with the default password set to "123456". By default, this feature is disabled. 
To enable password protection, adjust the configuration settings either in the ~/pialert/config/pialert.conf file or via the pialert-cli tool.

Moreover, the system offers extensive functionalities:

It manages device inventory and characteristics, facilitating individual management or bulk edits. The collected data, including sessions, connected devices, 
favorites, events, presence, and internet IP address changes, is visually represented. For enhanced device management, manual Nmap scans and Wake-on-LAN 
(if supported) are available, alongside speed tests for the "Internet" device in the details view.

Additionally, it provides insights into network relationships through a simple display. Users can perform various maintenance tasks and customize settings, 
including language selection (English, German, Spanish, French, Italian), AdminLTE-Skins/Theme/Favicons selection, API-key configuration, login management, 
database maintenance tools, and config file editing.

For support, a comprehensive Help/FAQ section is accessible. Notifications with download options keep users informed, while a journal tracks operations 
performed via the frontend, pialert-cli, and cronjob. 

New [Favicons/Homescreen icons](docs/ICONS.md) have been created based on the original design, tailored to different skins. To ensure compatibility with 
iOS devices, icons can be directly linked from the repository, as iOS devices may not load homescreen icons from insecure sources (without SSL or self-signed SSL).

There are [various ways](docs/API-USAGE.md) to submit a request to the backend with the help of a API. I will use curl/bash and curl/php as examples in the following. 

# Installation
<!--- --------------------------------------------------------------------- --->
Initially designed to run on a Raspberry Pi, probably it can run on some other
Linux distributions which use the "apt" package manager. Check "[Things to keep in mind when using different Linux distributions](docs/LINUX-DISTRIBUTIONS.md)" before using Pi.Alert with another Debian based distribution like DietPi or Ubuntu Server to see, if there are any special notes to follow.

<table>
  <thead>
    <tr><th align="left">One-step Automated Install</th></tr>
  </thead>
  <tbody>
  <tr><td>

```
bash -c "$(wget -qLO - https://github.com/leiweibau/Pi.Alert/raw/main/install/pialert_install.sh)"
```
  </td></tr>
  </tbody>
</table>


- [Installation Guide (step by step)](docs/INSTALL.md)

:bulb: <ins>Additional components and information</ins>

 - [Things to keep in mind when using different Linux distributions](docs/LINUX-DISTRIBUTIONS.md) (will be updated if necessary)
 - If you want to use my fork as LXC container, feel free to check out the awesome Helper scripts from [tteck/Proxmox](https://github.com/tteck/Proxmox)
 - Another active developed fork of Pi.Alert based on Docker can be found here: [jokob-sk/Pi.Alert](https://github.com/jokob-sk/Pi.Alert)
 - The original, but unmaintained, Pi.Alert can be found here [pucherot/Pi.Alert](https://github.com/pucherot/Pi.Alert/)

# Update
<!--- --------------------------------------------------------------------- --->
You can always check for a new release using the "Update Check" button in the sidebar. This check will show you if the GeoLite2 DB is 
installed or up to date and which new features, fixes or changes are available in the new Pi.Alert release, if you are not already using the latest version.

There are no updates as Github release package, because the update function is done by the "wget" command anyway. Instead, after a certain number of commits, 
a new archive is created, which is used as the source for the update.

This update script is only recommended for an already existing installation of this fork. If you are using another fork, 
I recommend uninstalling it first. If you backup the database, it may be possible to continue using it with my fork after a patch ([pialert-cli](docs/PIALERTCLI.md)).

<table>
  <thead>
    <tr><th align="left">One-step Automated Update</th></tr>
  </thead>
  <tbody>
  <tr><td>

```
bash -c "$(wget -qLO - https://github.com/leiweibau/Pi.Alert/raw/main/install/pialert_update.sh)"
```
  </td></tr>
  </tbody>
</table>

An archive of older versions can be found at [https://leiweibau.net/archive/pialert](https://leiweibau.net/archive/pialert/). This archive contains all release notes of my fork.

# Closing words
<!--- --------------------------------------------------------------------- --->

### Support

  If you would like to support me and my work, I offer the following options.

  | [<img src="https://raw.githubusercontent.com/leiweibau/Pi.Alert/assets/githubsponsor.png" height="30px">](https://github.com/sponsors/leiweibau) | [<img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" height="30px">](https://www.buymeacoffee.com/leiweibau) |
  | ---- | ---- |

  A personal thank you :pray: to every sponsor of my fork.

  [jbierwiler](https://github.com/jbierwiler), [tcoombs](https://github.com/tcoombs), [hspindel](https://github.com/hspindel), [accessiblepixel](https://github.com/accessiblepixel), [AJ Tatum](https://github.com/ajtatum)

  Also a big thank you to the direct or indirect contributors.

  [Macleykun](https://github.com/Macleykun), [Final-Hawk](https://github.com/Final-Hawk), [TeroRERO](https://github.com/terorero), [jokob-sk](https://github.com/jokob-sk/Pi.Alert), [tteck](https://github.com/tteck/Proxmox) and many more

### Additionally used components and services
[Animated GIF (Loading Animation)](https://commons.wikimedia.org/wiki/File:Loading_Animation.gif), [Selfhosted Fonts](https://github.com/adobe-fonts/source-sans), 
[Bootstrap Icons](https://github.com/twbs/icons), [For final processing of background images](https://www.imgonline.com.ua/eng/make-seamless-texture.php), 
[DeepL](https://www.deepl.com), [ChatGPT](https://chat.openai.com)


### License
  GPL 3.0
  [Read more here](LICENSE.txt)

### Contact

  leiweibau@gmail.com

<!--- --------------------------------------------------------------------- --->
[main]:    https://raw.githubusercontent.com/leiweibau/Pi.Alert/assets/screen_main.png          "Main screen"

