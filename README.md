# CSWebServerDetection (Client Side Web Server Detection)

CSWebServerDetection is a library written in JavaScript/JQuery to detect webservers on your local network (needs some requirements).  

### Use Cases
To be able to write your interface and host it seperate from your back end application. You can make a service acting as a http web server which you use to communicate with your web interface and control the application. (Interfaces can look much better if they are written in html/js :D). It also improve's cross platform compatibility if you write your backend in Java or in C# mono. You can make it run 24/7 on your RPI for example. The user does not need to remember the ip of your Raspberry Pi but can just use a url to your webhost where you host your interface, and the interface can detect the rpi automatically. 

### Requirements
Your http web server needs to send a response header containing the following field: `Access-Control-Allow-Origin: *`. You can leave the `*` as is or fill in your interface host url. 

This library only works in Chrome, Mozilla and Opera. Android and iOS browsers are supported as well. If you suspect that your users will use different browsers, you will have to let them fill in the base ip manually with the following function. 

    //example - base ip means the first three digit of your local ip, this has to be the same for the local client as for the local server
    .setBaseIp("192.168.33");

### Usage
Include the library through the cdn listed below or download it and include it in your headers:

    <script src="CSLocalServerDetection.js"></script>

And this is how you use it:
  
    var detectServers = new ClientSideServerDetection();
    
    //you can add multiple ports to check, but be aware that every new port to check increases search time significantly,
    //better check one port if possible.
    //if this is not set, default ports that will be checked are "80" and "8080"
    detectServers.setPorts(["80", "1234"]);
    
    //runs the detection twice for every partial
    detectServers.setPartials(["/", "/isthisalocalserver"]);
    
    //run the server detection, parameter is a callback function
    //you can run this multiple times
    detectServers.runDetection(gotServers);
    
    //callback function, needs a parameter which will contain server data *read more down below
    //in its current state, the callback function will be ran every single time a new unique server has been detected
    function gotServers(serverInfo){
        console.log(servers);
    }
   
   
   
### Server Data
The callback function you use to run when a server has been found needs a parameter that will be used to passthrough data about the found servers.
This parameter will turn into an array containing objects with information about the found servers, structured as follows:

On 200/206 OK:  {ip, port, partial, data}

    - ip: ip of the found server
    - port: port on where the server is running
    - partial: if partial (/home.html for example) it will show here which partial
    - data: data received, could be content of the html page requested if you did request a html page as partial

On XXX (some error): {ip, port, partial, data}

    - ip: ip of the found server
    - port: port on where the server is running
    - partial: if partial (/home.html for example) it will show here which partial
    - data: xhr data, which you can use to resolve the error
    
### Other Functions
You can set a number of things:
 
    .setTimeout(timeout)        //sets timeout for every individual ajax request where timout is a integer
    .setPortRange(start, end)   //you can check multiple ports at once (NOT RECOMMENDED), always try to check only one or two ports
    .setStartEnd(start, end)    //sets the start/end position of the ips to check, example:(5,100)->(192.168.111.5 -> 192.168.111.100)
    .setPorts(ports)            //sets the ports to check, requires string array, example: ["80", "443"]
    .setPartials(paritals)      //sets the partials to check, requires string array, example: ["/", "/home"]
    .setBaseIp(baseip)          //sets the base ip in case auto detection of base ip is not working, requires string, example: 192.168.111

### Tech

CSWebServerDetection uses a number of open source projects to work properly:

**Services:**
* [RawGit](https://rawgit.com/) - making CDN through GitHub available

**Scripts:**
* [jQuery](https://jquery.com/) - duh
* [webrtc-ips](https://github.com/diafygi/webrtc-ips) - a modified version of this, to retreive your local ip

### Development
This is programmed in a bit of a hurry, as I need it for another project that I am working on and I did not want to delay its development by putting all my time and mind into this. I am not really up for making this better/faster/more efficient than it already is. But feel free to do so yourself. I would like a bit of credit but you do not have to give me credit, it's up to you.

License
----

MIT


**Free Software, Hell Yeah!**
