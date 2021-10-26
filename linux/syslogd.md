## Syslogd

`syslogd` is the most common logger for Linux and Unix. The syslogd daemon handles messages from servers and programs.

syslogd provides a unified means of handling log files. It accepts log messsages delivered from servers and programs and directs them to the appropriate log files. This enables the consolidation of messages from various sources in standard log files, which makes them easier to manage.

The syslogd configuration is done using the `/etc/syslog.conf` file. The logging is specified with rules entries. On each line the **selector (facility.priority)** and the **action** are specified. For example, consider the following line:
```sh
kern.alert  /var/log/kern.log
```
The rule above specifies that each log message from the **kern** facility with the priority of **alert** and higher will be directed to the file `/var/log/kern.log`.

To direct messages to remote log host, use the `@` character to specify the hostname of the log host. For example, if we want to direct messages from the example above to the remote server **suse1**, we would use the following line:
```sh
kern.alert  @suse1
```
Here is another example, a fairly ordinary and simple entry:
```sh
mail.*  /var/log/mail
```
This line sends all log entries identified by the originating program as related to **mail** to the `/var/log/mail` file. Most of the entries in a default `/etc/syslog.conf` file resemble this one. Together, they typically cover all of the facilities mentioned earlier. Some messages may be handled by multiple rules. For instance, another rule might look like this one:
```sh
*.emerg  *
```
This line sends all **emerg**-level messages to the consoles of all users who are logged into the computer using text-mode tools. If this line and the earlier `mail.*` selector are both present, emerg-level messages related to mail will be logged to `/var/log/mail` and displayed on users’ consoles.

 

**On most modern Linux distributions syslogd has been replaced with some newer syslog implementations, such as rsyslog or syslog-ng.**

---

Credit: This file is a rework of [geekuniversity](https://geek-university.com/linux/syslogd/)

---
