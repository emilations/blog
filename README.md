This is a blog repo to follow with my homelab and coding project

## 2025-06-18
Discovered nginx proxy manager. Ran it over in a docker container and was able to create a self signed certificate towards cloudglare.
The solution is incredible because I can not remove the security warning from all my homelab services. 
## 2025-06-20
Currently adding all my services to pass through the proxy manager. Some services such as home automation are causing an http error.
Forums are saying that home assistant is not configured to run properly with a proxy out of the box. So I need to install a code editor in HA and add this to the configuration file:
```yaml
http:
    use_x_forwarded_for: true
    trusted_proxies:
    - x.x.x.x/24
```
Successfully added all the 30+ services and had an issue with the hp printer web services. The printer web portal seems to need a specific configuration to forward to a proxy. Will probably deal with it another time.
