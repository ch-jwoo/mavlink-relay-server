# mavlink-relay-server
Mavlink relay server that route mavlink packet.

## submodule
mavlink/c_library   
this help mavlink packet to parse.   
see https://github.com/mavlink/c_library_v2

## usage
First, go to this folder. And then,   

    make
    ./mavlink_relay_server {TCP_PORT=20250} {UDP_PORT=20200}

TCP and UDP is used for connecting GCSs and vehicles relatively.
   
**argument examples:**   
   
TCP_PORT = 20250 (default), UDP_PORT = 20200 (default)

    ./mavlink_relay_server
    
TCP_PORT = 20000, UDP_PORT = 20200 (default)   

    ./mavlink_relay_server 20000
    
TCP_PORT = 20000, UDP_PORT = 19000

    ./mavlink_relay_server 20000 19000
    
## explanation
This server connect to GCS using TCP socket. 
On the other hand, It use UDP socket for communicate with vehicle. 
I use MAVROS's connection URL for sending packet to server. 
But, if I use TCP URL, MAVROS didn't work when server was not executed. 
So, I use UDP on server in order to working MAVROS when server is not used.   

About ROS connection URL : https://github.com/mavlink/mavros/blob/master/mavros/README.md
   

## At Vehicle (ROS)
In this project, It is used that 'Jetson Nano' communicate with pixhawk using UART. 
And mavros is used on Jetson Nano that instal LTE dongle for send packet to server.

    roslaunch mavros px4.launch fcu_url:="udp://:14540@{your server's IP}:{your server's UDP port}"

If you need more information of mavros see https://github.com/mavlink/mavros/tree/master/mavros   

## At GCS (QgroundControl)
You can set UDP connection at QGC as follow.   
   
1. exec QGC
2. Application SEttings
3. Comm Links
4. Add
5. Type to TCP
6. Set Host Address (your server's fixed IP address), TCP Port (port number you set above)
7. Click OK
8. Select slot and click Connect
