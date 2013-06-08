---
layout: post
title: "Fighting with SELinux and Nagios"
date: 2009-05-02 09:25
comments: true
categories:
- howto
- linux
- nagios
- sys-admin
- tech
- tutorial
---
I can't believe it, but I won! I have been trying to set up Nagios on a RHEL5 machine running SELinux and have been loosing the fight for the last 3 days. But today, I win! This is such a win, it is worth sharing.

Now that I have won though, I believe this is not Nagios specific at all, and if I had bothered to learn about SELinux, this may have been obvious. Anyway, the error Nagios was giving me was:

{% blockquote %}
Error: Could not stat() command file '/usr/local/nagios/var/rw/nagios.cmd'!
The external command file may be missing, Nagios may not be running, and/or Nagios may not be checking external commands.
An error occurred while attempting to commit your command for processing.
Return from whence you came
{% endblockquote %}

As you may have already guess, the solution has nothing to do with the location or permissions of the file, the file was not missing, Nagios was running, and Nagios was checking external commands. The final line of the message is great though, and I can only hope we start to see more old English in error messages.

The problem of course, was that SELinux was enabled and stopping this blatant security violation. You can check to see if SELinux is on by running:

```
$ /usr/sbin/getenforce
Enforcing
```

If you got "Permissive" or "Disabled", then this post is not for you. To see SELinux's side of things, check out `/var/log/messages`:

```
setroubleshoot: SELinux is preventing ping (ping_t) "read write" to /usr/local/nagios/var/spool/checkresults/checkrXH96b (usr_t). For complete SELinux messages. run sealert -l 1ffc2533-42b5-4e04-b7ab-a81bb7d02040

setroubleshoot: SELinux is preventing ping (ping_t) "read write" to /usr/local/nagios/var/spool/checkresults/checkrZxsA1 (usr_t). For complete SELinux messages. run sealert -l 178ba2d4-0822-47eb-9e32-bfaa19ee3c4b

setroubleshoot: SELinux is preventing cmd.cgi (httpd_sys_script_t) "getattr" to /usr/local/nagios/var/rw/nagios.cmd (httpd_sys_content_t). For complete SELinux messages. run sealert -l 4df0946e-8816-4b90-a7d1-37e743697b9c
```

As you can see, SELinux is trying to give you a hint with that sealert bit, so you should take it.

```
$ sealert -l 1ffc2533-42b5-4e04-b7ab-a81bb7d02040
Summary:
SELinux is preventing ping (ping_t) "read write" to
/usr/local/nagios/var/spool/checkresults/checkrXH96b (usr_t).

Detailed Description:

... (removed from post)

Raw Audit Messages

host=myhost.myisp.host type=AVC msg=audit(1241217029.141:125305): avc: denied { read write } for pid=32379 comm="ping" path="/usr/local/nagios/var/spool/checkresults/checkrXH96b" dev=sda3 ino=52894945 scontext=user_u:system_r:ping_t:s0 tcontext=user_u:object_r:usr_t:s0 tclass=file

host=myhost.myisp.host type=SYSCALL msg=audit(1241217029.141:125305): arch=c000003e syscall=59 success=yes exit=0 a0=153952a0 a1=15395330 a2=7fff75c5eb40 a3=0 items=0 ppid=32378 pid=32379 auid=503 uid=508 gid=508 euid=0 suid=0 fsuid=0 egid=508 sgid=508 fsgid=508 tty=(none) ses=1392 comm="ping" exe="/bin/ping" subj=user_u:system_r:ping_t:s0 key=(null)
```

That raw audit message is **GOLD**! There is some other information in there, but nothing about what the next step should be to create a policy and make it permanent. Using `chron` I have heard is a temporary fix. The solution is copying that raw audit message into an empty file and running `audit2allow` to create a policy:

```
$ cat > /tmp/tmp-nagiosping
host=myhost.myisp.host type=AVC msg=audit(1241217029.141:125305): avc: denied { read write } for pid=32379 comm="ping" path="/usr/local/nagios/var/spool/checkresults/checkrXH96b" dev=sda3 ino=52894945 scontext=user_u:system_r:ping_t:s0 tcontext=user_u:object_r:usr_t:s0 tclass=file
host=myhost.myisp.host type=SYSCALL msg=audit(1241217029.141:125305): arch=c000003e syscall=59 success=yes exit=0 a0=153952a0 a1=15395330 a2=7fff75c5eb40 a3=0 items=0 ppid=32378 pid=32379 auid=503 uid=508 gid=508 euid=0 suid=0 fsuid=0 egid=508 sgid=508 fsgid=508 tty=(none) ses=1392 comm="ping" exe="/bin/ping" subj=user_u:system_r:ping_t:s0 key=(null)
* Ctrl-D *

$ audit2allow -M NagiosPing < /tmp/tmp-nagiosping

******************** IMPORTANT ***********************
To make this policy package active, execute:

semodule -i NagiosPing.pp
```

This creates a file call NagiosPing.pp which contains the SELinux policy needed to make these errors go away. The only thing left to do is to install this policy:

```
$ semodule -i NagiosPing.pp
```

If your setup was like mine, SELinux was actually preventing 3 different actions, needing 3 different policies. HA! That is easy now - just repeat the steps until Nagios is doing your bidding.
