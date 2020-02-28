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

# HDP-Ambari Rest API

## Note all the host components associated with the service
`curl -u admin:admin -H "X-Requested-By: ambari" -X GET  http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICENAME`

## Ensure the service is stopped (you can use the Ambari Web-UI to stop the service as well)

Stop the whole service (ensure correct values are provided for AMBARI_SERVER_HOST, SERVICE_NAME):<br>
`curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Service"},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICE_NAME`

### Stop individual components (ensure correct values are provided for AMBARI_SERVER_HOST, HOSTNAME, COMPONENT_NAME): 
`curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Component"},"Body":{"HostRoles":{"state":"INSTALLED"}}}' http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/hosts/HOSTNAME/host_components/COMPONENT_NAME`

### Stop all component instances (ensure correct values are provided for AMBARI_SERVER_HOST, SERVICE_NAME, COMPONENT_NAME) <br>
- just another way to stop Service members:<br>
`curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop All Components"},"Body":{"ServiceComponentInfo":{"state":"INSTALLED"}}}' http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICE_NAME/components/COMPONENT_NAME`

## Delete the whole SERVICE
`curl -u admin:admin -H "X-Requested-By: ambari" -X DELETE  http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICENAME`

## Removing a Service (1.2.3)

Note: These API calls do not uninstall the packages associated with the service and neither they remove the config or temp folders associated with the service components. <br>

Before the PUT or DELETE calls, you can do a GET to ensure that the API is referring to a valid resource.<br><br>

###  Note all the host components associated with the service.
`curl -u admin:admin -X GET  http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICENAME`

### Ensure the service is stopped (you can use the Ambari Web-UI to stop the service as well)
Stop the whole service (ensure correct values are provided for AMBARI_SERVER_HOST, SERVICE_NAME):<br>
`curl -u admin:admin -X PUT -d '{"RequestInfo":{"context":"Stop Service"},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICE_NAME`

### Stop individual components (ensure correct values are provided for AMBARI_SERVER_HOST, HOSTNAME, COMPONENT_NAME): 
`curl -u admin:admin -X PUT -d '{"RequestInfo":{"context":"Stop Component"},"Body":{"HostRoles":{"state":"INSTALLED"}}}' http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/hosts/HOSTNAME/host_components/COMPONENT_NAME`

## Put MASTER and SLAVE components into MAINTENANCE mode
`curl -u admin:admin -X PUT -d '{"HostRoles": {"state": "MAINTENANCE"}}' http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/hosts/HOSTNAME/host_components/COMPONENTNAME`

## Delete MASTER and SLAVE components
`curl -u admin:admin -X DELETE  http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/hosts/HOSTNAME/host_components/COMPONENTNAME`

## Delete the whole SERVICE
`curl -u admin:admin -X DELETE  http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/services/SERVICENAME`

## Removing a Host (2.1.0)
The preferred way to remove a host is to move the master services from the host, decommission the slave nodes and then remove the host after deleting all the host components. However, there are situations such as the host is lost and cannot be brought back online for graceful removal. Under these circumstances the following API calls can be used to clean up the host. <br>

### Get a list of host components mapped to the host
`curl -u admin:admin -H "X-Requested-By: ambari" -X GET  http://AMBARI_SERVER_HOST:8080/api/v1/clusters/c1/hosts/HOSTNAME`

### DELETE all host components mapped to this host
E.g. Delete DATANODE<br>
`curl -u admin:admin -H "X-Requested-By: ambari" -X DELETE http://AMBARI_SERVER_HOST:8080/api/v1/clusters/CLUSTERNAME/hosts/HOSTNAME/host_components/DATANODE`

### DELETE the host
`curl -u admin:admin -H "X-Requested-By: ambari" -X DELETE http://AMBARI_SERVER_HOST:8080/api/v1/clusters/CLUSTERNAME/hosts/HOSTNAME`
