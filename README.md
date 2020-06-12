# google-oauth2-sh

- [Description](#description)
- [Requirement](#requirement)
- [Installation](#installation)
- [Getting Started Guide](#getting-started-guide)
- [Usage](#usage)
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

## Getting Started Guide

### 1. OAuth2.0 client register by Google Console
Please setting your application.

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

The format of that file is shell script code.
For example, the following file.

```
# OAuth2.0 Client setting
SCOPE=https://mail.google.com/
CLIENT_ID=XXXXXX.apps.googleusercontent.com
CLIENT_SECRET=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Replace with the value for your application.


### 3. Generate Authorization-URI
Use the "-a" flag to generate Authroization-URI.

```
$ ./ggl-oauth2 -a my-account-file
https://accounts.google.com/o/oauth2/v2/auth?client_id=XXXXXX.apps.googleusercontent.com&scope=https://mail.google.com/&response_type=code&access_type=offline&redirect_uri=urn:ietf:wg:oauth:2.0:oob
please input authorization-code:
```

### 4. Authorization procedure
Access to Authorization-URI(got by step.3) by web browser.
And perform authorization procedure
if success, get Authorization-code.

### 5. Get access-token & refresh-token

Continue step3(`ggl-oauth2 -a` command), please input Authorization-code(got by step4)
Script gets access token and refresh token use input Authorization-code.

```
please input authorization-code: YYYYYYYYYYYYYYYYYY
AAAAAAAAAAAAAAAAA
```

If success, print value of got access-token and write refresh-token to Account-file.

See your Account-file, appended variable `REFRESH_TOKEN`.

```
REFRESH_TOKEN=BBBBBBBBBBBBBBBBB
```

### 6. Refresh access-token
Use the "-r" flag to refresh the access-token.

```
$ ./ggl-oauth2 -r my-account-file
ZZZZZZZZZZZZZZZZZ
```

If success, print value of new access-token.

## Usage

```
[USAGE]
  ggl-oauth2 [-hV]
  ggl-oauth2 [-a][-b browser-command] <Account-file>
  ggl-oauth2 [-r] <Account-file>

[OPTIONS]
  -a            Generate authorization URI & get access_token & refresh_token.
                Write the received refresh_token to <Account_file>, and print access_token.

  -b browser    Set Authroization-URI to browser argument.

  -h            Print this script help.

  -r            Refresh access_token, and print new access_token.

  -v            Verbose mode. print curl log, and print refresh_token.

  -V            Print script version.

[Account-file]
 Google account config file.
 This file is read by dot command. Therefor, the write rule is shell script format.
 Bellow variables are required.
  - SCOPE         : http-param name is 'scope'
  - CLIENT_ID     : http-param name is 'client_id'
  - CLIENT_SECRET : http-param name is 'client_secret'
  - REFRESH_TOKEN : http-param name is 'refresh_token' (auto set value by script)

[EXIT-STATUS]
  0             no error
  1             parameter error
  2             process error
  3             user cancel
```

## Licence

[MIT](https://github.com/hacolab/google-oauth2-sh/LICENCE)


## Author

[hacolab](https://github.com/hacolab)

