# sysdweb
Control systemd services through Web or REST API

## Installation

```sh
git clone https://github.com/ogarcia/sysdweb.git
virtualenv3 ./sysdweb-venv
source ./sysdweb-venv/bin/activate
cd sysdweb
python setup.py install
```

Arch Linux users can install sysdweb from [AUR][1].

## Run

First take a look to `sysdweb.conf` file to configure sysdweb. Is self
explanatory.

You can place `sysdweb.conf` in `/etc` for system, in user home
`~/.config/sysdweb/sysdweb.conf` or in same directory where you run sysdweb.

Once you have configured sysdweb, simply run.

```
sysdweb
```

By default sysdweb listen in 10080 port to 127.0.0.1, you can change listen
port and address with `-p` and `-l`.

```sh
sysdweb -p 9080 -l 0.0.0.0
```

## API

You can control configured services via REST API, for example, with curl.

The API endpoint is `/api/v1/<service>/<action>`, always `GET` and response
a json with following format.

```json
{
  "<action>": "<result>"
}
```

The `<service>` tag is defined in config file and match with section label.
For example, in following config, the service would be `ngx`.

```ini
[ngx]
title = Nginx
unit = nginx.service
```

The posible `<actions>` are.

* start
* stop
* restart
* reload
* reloadorrestart
* status

All actions (except `status`) return as result `OK` if can communicate with
DBUS or `Fail` if any error occurs.

For `status` action, the posible responses are.

* active (started unit)
* reloading
* inactive (stopped unit)
* failed (stopped unit)
* activating
* deactivating
* not-found (for inexistent unit)

In the example defined above all valid enpoins are.

```
http://127.0.0.1:10080/api/v1/ngx/start
http://127.0.0.1:10080/api/v1/ngx/stop
http://127.0.0.1:10080/api/v1/ngx/restart
http://127.0.0.1:10080/api/v1/ngx/reload
http://127.0.0.1:10080/api/v1/ngx/reloadorrestart
http://127.0.0.1:10080/api/v1/ngx/status
```

[1]: https://aur.archlinux.org/packages/sysdweb/
