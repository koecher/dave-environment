# dave-environment

**WARNING: this is not for production use, only for local tests!**

This repo provides the supporting environment for dave running in docker and helps to setup a locally running sandbox.

## prerequisites

### operating system

* macos 15.4, M-architecture (arm64), tested

### localhost (127.0.0.1) open ports needed

localhost ports must be free (not being used by your system or any other software):

* docker-compose components (of this repo), make sure that you don't have any of them currently used:
  * `8080` for keycloak
  * `9200` (and `9300`) for elasticsearch
  * `5601` for kibana
  * `5432` for postgresql
* dave components
  * `39146` for dave-backend
  * `8082` (and `8081` for dev) for dave-frontend
  * `8083` (and `8085` for dev) for dave-admin-portal
  * `8084` (and `8086` for dev) for dave-selfservice-portal
  * `xxxx`, `yyyy` for dave-eai (not needed for this sandbox) 
 
### dependencies

make sure to have:

* Xcode and commandline tools installed
* docker and docker-compose installed on your system, e.g. via docker-desktop and keep care if you have the license for your usage
* Homebrew installed; cf. https://brew.sh

## clone the repo & start the containers

```
git clone https://github.com/koecher/dave-environment.git
cd dave-environment
docker-compose up -d
```

## keycloak setup

* start a webbrowser and open the keycloak management console on http://localhost:8080
* use the default login with `admin` and `admin`
* in the keycloak master realm, setup your personal admin-user:
  * create a new user (at least give a username),
  * open "Credentials" tab and set a password,
  * open "Role mapping" tab, click on the button "Assign role", switch "Filter by client" to "Filter by realm roles", select "admin" (role_admin) and finally click on the "Assign" button
* logoff keycloak and login with your new admin-user
* delete the default "admin" user


