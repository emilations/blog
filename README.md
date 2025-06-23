This is a blog repo to follow with my homelab and coding project

## 2025-06-18
Discovered nginx proxy manager. Ran it over in a docker container and was able to create a self signed certificate towards cloudglare.
The solution is incredible because I can finally remove the security warning from all my homelab services (been dreaming of this moment for the last 4 years). 
## 2025-06-20
### Nginx proxy manager
Currently adding all my services to pass through the proxy manager. Some services such as home automation are causing an http error.
Forums are saying that home assistant is not configured to run properly with a proxy out of the box. So I need to install a code editor in HA and add this to the configuration file:
```yaml
http:
    use_x_forwarded_for: true
    trusted_proxies:
    - x.x.x.x/24
```
Successfully added all the 30+ services and had an issue with the hp printer web services. The printer web portal seems to need a specific configuration to forward to a proxy. Will probably deal with it on a sunny day. I also learned that you can only reverse proxy and forward services with ports. Meaning that one cannot just forward a machine or vm ip address for RDP or ssh. For that I will probably need to use the already installed adguard home dns. It is a shame since I wanted to consolidate the subdomains assignments to one solution.

### Github agent
Added my first github runner today. I had the choice between having a runner controler that is deployed to a k8s cluster instead. I chose to start experimenting with a single agent for now and then upgrade if need be. The agent runs in a newly created VM inside proxmox. For now, I want to test out building docker images and pushing them to the self hosted docker container registry.
![alt text](image.png)


## 2025-06-21
### Github action
Troubleshooting build errors with a newly created github workflow. This is my first workflow and had some issues with adding permissions to the user being used by the runner. I had to give admin permission to the user to use docker without having to sudo. In addition, the docker build needs the project to have been already been built by dotnet. However, the runner does not have dotnet installed. So instead of installing it manually like I did with docker, github actions supports the using:dotnet inside a step. My current issue is that the using dotnet is also returning a permission error.

![alt text](image-1.png)

Reading about dotnet action issue seems to have been patched in 2022 https://github.com/actions/setup-dotnet. They introduced an env variable DOTNET_INSTALL_DIR: /home/[targetUser]/.dotnet to set if the action user does not have root permissions. This fixed my issue.

## 2025-06-21
### Dockerfile
Successfully built the first image using github actions and dockerfile. The docker file gave me many errors that had to be troubleshooted during many trial and errors. I had to maybe try 20 different commits for the first image to be created. My next step would be determine if building the project is better suited inside a dockerfile or a github action. Another next step would be to push the images to the docker local registry.

## 2025-06-23
### C#
Created two projects in a solution called sigma. The solution is mostly to help me practice creating C# projects from scratch instead of maintaining one already build and setup. The solution carries two projects for now, one called sigma-database, and one called sigma-api. The database one is a code first db that defines the db context. And the api one is where the REST controllers are located. I came across an issue with my VPN today while coding at a coffee shop. My sql server is hosted on my servers with a dns record dev.blue.paraluno.com and this record is used to define the projects connection strings. Meaning, that although my vpn is instructed to use my internal DNS, the DNS is not resolving. Will probably take the occasion to set up a new openvpn server since the one I have is depreciated.
