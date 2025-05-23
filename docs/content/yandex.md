---
title: "Yandex"
description: "Yandex Disk"
versionIntroduced: "v1.26"
---

# {{< icon "fa fa-space-shuttle" >}} Yandex Disk

[Yandex Disk](https://disk.yandex.com) is a cloud storage solution created by [Yandex](https://yandex.com).

## Configuration

Here is an example of making a yandex configuration.  First run

    rclone config

This will guide you through an interactive setup process:

```
No remotes found, make a new one?
n) New remote
s) Set configuration password
n/s> n
name> remote
Type of storage to configure.
Choose a number from below, or type in your own value
[snip]
XX / Yandex Disk
   \ "yandex"
[snip]
Storage> yandex
Yandex Client Id - leave blank normally.
client_id>
Yandex Client Secret - leave blank normally.
client_secret>
Remote config
Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.
y) Yes
n) No
y/n> y
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth
Log in and authorize rclone for access
Waiting for code...
Got code
Configuration complete.
Options:
- type: yandex
- client_id:
- client_secret:
- token: {"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx","token_type":"OAuth","expiry":"2016-12-29T12:27:11.362788025Z"}
Keep this "remote" remote?
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
```

See the [remote setup docs](/remote_setup/) for how to set it up on a
machine with no Internet browser available.

Note that rclone runs a webserver on your local machine to collect the
token as returned from Yandex Disk. This only runs from the moment it
opens your browser to the moment you get back the verification code.
This is on `http://127.0.0.1:53682/` and this it may require you to
unblock it temporarily if you are running a host firewall.

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

Yandex paths may be as deep as required, e.g. `remote:directory/subdirectory`.

### Modification times and hashes

Modified times are supported and are stored accurate to 1 ns in custom
metadata called `rclone_modified` in RFC3339 with nanoseconds format.

The MD5 hash algorithm is natively supported by Yandex Disk.

### Emptying Trash

If you wish to empty your trash you can use the `rclone cleanup remote:`
command which will permanently delete all your trashed files. This command
does not take any path arguments.

### Quota information

To view your current quota you can use the `rclone about remote:`
command which will display your usage limit (quota) and the current usage.

### Restricted filename characters

The [default restricted characters set](/overview/#restricted-characters)
are replaced.

Invalid UTF-8 bytes will also be [replaced](/overview/#invalid-utf8),
as they can't be used in JSON strings.

{{< rem autogenerated options start" - DO NOT EDIT - instead edit fs.RegInfo in backend/yandex/yandex.go then run make backenddocs" >}}
### Standard options

Here are the Standard options specific to yandex (Yandex Disk).

#### --yandex-client-id

OAuth Client Id.

Leave blank normally.

Properties:

- Config:      client_id
- Env Var:     RCLONE_YANDEX_CLIENT_ID
- Type:        string
- Required:    false

#### --yandex-client-secret

OAuth Client Secret.

Leave blank normally.

Properties:

- Config:      client_secret
- Env Var:     RCLONE_YANDEX_CLIENT_SECRET
- Type:        string
- Required:    false

### Advanced options

Here are the Advanced options specific to yandex (Yandex Disk).

#### --yandex-token

OAuth Access Token as a JSON blob.

Properties:

- Config:      token
- Env Var:     RCLONE_YANDEX_TOKEN
- Type:        string
- Required:    false

#### --yandex-auth-url

Auth server URL.

Leave blank to use the provider defaults.

Properties:

- Config:      auth_url
- Env Var:     RCLONE_YANDEX_AUTH_URL
- Type:        string
- Required:    false

#### --yandex-token-url

Token server url.

Leave blank to use the provider defaults.

Properties:

- Config:      token_url
- Env Var:     RCLONE_YANDEX_TOKEN_URL
- Type:        string
- Required:    false

#### --yandex-client-credentials

Use client credentials OAuth flow.

This will use the OAUTH2 client Credentials Flow as described in RFC 6749.

Properties:

- Config:      client_credentials
- Env Var:     RCLONE_YANDEX_CLIENT_CREDENTIALS
- Type:        bool
- Default:     false

#### --yandex-hard-delete

Delete files permanently rather than putting them into the trash.

Properties:

- Config:      hard_delete
- Env Var:     RCLONE_YANDEX_HARD_DELETE
- Type:        bool
- Default:     false

#### --yandex-encoding

The encoding for the backend.

See the [encoding section in the overview](/overview/#encoding) for more info.

Properties:

- Config:      encoding
- Env Var:     RCLONE_YANDEX_ENCODING
- Type:        Encoding
- Default:     Slash,Del,Ctl,InvalidUtf8,Dot

#### --yandex-spoof-ua

Set the user agent to match an official version of the yandex disk client. May help with upload performance.

Properties:

- Config:      spoof_ua
- Env Var:     RCLONE_YANDEX_SPOOF_UA
- Type:        bool
- Default:     true

#### --yandex-description

Description of the remote.

Properties:

- Config:      description
- Env Var:     RCLONE_YANDEX_DESCRIPTION
- Type:        string
- Required:    false

{{< rem autogenerated options stop >}}

## Limitations

When uploading very large files (bigger than about 5 GiB) you will need
to increase the `--timeout` parameter.  This is because Yandex pauses
(perhaps to calculate the MD5SUM for the entire file) before returning
confirmation that the file has been uploaded.  The default handling of
timeouts in rclone is to assume a 5 minute pause is an error and close
the connection - you'll see `net/http: timeout awaiting response
headers` errors in the logs if this is happening.  Setting the timeout
to twice the max size of file in GiB should be enough, so if you want
to upload a 30 GiB file set a timeout of `2 * 30 = 60m`, that is
`--timeout 60m`.

Having a Yandex Mail account is mandatory to use the Yandex.Disk subscription.
Token generation will work without a mail account, but Rclone won't be able to complete any actions.
```
[403 - DiskUnsupportedUserAccountTypeError] User account type is not supported.
```
