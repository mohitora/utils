# Utils

## Dynamic Port Forwarding


`ssh -C -D 1080 <node_name>`
 - -D option specifies dynamic port forwarding. 
 - 1080 is the standard SOCKS port. (Although you can use any port number, some programs will only work if you use 1080.)
 - -C enables compression, which speeds the tunnel up when proxying mainly text-based information (like web browsing), but can slow it down when proxying binary information (like downloading files).

Next you would tell Firefox to use your proxy:

go to Edit -> Preferences -> Advanced -> Network -> Connection -> Settings...

- check "Manual proxy configuration"
- make sure "Use this proxy server for all protocols" is cleared
- clear "HTTP Proxy", "SSL Proxy", "FTP Proxy", and "Gopher Proxy" fields
- enter "127.0.0.1" for "SOCKS Host"
- enter "1080" (or whatever port you chose) for Port.
