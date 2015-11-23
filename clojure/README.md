# RentPath Clojure Style Guide

This is the style guide for the Clojure team at RentPath.

In this document are links to links to specific guides and to applications and libraries maintained by the RentPath Clojure team.

Currently all styles are text only. In the future, perhaps they will be enforced in code.

## Specific guides 
- [Database Migrations](migrations.md)
- [Libraries used](libraries.md)
- [Libraries being considered](tech-radar.md)

## Clojure Applications
- [ag-console-server](https://github.com/rentpath/ag-console-server) - Server side of application for setting up and configuring ag-sites.
- [ag-console](https://github.com/rentpath/ag-console) - Client side of application for setting up and configuring ag-sites.
- [message-api](https://github.com/rentpath/message-api) - Application for proxying emails for lead submission
- [lease-pulse-api](https://github.com/rentpath/lease-pulse-api) Application providing an API for leasepulse Android, IOS and Web Clients

## Internal Clojure Libraries
- [paul-revere](https://github.com/rentpath/paul-revere) - Library for notifying Sentry of errors
- [clj-ops](https://github.com/rentpath/clj-ops) - Library implementing ops routes. 
- [clj-config](https://github.com/rentpath/clj-config) - Library for reading config files
- [clj-infra](https://github.com/rentpath/clj-infra) - Library for reading ini files and outputting as text. Used to keep local copies of .env in sync with version maintained by infra
