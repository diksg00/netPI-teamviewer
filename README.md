## TeamViewer

[![](https://images.microbadger.com/badges/image/hilschernetpi/netpi-teamviewer.svg)](https://microbadger.com/images/hilschernetpi/netpi-teamviewer "Teamviewer")
[![](https://images.microbadger.com/badges/commit/hilschernetpi/netpi-teamviewer.svg)](https://microbadger.com/images/hilschernetpi//netpi-teamviewer "Teamviewer")
[![Docker Registry](https://img.shields.io/docker/pulls/hilschernetpi/netpi-teamviewer.svg)](https://registry.hub.docker.com/u/hilschernetpi/netpi-teamviewer/)&nbsp;
[![Image last updated](https://img.shields.io/badge/dynamic/json.svg?url=https://api.microbadger.com/v1/images/hilschernetpi/netpi-teamviewer&label=Image%20last%20updated&query=$.LastUpdated&colorB=007ec6)](http://microbadger.com/images/hilschernetpi/netpi-teamviewer "Image last updated")&nbsp;

Made for [netPI](https://www.netiot.com/netpi/), the Raspberry Pi 3B Architecture based industrial suited Open Edge Connectivity Ecosystem

### Debian with TeamViewer

The image provided hereunder deploys a container with installed [TeamViewer](https://www.teamviewer.com/en/) Host software to enable remote management of exactly this container over a TeamViewer Client.

Base of this image builds [debian](https://www.balena.io/docs/reference/base-images/base-images/) and a [TeamViewer Host](https://www.teamviewer.com/en/download/linux/) daemon compiled for [Raspberry Pi](https://download.teamviewer.com/download/linux/teamviewer-host_armhf.deb)

Teamviewer Host daemon in general is able to grab frames (/dev/fb0) and keystrokes from the Linux system console (/dev/tty1) only. This is why the container invokes a login [session](https://linux.die.net/man/2/setsid) with the program [agetty](http://man7.org/linux/man-pages/man8/agetty.8.html) to generate a new system console you can access to via the TeamViewer client.

### TeamViewer End User License Agreement (EULA)

Before using TeamViewer you have to accept the [TeamViewer EULA](https://www.teamviewer.com/en/eula/).

Since in a container you cannot click on an "accept" button like you are use to on desktop systems such as Windows or Linux, you have to define an environment variable as container parameter instead to accept it.

To accept the TeamViewer EULA set a container environment variable `TEAMVIEWER_LICENSE` to the value `accept`. The subsequent TeamViewer daemon will be started with the option `license accept` then.

For a quick overview ... here is the Teamviewer Host daemon printout if called with the `license show` option:

-----------------------------------------------------------------------------------------------------------------------------
TeamViewer® End-User License Agreement

This End-user License Agreement including its Annex ("EULA") applies to you and TeamViewer GmbH ("TeamViewer" or "We") for the licensing and use of our software, which includes the TeamViewer software and all versions, features, applications and modules thereto ("Software").
This EULA also covers any associated media, printed materials and electronic documentation that we make available to you (with our Software and "Product"). Future releases of our Product may warrant amendments to this EULA.

BY CLICKING "ACCEPT", DOWNLOADING OR OTHERWISE USING OUR SOFTWARE, YOU AGREE TO ALL TERMS AND CONDITIONS OF THIS EULA. IF YOU DO NOT AGREE TO ANY OF THE TERMS OF THIS EULA, PLEASE IMMEDIATELY RETURN, DELETE OR DESTROY ALL COPIES OF OUR SOFTWARE IN YOUR POSSESSION.

You can review the full license agreement at http://www.teamviewer.com/link/?url=271351

-----------------------------------------------------------------------------------------------------------------------------

### TeamViewer Password

A session between a TeamViewer Client and TeamViewer Host is protected by a password. The password is a parameter handed over to the TeamViewer Host daemon when it is started and is requested by the TeamViewer client before the connection is established.

To define the TeamViewer Daemon password set a ontainer environment variable `TEAMVIEWER_PASSWD` to your specific password e.g. `V>yC9b[J`. If there is no password environment variable defined, the container sets the default password to `12345678`.

#### Container prerequisites

##### Privileged mode

The privileged mode option needs to be activated to lift the standard Docker enforced container limitations. With this setting the container and the applications inside are the getting (almost) all capabilities as if running on the Host directly. 

netPI's secure reference software architecture prohibits root access to the Host system always. Even if priviledged mode is activated the intrinsic security of the Host Linux Kernel can not be compromised.

##### Host devices

To grant the program agetty access to the system console the `/dev/tty1` host device needs to be exposed to the container.

To grant the TeamViewer Daemon access to the system console framebuffer the `/dev/fb0` host device needs to be exposed to the container.

##### Environment Variables

Define an environment variable `TEAMVIEWER_LICENSE` with value `accept` to accept the TeamViewer EULA.

Define an environment variable `TEAMVIEWER_PASSWD` with value `YOUR PASSWORD` to define a secure password between TeamViewer Host and TeamViewer Client.

#### Getting started

STEP 1. Open netPI's website in your browser (https).

STEP 2. Click the Docker tile to open the [Portainer.io](http://portainer.io/) Docker management user interface.

STEP 3. Enter the following parameters under *Containers > + Add Container*

Parameter | Value | Remark
:---------|:------ |:------
*Image* | **hilschernetpi/netpi-teamviewer**
*Runtime > Env* | *name* **TEAMVIEWER_LICENSE** -> *value* **accept** |
*Runtime > Env* | *name* **TEAMVIEWER_PASSWD** -> *value* **YOUR PASSWORD** |
*Restart policy* | **always**
*Runtime > Devices > +add device* | *Host path* **/dev/tty1** -> *Container path* **/dev/tty1** |
*Runtime > Devices > +add device* | *Host path* **/dev/fb0** -> *Container path* **/dev/fb0** | 
*Runtime > Privileged mode* | **On** |

STEP 4. Press the button *Actions > Start/Deploy container*

Pulling the image may take a while (5-10mins). Sometimes it may take too long and a time out is indicated. In this case repeat STEP 4.

#### Accessing

The container starts the TeamViewer Host daemon in accordance your environment variable settings. 

Based on the physical hardware parameters found (CPU serial number, MAC ID) the deamon will generate a unique TeamViewer ID and tries to log on to the TeamViewer server over the Internet using this ID.

Finally it prints the Teamviewer ID to the containers log file. 

To retreive the ID you can have a look at this log file via netPI's Docker management web GUI if you click the small icon `logs` in the overiew list of the running containers.

#### Automated build

The project complies with the scripting based [Dockerfile](https://docs.docker.com/engine/reference/builder/) method to build the image output file. Using this method is a precondition for an [automated](https://docs.docker.com/docker-hub/builds/) web based build process on DockerHub platform.

DockerHub web platform is x86 CPU based, but an ARM CPU coded output file is needed for Raspberry systems. This is why the Dockerfile includes the [balena](https://balena.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/) steps.

#### License

View the license information for the software in the project. As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).
As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.ex.php/bluetooth) to start the bluetooth interactive command utility. Input `scan on` to discover nearby bluetooth devices.

[![N|Solid](http://www.hilscher.com/fileadmin/templates/doctima_2013/resources/Images/logo_hilscher.png)](http://www.hilscher.com)  Hilscher Gesellschaft fuer Systemautomation mbH  www.hilscher.com

