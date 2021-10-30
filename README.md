# Sitecore 10 XP0 with xDB and ID Server disabled

An example repo for running Sitecore 10 XP0 topology on Docker. The build will layer in SPE, this is a minimum requirement for all our projects.

Additionally both xDB and ID Server are disabled. xConnect service instances are scaled down to 0 to save additional CPU/RAM for local development purposes. This provides a minimal development environment for most purposes.
# Port Issues

Anti-virus and other programs conflict with default ports. To check if the port is in use `netstat -a | findstr 8888` in power shell wiith `8888` the port in question. Sitecore lists ports in [use here](https://doc.sitecore.com/en/developers/101/developer-tools/run-your-first-sitecore-instance.html):

- 80
- 443
- 8079
- 8081 
- 8984
- 14330

I am actively working on changing the ports but the list but a simple `grep` doesn't seem to expose all ports and it appears that some ports for some reason need to be those ports. The biggest offender is McAfee and IIS. IIS can be disabled with `NET STOP IISADMIN` but McAfee needs to be rerouted.

# Sitecore Development Files

This is a development environment and utilizes the new Sitecore patches to speed up development. Read more [https://github.com/Sitecore/docker-tools](https://github.com/Sitecore/docker-tools), this is already installed.

# Getting Started

- Copy your license.xml file to the ./docker/license folder of topology of your choice
- *Run start.ps1*
- First time you may be prompted to install a root certificate authority (CA) certificate. This is required by mkcert to allow - local SSL certs to be used without warnings and errors. Click yes to install the certificate.

# Deployment

Deploy your files to ./docker/data/cm/website folder.

# TODO / Nice to have

This is rough and a POC to make sure it works in the corporate environment. In my free time I will:

- Add license and other items to the Sitecore build. I have requested access to the Sitecore repository the week of 10/25 and will follow up after my VM is complete.
- Add SXA and JSS, this will be simple.
- Ideally on DCE Sitecore builds add those files into the CM Dockerfile image, This is straightforward and I have a POC using GitLab's CI which should be the same as. The end result is that the image pull will have SXA, the latest DCE built into the image.
- Incorporate Bas Lijten's DACPAC to the build process [https://blog.baslijten.com/test-and-demo-environments-in-an-intsant-how-to-pre-provision-content-to-the-master-and-web-database-in-sitecore-containers-in-5-simple-steps/](https://blog.baslijten.com/test-and-demo-environments-in-an-intsant-how-to-pre-provision-content-to-the-master-and-web-database-in-sitecore-containers-in-5-simple-steps/) to speed up deployment

# N.B. This is for development purposes only

I have purposely stripped production RBAC and other non-necessary components. In the end the workflow should be exactly the same as deploying to `inetpub`. Adding XConnect, and other modules are not necessary but when or if needed will just simply be a line. Upgrades in the future will now be as simple as changing this in the `.env` line: `SITECORE_VERSION=10.1-ltsc2019`