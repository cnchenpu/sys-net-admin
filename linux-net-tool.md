# Linux Network Analysis Tools

## nc (ncat)
![](fig/ncat.jpg)

```nc``` (```ncat```) reads and writes data across the network from the command line.

- Listen for connections on TCP port 8080.
  
  ```nc -l 8080```

- Send a file over TCP port 9899 from host2 (client) to host1 (server).
  
  ```HOST1$ nc -l 9899 > outputfile```

  ```HOST2$ nc HOST1 9899 < inputfile```

- Transfer in the other direction, turning nc into a "one file" server.

  ```HOST1$ nc -l 9899 < inputfile```

  ```HOST2$ nc HOST1 9899 > outputfile```

- Enable communication between host1 (client) and host2 (server).
 
  ```HOST1$ nc -l 8000```

  ```HOST2$ nc HOST1 8000```

- Port scanning.
  
  ```$ nc -z host.example.com 20-30```

  