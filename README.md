# Docker image for MATLAB license manager (lmgrd)

Dockerized matlab license server. The deployment requires a license.dat file placed in the same place as the composer manifest:

license.dat file example:

```text
# BEGIN--------------BEGIN--------------BEGIN
# MathWorks license passcode file.
# LicenseNo: ***   HostID: {{ container MAC address}}
#
# R***
#
SERVER {{ hostname }} {{ container MAC address}} {{ lmgrd port }}
DAEMON MLM /lmgrd/etc/glnxa64/MLM port={{ mlm port }}
INCREMENT <<License information>>
# END-----------------END-----------------END
```

The `{{}}` placeholders have to fit with the docker-compose configuration. Hostname and mac address had to fit with license.dat information.

Port for lmgrd 27000
Port for mlm 27001

For further explanation, refer to the [matlab info](https://es.mathworks.com/help/install/administer-network-licenses.html). 

For clienta who need to use this lmgrd, they could use a simple license file which points to the server address:

```text
SERVER 192.168.99.10 INTERNET=192.168.99.10 27000
USE_SERVER
```

Advertisement: Mathworks does not provide a security method for the license server access, and it should be secured by the administrator, it's not recomended to expose it to the internet without security further security mechanism.