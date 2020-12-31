# NetStatus
NetStatus is a dashboard WebUI to track internet connectivity

This project was intended for use with Raspberry Pi 4 + 3.5" LCD display, however with docker
it supports AMD64 arch & can be used on other hardware
![Raspberry PI screenshot](https://raw.githubusercontent.com/Ryandev/NetStatus/master/documentation/rpi.jpg "Raspberry PI screenshot")
[Setup instructions for RPI](https://raw.githubusercontent.com/Ryandev/NetStatus/master/documentation/rpi.md)

## Features
- Offline notifications
- Periodic netspeed speed checks (latency, jitter, upload/download speed)
- Configurable
- Easy to run (Docker)
- Designed to work on any landscape screensize, optimized for small screens

## Screenshots

##### Everything ok
![Dashboard status ok](https://raw.githubusercontent.com/Ryandev/NetStatus/master/documentation/dashgood.png "Dashboard status ok")

##### No network connection found/lost
![Dashboard offline](https://raw.githubusercontent.com/Ryandev/NetStatus/master/documentation/dashoffline.png "Offline")

##### Latency/Upload error status & download warning status
![Dashboard status slow](https://raw.githubusercontent.com/Ryandev/NetStatus/master/documentation/dashslow.png "Dashboard status slow")
*Bottom left is the time elapsed since the last speed test, 
bottom right is your ISP name/location*

## Run
### Arm64 (Raspberry Pi 4)
```
sudo docker run --name netspeed -d --restart=always -p 80:80 ryandev/netspeed:arm64
```
Now go to http://localhost to view 

### AMD64 (PC)
```
sudo docker run --name netspeed -d --restart=always -p 80:80 ryandev/netspeed:latest
```


### Configurables
| Name                                      | Description                                                                                                                                                                                   | Environment name               | Value units | Default value           |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|-------------|-------------------------|
| Frequnecy of ping checks                  | How frequently to fetch a favicon to check if the network is there.  This is needed as `navigator.isOnline` implmentation varies across browsers                                              | REACT_APP_PINGINTERVAL         | Seconds     | 15                      |
| Which websites to check connectivity with | Used with the above parameter, a random website from this list is pulled and the favicon is fetched from. To override this value please set as a JSON array                                   | REACT_APP_PINGWEBSITES         | N/A         | See config/ping.ts      |
| Speed test interval                       | How frequently to check network speed (latency, jitter, upload &  download speed)                                                                                                             | REACT_APP_TESTINTERVAL         | Seconds     | 300                     |
| Speed test servers                        | List of speed test servers to use to test speed against.  Configuration is passed to Librespeed, for more info see the config or Librespeed/speedtest website                                 | REACT_APP_SERVERCONFIGURATIONS | N/A         | See config/speedtest.ts |
| Upload warning threshold                  | Threshold at which the color of the upload status will be shown as a  warning.  Example: if after a speed test the upload speed is less than  `REACT_APP_UPLOADWARN` then display as warning  | REACT_APP_UPLOADWARN           | Mbit/s      | 4                       |
| Upload error threshold                    | Same as above except for displaying as error status                                                                                                                                           | REACT_APP_UPLOADERROR          | Mbit/s      | 1                       |
| Download warning theshold                 |                                                                                                                                                                                               | REACT_APP_DOWNLOADWARN         | Mbit/s      | 8                       |
| Download error threshold                  |                                                                                                                                                                                               | REACT_APP_DOWNLOADERROR        | Mbit/s      | 1                       |
| Latency warning threshold                 |                                                                                                                                                                                               | REACT_APP_LATENCYWARN          | ms          | 40                      |
| Latency error threshold                   |                                                                                                                                                                                               | REACT_APP_LATENCYERROR         | ms          | 100                     |
| Jitter warning threshold                  |                                                                                                                                                                                               | REACT_APP_JITTERWARN           | ms          | 50                      |
| Jitter error threshold                    |                                                                                                                                                                                               | REACT_APP_JITTERERROR          | ms          | 100                     |
All configurables can be found under src/config/*.ts

#### Example usage
Set speed test interval to 10mins, ping checks every 1min, & latency warn threshold to 20ms
```
sudo docker run --name netspeed -d --restart=always -p 80:80 --env REACT_APP_TESTINTERVAL=600 --env REACT_APP_PINGINTERVAL=60 --env REACT_APP_LATENCYWARN=20 ryandev/netspeed:arm64
```

### Attributions
- [Librespeed](github.com/librespeed/speedtest)
- [FontAwesome](fontawesome.com)
- [Bootstrap](getbootstrap.com)
- [Inconsolata - Google fonts](https://fonts.google.com/specimen/Inconsolata?query=consol&preview.text=NetSpeed&preview.text_type=custom)


License
----

MIT
