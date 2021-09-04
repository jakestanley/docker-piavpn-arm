docker-mullvad-deluge-arm
================

Debian Jessie based mullvad.net vpn with torrents/proxy (openvpn, deluged, deluge-web, dante-server)

This is a fork of docker-piavpn-arm that works (TBC) with Mullvad VPN.

Tested (TBC) against on Raspberry Pi OS on Raspberry Pi 4.

Currently hardcoded to use a UK Manchester exit node.

Complete run command with all options

    docker run -d -p 8112:8112 -p 1080:1080 \
        --name vpn \
        --dns=8.8.8.8 \
        --cap-add=NET_ADMIN \    
        -v /mytorrentdir:/torrents \
        -v /mydelugeconfigdir:/app/deluge \
        -v /etc/localtime:/etc/localtime:ro \
        -e DELUGE_UID=500 -e DELUGE_GID=500 \
        -e HOST_SUBNET=192.168.1.0/24 \
        -e MVAD_USER=<user> \
        -e MVAD_PASS=<password> \
        -e MVAD_PORT=12345 \
        jakestanley/docker-mullvad-deluge-arm


Change directory mappings as appropriate (delugeconfig, torrents)

notes
=====

* DELUGE_UID and DELUGE_GID are optional, but will default to 500/500.   Specify the UID/GID that corresponds to the **HOST** UID/GID you want to own the downloads, config and movies directories.
* The deluge web will only work on your local subnet.   Policy based routing is what led me to this solution, I'm not going down that road again.  If you need access from the outside, either VPN or SSH/Port forward in.
* You must map a torrents directory, no torrents inside container
* If you want it to restart on reboot, add --restart=always
* If you leave the DNS out, your local dns servers will be used.  Not good for privacy.
* The NET_ADMIN capability is needed to create the TUN device
* The host subnet needs to be in CIDR notation. For example if your host network is 192.168.1.x with a netmask of 255.255.255.0, then HOST_SUBNET=192.168.1.0/24
* After running for the first time, setup deluge so all torrents begin with /torrents
