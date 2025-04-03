# dave-environment

**WARNING: this is not for production use, only for local tests!**

This repo provides the supporting environment for dave running in docker and helps to setup a locally running sandbox.

## License notes; cf. [LICENSE](https://github.com/koecher/dave-environment/blob/main/LICENSE)

```
MIT License

Copyright (c) 2025 Freie und Hansestadt Hamburg, Behörde für Verkehr und Mobilitätswende, Landesbetrieb Straßen, Brücken und Gewässer (LSBG)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
``` 

**Please contact the developers of City of Munich in case of trouble with the dave-components. The origin repos are here: https://github.com/it-at-m**

This repo is being maintained by City of Hamburg, (Freie und Hansestadt Hamburg, Behörde für Verkehr und Mobilitätswende, Landesbetrieb Straßen, Brücken und Gewässer, Geschäftsbereich X, Fachbereich XR DigiLab without any warrenty or support, as given by the license, by the developers:

* Dr.-Ing. Uwe Köcher, Software-Architekt, LSBG GB X FB XR
* Anirban Dutta, Werkstudent Dev-Team, LSBG GB X FB XR

## Prerequisites

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
  * Optional: https://formulae.brew.sh/formula/postgresql@17 installed for the tools but do not run the service since this will block the port `5432`
* Optional: pgAdmin, https://www.pgadmin.org, for PostgreSQL database inspection

## clone the repo & start the containers

```
git clone https://github.com/koecher/dave-environment.git
cd dave-environment
docker-compose up -d
```

## keycloak

### inital setup

* start a webbrowser and open the keycloak management console on http://localhost:8080
* use the default login with `admin` and `admin`
* in the keycloak master realm, setup your personal admin-user:
  * create a new user (at least give a username),
  * open "Credentials" tab and set a password,
  * open "Role mapping" tab, click on the button "Assign role", switch "Filter by client" to "Filter by realm roles", select "admin" (role_admin) and finally click on the "Assign" button
* logoff keycloak and login with your new admin-user
* delete the default "admin" user

### inital realm "dave" setup

* start a webbrowser and open the keycloak management console on http://localhost:8080
* login with your user with admin rights, as setup above
* click in the left pane on "Keycloak, master" and then on the button "Create realm", then
  * "Ressource file" click on browse and navigate to the folder of this repo, select the file `dave-environment/keycloak-realm-dave/dave_example_keycloak_realm_dave.json`
  * check that the realm-name is detected as "dave", make sure that the "Enabled" switch is set to "ON" and click on the button "Create"
  * click in the left pane on "Clients" and click on the client "dave"
  * open "Credentials" tab and click on the button "Regenerate" for the "client secret"; This secret will be used later!
  * open "Users" from the left pane of the realm "dave", add the users: "anwender", "fachadmin", "poweruser" and "external", each of them with
    * "Email verified" switch on, set the fields username, email, first and last name and then click on the button "Create".
    * open "Credentials" tab and set a password
    * open "Role mapping" and add the corresponding role, i.e. "ANWENDER", "FACHADMIN", "POWERUSER" or "EXTERNAL", respectively.
  * NOTE: we have given here, for this sandbox example, in the client "dave" a composite role for "ANWENDER", i.e. precisely giving all other roles such that you can use the user "anwender" through all portals for checking the overall workflow. This needs to be adapted for a production system.
