## Guide to install ModSecurity for Nginx on Rocky Linux 10

This guide will try to guide you to installing all dependancies onto a minimal base install of Rocky Linux. 

Please make sure you have the following installed 

- Nginx
- EPEL repository

If not ? Install them first:

```
dnf install nginx
EPEL?? Not sure if we really need it
```
At the time of writing the latest release in the BaseOS repository is 1.26.3-1.el10.x86_64
As the purpose of this writing is not getting NGINX up and running, please make sure that part works before you continue.

For actually installing modSecurity you need 2 components

- ModSecurity
- The connection layer between NGINX and ModSecurity

Both will need to be built on your system. There is no prebuilt package. So you will need packages to build and compile software. Don't be scared you can do this!

First we are going to clone both components to your system that is running nginx

Install git
```
dnf install git
```
Clone both repositories
```
git clone --recursive https://github.com/owasp-modsecurity/ModSecurity ModSecurity
git clone --recursive https://github.com/owasp-modsecurity/ModSecurity-nginx ModSecurity-nginx
```
As the nginx layer depends on ModSecurity that one needs to be built first. To build we need tools.
```
dnf install make autoconf libtool
```
First we will need to make sure that all the dependancies are met. If we don't the compile will fail or some features will be missing. Below is a quote from the Github page: 

> This library is written in C++ using the C++17 standards. It also uses Flex and Yacc to produce the “Sec Rules Language” parser. Other, mandatory dependencies include YAJL, as ModSecurity uses JSON for producing logs and its testing framework, libpcre (not yet mandatory) for processing regular expressions in SecRules, and libXML2 (not yet mandatory) which is used for parsing XML requests.

> All others dependencies are related to operators specified within SecRules or configuration directives and may not be required for compilation. A short list of such dependencies is as follows:

> libinjection is needed for the operator @detectXSS and @detectSQL
> curl is needed for the directive SecRemoteRules.
> If those libraries are missing ModSecurity will be compiled without the support for the operator @detectXSS and the configuration directive SecRemoteRules.

So lets go and find:
- JAYL
- libpcre
- libXML2
- libinjection

```
CD into the ModSecurity directory. You will find a ```build.sh``` file there. 
```
./build.sh
```
This will create the neccesary files to actually build the ModSecurity library. 
