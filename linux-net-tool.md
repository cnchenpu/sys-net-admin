# Linux Network Tools

## nc, ncat
```nc```, ```ncat``` reads and writes data across the network from the command line.
### 1. Enable communication between a client and a server.
 - Set a client machine to listen for connections on TCP port 8000:
   ```bash
   $ nc -l 8000
   ```
 - On a server machine, specify the IP address of the client and use the same port number: 
   ```bash
   $ nc 192.168.1.205 8000
   ```
- Then you can send messages on either side of the connection and they appear on both local and remote machines. 
    
### 2. Send file.
   - On a client machine, to listen a specific port transferring a file to the server machine: 
     ```bash
     $ nc -l 8000 > outputfile
     ``` 
   - On a server machine, specify the IP address of the client, the port and the file which is to be transferred: 
     ```bash
     $ nc 192.168.1.205 8000 < inputfile
     ``` 
   - After the file is transferred, the connection closes automatically.  

