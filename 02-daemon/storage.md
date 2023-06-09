## Docker storage drivers:

> Storage is essential to almost any system, and containers are no exception.

> Storage drivers are sometimes also called as graph drivers. the proper storage driver to use often depends on your operating system and other local configuration factors. 

```
   overlay2: current Ubuntu and CentOS/RHEL veresions. 
   aufs: Ubuntu 14.04 and older
   devicemapper: CentOS 7 and earlier
```
```
Storage models: 

Persistent data can be manageed using several storage models. 
  Filesystem Storage:
     1) Data is stored in the form of a file system.
     2) used by overlay2 and aufs
     3) Efficient use of memeory
     4) iefficient with write-heavy workloads
  Block storage:
     1) stores data in blocks
     2) used by devicemapper
     3) Efficient with write-heavy workloads
  Object storage:
     1) stores data in an external object-based stored
     2) Application must be designed to use object-based storage.
     3) Flexible & scalable
```
```
Device Mapper Storage Driver:
   Device Mapper is one of the Docker storage drivers available for some Linux distributions. it is the default storage driver for CentOS7 and earlier

   we can customize Device Mapper configuration using the daemon config file. 

   Device Mapper supports two modes:
     loop-lvm mode:
        1) Loopback machanism simulates an additional physical disk using files on the local disk.
        2) Minimal setup, does not require an additional storage device.
        3) Bad performance, suggested use only for testing.
     direct-lvm mode: 
        1) Stores data on a seperate device
        2) Requires and additional storage device.
        3) Good Performance, suggested to use for Production.
```

## Configure a Storage Driver:

#### Get the current storage driver

`docker info`

#### Two ways we can set the storage drivers
```
    1) Set the storage driver explicitly by providing a flag to the Docker daemon:
	 sudo vi /usr/lib/systemd/system/docker.service
	 Edit the ExecStart line, adding the --storage-driver devicemapper flag:
	 ExecStart=/usr/bin/dockerd --storage-driver devicemapper ...

	 After any edits to the unit file, reload Systemd and restart Docker:

	 sudo systemctl daemon-reload
	 sudo systemctl restart docker
```
```
    2) We can also set the storage driver explicitly using the daemon configuration file. 
         This is the method that Docker recommends. 
         Note that we cannot do this and pass the --storage-driver flag to the daemon at the same time

         sudo vi /etc/docker/daemon.json
	 Set the storage driver in the daemon configuration file:
		{
			"storage-driver": "devicemapper"
		}
	 Restart Docker after editing the file. 
	 It is also a good idea to make sure Docker is running properly after changing the configuration file:

	 sudo systemctl restart docker
	 sudo systemctl status docker
```
##### more Info: https://docs.docker.com/storage/storagedriver/select-storage-driver/
