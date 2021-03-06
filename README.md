# syncthing-tools
Supplemental tools for Syncthing

## set-st-bw
This is a python script to modify the traffic shaping on a Syncthing server
using the REST API.  It was written for altering Syncthing's trafic shaping on a
schedule using CRON.

### Requires
* Python 2.7+
  * optparse
  * ConfigParser
  * requests , probably > 2.0
  * json


### Configuration
A minimal configuation file:
```
[localhost]
url=https://localhost:8384
apikey=SECRETSAUCE
# If set to 1 SSL certificates are not verified against any CA.
# Use for self-signed certs like those that Syncthing uses by default.
insecure=True

[remotehost]
url=https://remotehost:8384
username=USER
password=PASSWORD
apikey=UNKNOWNADDITIVES
# If set to 1 SSL certificates are not verified against any CA.
# Use for self-signed certs like those that Syncthing uses by default.
insecure=True
```
The defaul configuration location is hard coded in the script to
/etc/syncthing/set-st-bw.cfg. Override it with the -c command line option.

### Running via CRON

- [ ] try

Example:
```
30 22  * * * root /path/to/set-st-bw.py -x remotehost -r 2000 -s 2000
0   5  * * * root /path/to/set-st-bw.py -x remotehost -r 500 -s 500
```


### Usage
```
Usage: set-st-bw.py [-e] [-c CONFIGFILE] [-x SECTION]  [-n] [-r MAX_RECV] [-s MAX_SEND]

Options:
  -h, --help            show this help message and exit
  -x SECTION, --conf-section=SECTION
                        Name of the configuration section to use for
                        connection parameters.
  -c CONFIG_PATH, --config-file=CONFIG_PATH
                        Path to configuration file.
  -n, --no-change       Make no changes, only print the current settings.
  -e, --restart         Restart Syncthing after changes have been made. For
                        Syncthing < v0.14.19.
  -r MAX_RECV, --max-recv=MAX_RECV
                        Set maxRecvKbps. Not required if -n is used.
  -s MAX_SEND, --max-send=MAX_SEND
                        Set maxSendKbps. Not required if -n is used.

```
