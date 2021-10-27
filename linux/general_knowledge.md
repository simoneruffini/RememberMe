# General Knowledge

## System Logs

You can write a kernel message by either:
`echo "message" > /dev/kmsg`
Or by using `logger` command.

## `tree` without `tree`
Taken from [here](https://access.redhat.com/solutions/53656).
```sh
ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/   /' -e 's/-/|/'
```
Usefull when in certain busybox builds that lack `tree`.
