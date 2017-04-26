# loopback-ssl

Node module to enable HTTPS/SSL in a [loopback] application with simple configurations. The module also enables trusted peer authentication.

[![Travis](https://img.shields.io/travis/yantrashala/loopback-ssl/master.svg)](https://travis-ci.org/yantrashala/loopback-ssl) [![npm](https://img.shields.io/npm/dm/loopback-ssl.svg)](https://www.npmjs.com/package/loopback-ssl) [![npm](https://img.shields.io/npm/dt/loopback-ssl.svg)](https://www.npmjs.com/package/loopback-ssl) [![npm](https://img.shields.io/npm/l/loopback-ssl.svg)](https://github.com/yantrashala/loopback-ssl) [![David](https://img.shields.io/david/dev/yantrashala/loopback-ssl.svg?style=plastic)](https://www.npmjs.com/package/loopback-ssl) [![David](https://img.shields.io/david/yantrashala/loopback-ssl.svg?style=plastic)](https://www.npmjs.com/package/loopback-ssl) [![Codacy Badge](https://api.codacy.com/project/badge/Grade/74ddc643152f4f439d6ef7d99ed9d5f6)](https://www.codacy.com/app/siddhartha-lahiri/loopback-ssl?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=yantrashala/loopback-ssl&amp;utm_campaign=Badge_Grade) [![Codacy Badge](https://api.codacy.com/project/badge/Coverage/74ddc643152f4f439d6ef7d99ed9d5f6)](https://www.codacy.com/app/siddhartha-lahiri/loopback-ssl?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=yantrashala/loopback-ssl&amp;utm_campaign=Badge_Coverage) [![GitHub issues](https://img.shields.io/github/issues/yantrashala/loopback-ssl.svg)](https://github.com/yantrashala/loopback-ssl/issues)
[![Join the chat at https://gitter.im/yantrashala/loopback-ssl](https://badges.gitter.im/yantrashala/loopback-ssl.svg)](https://gitter.im/yantrashala/loopback-ssl?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


# Features
- Enable SSL in Loopback application
- Enable mutual SSL authentication in Loopback

# Usage

## Setup

### Install [loopback]:
```js
slc loopback <app-name>

cd <app-name>
```

### Install [loopback-ssl]:
```js
npm install loopback-ssl --save
```

### Setup Configuration:
Add the following lines of configuration in 'config.json' in location "\<app-dir\>/server/config.json"
```js
  "httpMode": false,
  "certConfig": {
    "path": "/certificate/path/",
    "key": "local.pem",
    "cert": "local.crt.pem",
    "ca": [],
    "requestCert": false,
    "rejectUnauthorized": false
  }
```

### Configure server.js
Edit the server.js located at "\<app-dir\>/server/server.js"

Add the following line at the top of the code.
```js
var loopbackSSL = require('loopback-ssl');
```

Replace the all the content within the code block starting with
```js
app.start = function() { ... }

boot(app, __dirname, function(err) { ... });
```

with following code
```js
boot(app, __dirname, function(err) {
  if (err) throw err;
});

return loopbackSSL.startServer(app);
```
### Example server.js
For example, the bare minimum server.js should look like
```js
var loopback = require('loopback');
var boot = require('loopback-boot');
var loopbackSSL = require('loopback-ssl');

var app = module.exports = loopback();

boot(app, __dirname, function(err) {
  if (err) throw err;
});

return loopbackSSL.startServer(app);
```

## Option 1: HTTP (default loopback configuration)
The configuration entry `"httpMode": true` will enable http (disable https). In this mode the `"certConfig": {..}` configuration is not required and can be omitted.
```js
  "httpMode": true
```

## Option 2: HTTPS
The configuration entry `"httpMode": false` will enable https.
```js
  "httpMode": false,
  "certConfig": {
    "path": "/certificate/path/",
    "key": "serverkey.pem",
    "cert": "server-certificate.pem",
    "ca": [],
    "requestCert": false,
    "rejectUnauthorized": false
  }
```
- `"path"` - folder location where the certificates files will be installed
- `"key"` - server key
- `"cert"` - server certificate

## Option 2.1: HTTPS (Generate self signed keys on startup - no pem files)
The configuration entry `"httpMode": false` will enable https.

### Note:
- To enforce generation of self signed certificates dynamically on startup, the ```key```, ```cert``` and ```ca```  options must be  empty and ```requestCert``` & ```rejectUnauthorized``` must be ```false```
- This option will not work if Mutual SSL (Option 3) is enabled.

```js
  "httpMode": false,
  "certConfig": {
    "path": "/anything/",
    "key": "",
    "cert": "",
    "ca": [],
    "requestCert": false,
    "rejectUnauthorized": false
  }
```

## Option 3: Enable Mutual SSL authentication

```js
  "httpMode": false,
  "certConfig": {
    "path": "/certificate/path/",
    "key": "serverkey.pem",
    "cert": "server-certificate.pem",
    "ca": [
        "client-certificate-to-validate.pem"
    ],
    "requestCert": true,
    "rejectUnauthorized": true
  }
```
- The `ca[]` configuration contains the list of client certificates which the server will authenticate
- `"requestCert": true` enables mutual SSL authentication
- `"rejectUnauthorized": true` enables the authenticity and validity check of client keys
- For any reason, if the client certificate is a self signed certificate, `"rejectUnauthorized":` can be set to `false`.

# License

[MIT](./LICENSE).

# See Also

- [Self Signed Certificates - Example][self_signed]

[loopback]: http://loopback.io
[loopback-ssl]: https://www.npmjs.com/package/loopback-ssl
[trusted_peer]: https://github.com/coolaj86/nodejs-ssl-trusted-peer-example
[self_signed]: https://github.com/coolaj86/nodejs-self-signed-certificate-example

# Contributing to Yantrashala/loopback-ssl

We welcome contributions, but request you follow these guidelines.
 - [Raising issues](#raising-issues)
 - [Feature requests](#feature-requests)
 - [Pull-Requests](#pull-requests)

This project adheres to the [Contributor Covenant 1.4](http://contributor-covenant.org/version/1/4/).
By participating, you are expected to uphold this code. Please report unacceptable
behavior to any of the [project's core team].

## Raising issues

Please raise any bug reports on the relevant project's issue tracker. Be sure to
search the list to see if your issue has already been raised.

A good bug report is one that make it easy for us to understand what you were
trying to do and what went wrong.

Provide as much context as possible so we can try to recreate the issue.
If possible, include the relevant part of your flow. To do this, select the
relevant nodes, press Ctrl-E and copy the flow data from the Export dialog.

At a minimum, please include:

 - Version of loopback-ssl - either release number if you downloaded a zip, or the first few lines of `git log` if you are cloning the repository directly.
 - Version of node.js - what does `node -v` say?

## Feature requests

For feature requests, please raise them on the [issues page](https://github.com/yantrashala/loopback-ssl/issues) with label "enhancement".

## Pull-Requests

If you want to raise a pull-request with a new feature, or a refactoring
of existing code, it may well get rejected if you haven't discussed it on
the [issues page](https://github.com/yantrashala/loopback-ssl/issues) first.

### Coding standards

Please ensure you follow the coding standards used through-out the existing
code base. Some basic rules include:

 - all files must have the Apache license in the header.
 - indent with 4-spaces, no tabs. No arguments.
 - opening brace on same line as `if`/`for`/`function` and so on, closing brace
 on its own line.

 ## Attribution

 This Contributing article is adapted from node-red, version 0.16.2, available at https://github.com/node-red/node-red/blob/master/CODE_OF_CONDUCT.md

# Yantrashala - Contributor Covenant Code of Conduct

## Our Pledge

In the interest of fostering an open and welcoming environment, we as contributors and maintainers pledge to making participation in our project and our community a harassment-free experience for everyone, regardless of age, body size, disability, ethnicity, gender identity and expression, level of experience, nationality, personal appearance, race, religion, or sexual identity and orientation.

## Our Standards

### Examples of behavior that contributes to creating a positive environment include:
- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

### Examples of unacceptable behavior by participants include:
- The use of sexualized language or imagery and unwelcome sexual attention or advances
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information, such as a physical or electronic address, without explicit permission
- Other conduct which could reasonably be considered inappropriate in a professional setting

## Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable behavior and are expected to take appropriate and fair corrective action in response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or reject comments, commits, code, wiki edits, issues, and other contributions that are not aligned to this Code of Conduct, or to ban temporarily or permanently any contributor for other behaviors that they deem inappropriate, threatening, offensive, or harmful.

## Scope

This Code of Conduct applies both within project spaces and in public spaces when an individual is representing the project or its community. Examples of representing a project or community include using an official project e-mail address, posting via an official social media account, or acting as an appointed representative at an online or offline event. Representation of a project may be further defined and clarified by project maintainers.

## Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be reported by contacting the project team at [INSERT EMAIL ADDRESS]. All complaints will be reviewed and investigated and will result in a response that is deemed necessary and appropriate to the circumstances. The project team is obligated to maintain confidentiality with regard to the reporter of an incident. Further details of specific enforcement policies may be posted separately.

Project maintainers who do not follow or enforce the Code of Conduct in good faith may face temporary or permanent repercussions as determined by other members of the project's leadership.

## Attribution

This Code of Conduct is adapted from the Contributor Covenant, version 1.4, available at http://contributor-covenant.org/version/1/4.

[homepage]: http://contributor-covenant.org
[version]: http://contributor-covenant.org/version/1/4/
