# Docker image for MATLAB license manager (lmgrd)

Dockerized matlab license server image and deployment compose file.

## About license files

The deployment requires one or serveral valid license.dat files. The files have to be placed in a folder named licenses, in same place as the composer manifest. A volume is created in the licenses folder inside the container as required by the lmgrd software.

License files can also be copied inside the container image if needed. Just uncomment the '''COPY''' line in the 'Dockerfile', it will copy a file named license.dat from ./licenses/ to the desired location. If more license files are needed, just concatenate the files, copy each file independetly or copy the whole folder.

Important notice: Files loaded have to be '''.dat''' files. '''.lic''' files have to be converted to work properly. More information and conversion instructions [here](https://www.mathworks.com/matlabcentral/answers/116637-what-are-the-differences-between-the-license-lic-license-dat-network-lic-and-license_info-xml-lic)

### license.dat file example:

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

## About docker compose

In the docker-compose file, the `{{}}` placeholders have to fit with the required configuration for the license or licenses that have to be deployed. Hostname and mac address had to fit with license.dat information.

Default ports:

 - Port for lmgrd 27000
 - Port for mlm 27001

If other ports need to be used, ensure that the docker-compose is properly updated.

For further explanation, refer to the [matlab info](https://es.mathworks.com/help/install/administer-network-licenses.html).

'''LICENSEDIR''' is where you store your licenses for the license manager. If the volume method is used, it is possible to easily update the license file without rebuild the Docker image. You might need to restart the Docker container if needed. If the copy method is used, the image has to be rebuild.

## About clients usage

For clients who need to use this lmgrd, they could use a simple license file which points to the server address:

```text
SERVER 192.168.99.10 INTERNET=192.168.99.10 27000
USE_SERVER
```

## IMPORTANT cybersecurity considerations

Advertisement: Mathworks does not provide a security method for the license server access, and it should be secured by the administrator, it's not recomended to expose it to the internet without further access management mechanisms.