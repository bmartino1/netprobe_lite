# Disclaimer!
Many thanks to plaintextpackets/netprobe_lite, as this project is a fork. It is intended for non-commercial use and is specifically tailored to run on Unraid as detailed in the forum post:
[Forum Link](https://forums.unraid.net/topic/163308-docker-netprobe/#comment-1425154)

This is not my original work; it has been modified to suit my needs and those of the Unraid community. A huge thank you to all contributors to the original project! Please see the original project here:
[Original Project on GitHub](https://github.com/plaintextpackets/netprobe_lite)

# Netprobe
What is it? Netprobe is a simple and effective tool for measuring ISP performance at home. The tool measures several performance metrics including packet loss, latency, jitter, and DNS performance. It also has an optional speed test to measure bandwidth. Netprobe aggregates these metrics into a common score, which you can use to monitor the overall health of your internet connection.

## Support the Project
If you'd like to support the development of this project, feel free to buy me a coffee!
- Plaintextpackets: [Buy me a coffee](https://buymeacoffee.com/plaintextpm)
- Bmartino1: [PayPal Donation](https://www.paypal.com/donate/?business=PA3ZHF52483KW&no_recurring=1&item_name=Tech+Support+%2F+Health+%2F+Buy+Me+a+Coffee&currency_code=USD)

## Full Tutorial about Netprobe
Visit YouTube for a full tutorial on how to install and use Netprobe:
[YouTube Tutorial](https://youtu.be/Wn31husi6tc)

## Requirements and Setup Notes FOR UNRAID ONLY!
The docker compose file can be edited for use elsewhere. To run Netprobe, you'll need a PC running Docker connected directly to your ISP router. Specifically:
1. Netprobe requires the latest version of Docker. For instructions on installing Docker, see YouTube — it's super easy. (Already installed on Unraid.)
2. Netprobe should be installed on a machine (the 'probe') which has a wired Ethernet connection to your primary ISP router. This ensures the tests are accurately measuring your ISP performance and exclude any interference from your home network. An old PC with Linux installed is a great option for this!

## Unraid Installation:
Assumptions:
- I assume you already have the compose plugin installed: [Docker Compose Manager by dcflachs](https://unraid.net/community/apps?q=docker+compose#r) Install via the community app plugin.

I assume you have a data spot in mind as we will need to download the data here. I chose to use Unraid's default docker appdata path: "/mnt/user/appdata"

### First-time Install
Refer to the forum post: [Forum Installation Guide](https://forums.unraid.net/topic/163308-docker-netprobe/?do=findComment&comment=1418929)

1. Open Unraid terminal (via Putty/Web UI / Console directly) cd to assumed data path to keep files. "cd /mnt/user/appdata" then GIT Clone the repo locally to the "probe"/Unraid machine:

    ```bash
    git clone https://github.com/bmartino1/netprobe_lite.git
    ```

2. From the docker tab in the Unraid Web UI, click add a new stack, give it the name Netprobe, and set the git download path location... in my case it will be "/mnt/user/appdata/netprobe_lite"

    See Picture: ![Netprobe Setup](https://content.invisioncic.com/u329766/monthly_2024_05/image.png.2d38cb023d10487efe2a0e6c6d17d835.png)

    *Congratulations! That's it! Unraid should now recognize and use the docker-compose.yml file and .env file from within the web UI.

### First-time Install - Unraid Web UI Fixes!
At this time, we need to fix Unraid web UI links and "beautify" the Unraid Docker Tab by adding a UI Label and linking the web UI to Grafana. Unfortunately, the docker compose no longer reads the docker-compose label/docker run -l option used by Unraid for its WEB UI. It is recommended to copy the links from this site by clicking the docker-compose.yml file. Options labeled below as well:

- Icons should autofill from first run of compose. Edit Stack > Compose File.... Otherwise you will need to update the UI Labels!

- **Service: netprobe-redis**
  - Icon: <img src="https://raw.githubusercontent.com/A75G/docker-templates/master/templates/icons/redis.png" width="64" height="64">
- **Service: netprobe-probe**
  - Icon: <img src="https://raw.githubusercontent.com/simonjenny/fastcom-mysql/master/fastlogo.jpg" width="64" height="64">
- **Service: netprobe-presentation**
  - Icon: <img src="https://github.com/chvb/docker-templates/raw/master/chvb/img/OnlyOfficeDocumentServer.jpg" width="64" height="64">
- **Service: netprobe-prometheus**
  - Icon: <img src="https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/prometheus.png" width="64" height="64">
- **Service: netprobe-speedtest**
  - Icon: <img src="https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/speedtest-tracker.png" width="64" height="64">
- **Service: netprobe-grafana**
  - Icon: <img src="https://github.com/atribe/unRAID-docker/raw/master/icons/grafana.png" width="64" height="64">
  - Web UI: `http://[IP]:[PORT:3001]`

Now it's time for the first run start.

    ```bash
    click the "compose up" button in the web UI
    ```

    *Please keep this window open until it finishes!
    This will set up the docker bridge network, then pull the necessary images. The running directory is the advance path location you set earlier when making the stack.

3. To finish the app, use docker compose again:

    ```bash
    click the "compose down" button in the web UI
    ```

    This Finishes the Unraid First Run Install!

### Upgrading Between Versions

*compose has been updated to facilitate this. uses a docker image. see compose file...
Unraid may show update available. Please click update stack if the docker update notification didn't go away... otherwise in Unraid you will have to stop the Netprobe dockers click compose down. Then click advance toggle and remove all the Netprobe docker images (this will not delete any collected data!) then click docker up this will pull the latest docker images.

Unraid seems to be fickle. You may see an "Update Available" message At this time Please ignore that unless you know there is a docker update. Per the support Forum for the docker plugin this is a known issue. and the update can only be done via the web UI update stack. I have found that you need to compose down first. click basic view switch to get to advance options and to delete the docker images to update and fix the web UI update displayed message. Once all the images are deleted (THIS WILL NOT DELETE ANY COLLECTED DATA!) click the "Update stack" button. If there is a docker image update this will update the docker. The compose file has been edited for Unraid use and how it handles the docker images. The file paths were changed to accommodate the storage of files between updates. I have not experienced Data loss doing updates this way.

Original Notes from plaintextpackets/netprobe_lite edited for Unraid:
When upgrading between versions, it is best to delete the deployment altogether and restart with the new code. The process is described below.
*with the data folder and compose file edits this is no longer necessary. Kept for others' use and info.

1. Stop Netprobe in Docker and use the -v flag to delete all volumes (warning this deletes old data):

    ```bash
    docker compose down -v
    ```

2. Clone the latest code (or download manually from Github and replace the current files):

    ```bash
    git clone https://github.com/plaintextpackets/netprobe_lite.git
    ```

3. Re-start Netprobe:

    ```bash
    docker compose up
    ```

## How to use

1. If you updated the web UI you can click on the Grafana docker and click web UI it will open Grafana. Then login and navigate to dashboard > netprobe
   *Navigate to: http://x.x.x.x:3001/d/app/netprobe where x.x.x.x = IP of the probe machine running Docker.

3. The Default user / pass is 'admin/admin'. Login to Grafana and set a custom password.
   *The password doesn't keep between docker updates. Please edit the compose file and set an admin password if you wish to change it.

### ENV File Edits to use:

### Customize DNS test

If the DNS server your network uses is not already monitored, you can add your DNS server IP for testing.

To do so, modify this line in .env:

    ```
    DNS_NAMESERVER_4_IP="8.8.8.8" # Replace this IP with the DNS server you use at home
    ```

Change 8.8.8.8 to the IP of the DNS server you use, then restart the application (docker compose down / docker compose up)

### Data storage - default method

By default, Docker will store the data collected in several Docker volumes, which will persist between restarts.

They are:

    ```
    netprobe_lite>config>grafana>data (used to store Grafana)
    netprobe_lite>config>prometheus>data (used to store time series data)
    ```

### Run on startup

Disabled by default. To enable on at restart Flip the Auto Start switch by the stack name made earlier "netprobe".
Netprobe will automatically restart itself after the host system is rebooted after that switch is pressed. the dockers also have a compose file edit in event of a docker error.


## FAQ & Troubleshooting

Q. I am running Pihole and when I enter my host IP under 'DNS_NAMESERVER_4_IP=' I receive this error:

    ```
    The resolution lifetime expired after 5.138 seconds: Server Do53:192.168.0.91@53 answered got a response from ('172.21.0.1', 53) instead of ('192.168.0.91', 53)
    ```
A. This is a limitation of Docker. If you are running another DNS server in Docker and want to test it in Netprobe, you need to specify the Docker network gateway IP:

1. Stop netprobe but don't wipe it (docker compose down)
2. Find the gateway IP of your netprobe-probe container:
    ```
    $ docker inspect netprobe-probe | grep Gateway
                "Gateway": "",
                "IPv6Gateway": "",
                        "Gateway": "192.168.208.1",
                        "IPv6Gateway": "", 
    ```
3. Enter that IP (e.g. 182.168.208.1) into your .env file for 'DNS_NAMESERVER_4_IP='

Q. I constantly see one of my DNS servers at 5s latency, is this normal?

A. 5s is the timeout for DNS queries in Netprobe Lite. If you see this happening for one specific IP, likely your machine is having issues using that DNS server (and so you shouldn't use it for home use).

## License
This project is released under the GNU Public License it origins comes from the forked custom license that restricts commercial use! You are free to use, modify, and distribute the software for non-commercial purposes. Commercial use of this software is prohibited without prior permission. If you have any questions or wish to use this software commercially, please contact [plaintextpackets@gmail.com].
