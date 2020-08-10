# google-oauth2-sh

- [Description](#description)
- [Requirement](#requirement)
- [Installation](#installation)
- [Usage](#usage)
- [Getting Started Guide](#getting-started-guide)
- [Licence](#licence)
- [Author](#author)

## Description
This script is authorization(OAuth2.0) google.

## Requirement

- curl [https://github.com/curl/curl]

## Installation

```
$ git clone https://github.com/hacolab/google-oauth2-sh
$ chmod +x google-oauth2-sh/ggl-oauth2
```

## Usage

```
Authorization(OAuth2.0) for Google API.
Functions of this script are below.
  * Generate Authorization-URI
  * Get new access_token & refresh_token
  * Get infomation of access_token
  * Update access_token
  * Revoke access_token & refresh_token
You must prepare the <Account-file>.

[USAGE]
  ggl-oauth2 -g [-b browser] [-nv] <Account-file>
  ggl-oauth2 -i <token>
  ggl-oauth2 -n [-v] <Account-file>
  ggl-oauth2 -r <token>
  ggl-oauth2 -u [-v] <Account-file>
  ggl-oauth2 [-hV]

[OPTIONS]
  -b <browser>  Set Authorization-URI to browser argument. use with '-g'.

  -g            Generate Authorization-URI.

  -i <token>    Inquire infomation of access_token.

  -n            Get new access_token & refresh_token.
                If success, Write the received refresh_token to <Account-file>,
                and print value of access_token.

  -r <token>    Revoke access_token & refresh_token.

  -u            Update access_token, and print value of new access_token.

  -v            Verbose mode. print curl log and refresh_token.

  -h            Print this script help.

  -V            Print script version.

[Account-file]
 Google account config file.
 This file is load by dot command. Therefor, the write rule is shell script format.
 Below variables are required.
  - SCOPE         : http-param name is 'scope'
  - CLIENT_ID     : http-param name is 'client_id'
  - CLIENT_SECRET : http-param name is 'client_secret'
  - REFRESH_TOKEN : http-param name is 'refresh_token' (auto set value by script)

[EXIT-STATUS]
  0             no error
  1             parameter error
  2             process error
  9             user cancel

[DEPENDENCY]
  - curl

[REFERENCE]
  - https://developers.google.com/identity/protocols/oauth2
```

## Getting Started Guide

### 1. OAuth2.0 client register by Google Console
Please setting your application by google console.

[https://console.developers.google.com/apis/credentials]

See below for the procedure.

[https://hacolab.hatenablog.com/entry/2020/05/28/190000]

### 2. Create Google account config file.
Please create simple text file.

```
$ cat << EOF > my-account-file
SCOPE=
CLIENT_ID=
CLIENT_SECRET=
EOF
$ chmod 600 my-account-file
```

The format of my-account-file is shell script code.
For example, the following file.

```
# OAuth2.0 Client setting
SCOPE=https://mail.google.com/
CLIENT_ID=XXXXXX.apps.googleusercontent.com
CLIENT_SECRET=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Replace values for your application.

### 3. Generate Authorization-URI
Use the "-g" flag to generate Authroization-URI.

```
$ ./ggl-oauth2 -g my-account-file
https://accounts.google.com/o/oauth2/v2/auth?client_id=XXXXXX.apps.googleusercontent.com&scope=https://mail.google.com/&response_type=code&access_type=offline&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

### 4. Authorization procedure
Access to Authorization-URI(got by step.3) by web browser.
And perform authorization procedure
if success, get Authorization-code.

### 5. Get new access-token & refresh-token
Use the "-n" flag to get new access-token & refresh-token.

command), please input Authorization-code(got by step4)
Script gets access token and refresh token use input Authorization-code.

```
$ ./ggl-oauth2 -n my-account-file
please input authorization-code: YYYYYYYYYYYYYYYYYY
AAAAAAAAAAAAAAAAA
```

If success, print value of got access-token and write refresh-token to Account-file.

See your Account-file, appended variable `REFRESH_TOKEN`.

```
REFRESH_TOKEN=BBBBBBBBBBBBBBBBB
```

### 6. Update access-token
Use the "-u" flag to refresh the access-token.
And print new access-token.

```
$ ./ggl-oauth2 -u my-account-file
ZZZZZZZZZZZZZZZZZ
```

If success, print value of new access-token.

## Licence

[MIT](https://github.com/hacolab/google-oauth2-sh/LICENCE)


## Author

[hacolab](https://github.com/hacolab)

