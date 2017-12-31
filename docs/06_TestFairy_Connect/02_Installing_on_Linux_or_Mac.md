
## Server requirements
In order to install TestFairy Connect, you will need a linux server with
- Nodejs 4.2+
- Git 1.7+

This server will need permissions to access your JIRA server, and to access the TestFairy cloud.

For Team Foundation Server (TFS) integration, please refer to [that documentation](https://docs.testfairy.com/TestFairy_Connect/Configuring_TFS.html) for additional requirements. 

## Installation

Installation is done via npm, please run the following command in a folder for your TestFairy Connect installation.

```
npm install git+https://github.com/testfairy/testfairy-connect
```

## Configuration

TestFairy Connect has a built-in configuration wizard. This wizard will guide you through configuration of API key, http proxy, credentials and so on.

To start the configuration wizard, simply run the following command:

 ```
 mkdir ~/.testfairy-connect
 ```
 ```
 node node_modules/testfairy-connect/service.js configure
 ```

## Running TestFairy Connect

To start the TestFairy Connect agent, simply run the following command:

 ```
 node node_modules/testfairy-connect/service.js run
 ```
   

