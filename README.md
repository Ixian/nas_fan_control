NAS_fan_control

This is a script to control fan speed on NAS boxes based on hdd temps as well as the usual CPU/System/etc. temps, using fans in zones. This fork is specific to newer Asrock motherboards such as the X570D4U running Linux. It uses a PID calculation which works well to smooth out fan curves/ramp time/etc.

The script has been modified for Linux from the origina(s) which were targeted at FreeBSD. "nas_fan_control.pl" and "nas_fan_control.config.ini" are the two files you'll need for Supermicro or Asrock boards running Linux (tested on Ubuntu LTS 20.04.1). Expermental CorsairLink support is also included as an alternative but has not been fully tested. 

Originally this was a fork of Kevin Horton's repository with updates from https://github.com/roburban/nas_fan_control by Rob Urban for the Asrock code: https://github.com/khorton/nas_fan_control

Sretalla forked the above repository and made extensive changes to PID_fan_control.pl which I incorporated, including optional support for logging to InfluxDB.

I've made several changes to support running the script on Linux-based OSs, such as replacing camcontrol (which is BSD-specific) to simply allow and option to input a static list of drives for simplicity. This could be cleaned up and moved to a config file. 

See the script header for detailed notes and history.

Usage:

The two primary files you want are nas_fan_control.pl and nas_fan_control_config.ini. See file headers for further detail.

In nas_fan_control.pl set the Script Mode to the type of board you have. 

Insure dependencies are installed if not present:

sudo apt install perl hddtemp lm-sensors ipmitool

Create new files; modify names & paths to suit:

sudo vim nas_fan_control.pl /usr/local/bin/
sudo vim nas_fan_control_config.ini /usr/local/bin/
sudo vim NasFanControl.service /lib/systemd/system/

Create and enable new service via SystemD:

sudo systemctl daemon-reload
sudo systemctl enable NasFanControl.service
sudo service NasFanControl start

Check working status:

watch -n5 sudo ipmitool sensor OR sudo ipmitool sdr

See logs (paths in script) for additional details/status. 
