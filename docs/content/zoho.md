---
title: "Zoho"
description: "Zoho WorkDrive"
versionIntroduced: "v1.54"
---

# {{< icon "fas fa-folder" >}} Zoho Workdrive

[Zoho WorkDrive](https://www.zoho.com/workdrive/) is a cloud storage solution created by [Zoho](https://zoho.com).

## Configuration

Here is an example of making a zoho configuration.  First run

    rclone config

This will guide you through an interactive setup process:

```
No remotes found, make a new one?
n) New remote
s) Set configuration password
n/s> n
name> remote
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
[snip]
XX / Zoho
   \ "zoho"
[snip]
Storage> zoho
** See help for zoho backend at: https://rclone.org/zoho/ **

OAuth Client Id
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_id> 
OAuth Client Secret
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_secret> 
Edit advanced config? (y/n)
y) Yes
n) No (default)
y/n> n
Remote config
Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.
y) Yes (default)
n) No
y/n> 
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=LVn0IHzxej1ZkmQw31d0wQ
Log in and authorize rclone for access
Waiting for code...
Got code
Choose a number from below, or type in your own value
 1 / MyTeam
   \ "4u28602177065ff22426787a6745dba8954eb"
Enter a Team ID> 1
Choose a number from below, or type in your own value
 1 / General
   \ "4u2869d2aa6fca04f4f2f896b6539243b85b1"
Enter a Workspace ID> 1
Configuration complete.
Options:
- type: zoho
- token: {"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx","token_type":"Zoho-oauthtoken","refresh_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx","expiry":"2020-10-12T00:54:52.370275223+02:00"}
- root_folder_id: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Keep this "remote" remote?
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> 
```

See the [remote setup docs](/remote_setup/) for how to set it up on a
machine with no Internet browser available.

Rclone runs a webserver on your local computer to collect the
authorization token from Zoho Workdrive. This is only from the moment
your browser is opened until the token is returned.
The webserver runs on `http://127.0.0.1:53682/`.
If local port `53682` is protected by a firewall you may need to temporarily
unblock the firewall to complete authorization.

Once configured you can then use `rclone` like this, (replace `remote` with the name you gave your remote)

See top level directories

    rclone lsd remote:

Make a new directory

    rclone mkdir remote:directory

List the contents of a directory

    rclone ls remote:directory

Sync `/home/local/directory` to the remote path, deleting any
excess files in the path.

    rclone sync --interactive /home/local/directory remote:directory

Zoho paths may be as deep as required, eg `remote:directory/subdirectory`.

### Modification times and hashes

Modified times are currently not supported for Zoho Workdrive

No hash algorithms are supported.

### Usage information

To view your current quota you can use the `rclone about remote:`
command which will display your current usage.

### Restricted filename characters

Only control characters and invalid UTF-8 are replaced. In addition most
Unicode full-width characters are not supported at all and will be removed 
from filenames during upload.

{{< rem autogenerated options start" - DO NOT EDIT - instead edit fs.RegInfo in backend/zoho/zoho.go then run make backenddocs" >}}
### Standard options

Here are the Standard options specific to zoho (Zoho).

#### --zoho-client-id

OAuth Client Id.

Leave blank normally.

Properties:

- Config:      client_id
- Env Var:     RCLONE_ZOHO_CLIENT_ID
- Type:        string
- Required:    false

#### --zoho-client-secret

OAuth Client Secret.

Leave blank normally.

Properties:

- Config:      client_secret
- Env Var:     RCLONE_ZOHO_CLIENT_SECRET
- Type:        string
- Required:    false

#### --zoho-region

Zoho region to connect to.

You'll have to use the region your organization is registered in. If
not sure use the same top level domain as you connect to in your
browser.

Properties:

- Config:      region
- Env Var:     RCLONE_ZOHO_REGION
- Type:        string
- Required:    false
- Examples:
    - "com"
        - United states / Global
    - "eu"
        - Europe
    - "in"
        - India
    - "jp"
        - Japan
    - "com.cn"
        - China
    - "com.au"
        - Australia

### Advanced options

Here are the Advanced options specific to zoho (Zoho).

#### --zoho-token

OAuth Access Token as a JSON blob.

Properties:

- Config:      token
- Env Var:     RCLONE_ZOHO_TOKEN
- Type:        string
- Required:    false

#### --zoho-auth-url

Auth server URL.

Leave blank to use the provider defaults.

Properties:

- Config:      auth_url
- Env Var:     RCLONE_ZOHO_AUTH_URL
- Type:        string
- Required:    false

#### --zoho-token-url

Token server url.

Leave blank to use the provider defaults.

Properties:

- Config:      token_url
- Env Var:     RCLONE_ZOHO_TOKEN_URL
- Type:        string
- Required:    false

#### --zoho-client-credentials

Use client credentials OAuth flow.

This will use the OAUTH2 client Credentials Flow as described in RFC 6749.

Properties:

- Config:      client_credentials
- Env Var:     RCLONE_ZOHO_CLIENT_CREDENTIALS
- Type:        bool
- Default:     false

#### --zoho-upload-cutoff

Cutoff for switching to large file upload api (>= 10 MiB).

Properties:

- Config:      upload_cutoff
- Env Var:     RCLONE_ZOHO_UPLOAD_CUTOFF
- Type:        SizeSuffix
- Default:     10Mi

#### --zoho-encoding

The encoding for the backend.

See the [encoding section in the overview](/overview/#encoding) for more info.

Properties:

- Config:      encoding
- Env Var:     RCLONE_ZOHO_ENCODING
- Type:        Encoding
- Default:     Del,Ctl,InvalidUtf8

#### --zoho-description

Description of the remote.

Properties:

- Config:      description
- Env Var:     RCLONE_ZOHO_DESCRIPTION
- Type:        string
- Required:    false

{{< rem autogenerated options stop >}}

## Setting up your own client_id

For Zoho we advise you to set up your own client_id. To do so you have to complete the following steps.

1. Log in to the [Zoho API Console](https://api-console.zoho.com)

2. Create a new client of type "Server-based Application". The name and website don't matter, but you must add the redirect URL `http://localhost:53682/`.

3. Once the client is created, you can go to the settings tab and enable it in other regions.

The client id and client secret can now be used with rclone.
