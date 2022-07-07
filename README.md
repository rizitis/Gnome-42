# Gnome Slackware current

 
`SLACKWARE don``t provide Gnome and officially not support any issues...
This repository created for personal use and I DON``T SUGGEST YOU to install these packages to a working Slackware system. 
If you do it, you do it at your own risk!!! So better think again about it before you make  installation!!!` 

### In case that you think you can hack your system, read this:

  THIS SOFTWARE IS PROVIDED BY THE AUTHOR "AS IS" AND ANY EXPRESS OR IMPLIED
  WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED.  IN NO
  EVENT SHALL THE AUTHOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


## Getting started

# STEP 0

```
slackpkg update
slackpkg upgrade-all
```

# STEP 1

```
groupadd -g 214 avahi
 useradd -u 214 -g 214 -c "Avahi User" -d /dev/null -s /bin/false avahi

 groupadd -g 303 colord
 useradd -d /var/lib/colord -u 303 -g colord -s /bin/false colord
```

# STEP 2

open a terminal to edit /etc/rc.d/rc.4
```
nano /etc/rc.d/rc.4
```
and remove the -nodaemon from gdm
it should be like this 
```
if [ -x /usr/sbin/gdm ]; then
  exec /usr/sbin/gdm
fi
```

# STEP 3

edit /etc/rc.d/rc.local and add these lines

```
# Start avahidaemon
if [ -x /etc/rc.d/rc.avahidaemon ]; then
 /etc/rc.d/rc.avahidaemon start
fi
# Start avahidnsconfd
if [ -x /etc/rc.d/rc.avahidnsconfd ]; then
  /etc/rc.d/rc.avahidnsconfd start
fi
```
# STEP 4 

edit /etc/rc.d/rc.local_shutdown (if you dont have  rc.local_shutdown on your system then create one)

Add these lines in this file if exist:

```
# Stop avahidnsconfd
if [ -x /etc/rc.d/rc.avahidnsconfd ]; then
  /etc/rc.d/rc.avahidnsconfd stop
fi
# Stop avahidaemon
if [ -x /etc/rc.d/rc.avahidaemon ]; then
  /etc/rc.d/rc.avahidaemon stop
fi
```

if not exist then create it and add these lines to the empty file:

```
#!/bin/bash
#


# Stop avahidnsconfd
if [ -x /etc/rc.d/rc.avahidnsconfd ]; then
  /etc/rc.d/rc.avahidnsconfd stop
fi
# Stop avahidaemon
if [ -x /etc/rc.d/rc.avahidaemon ]; then
  /etc/rc.d/rc.avahidaemon stop
fi

```
then save it and command 

```
chmod +x /etc/rc.d/rc.local_shutdown
```

# STEP 5

Blacklist packages

```
nano /etc/slackpkg/blacklist
```
at the end of the file add this
```
[0-9]+_rtz
```

# STEP 6
```
git clone git@gitlab.gnome.org:rizitis/gnome-slackware-current.git
cd gnome-slackware-current
upgradepkg --install-new --reinstall *.t?z 
```

# STEP 7 

edit inittab to runlevel 4
```
nano /etc/inittab

```
Default runlevel. (Do not set to 0 or 6)
id:4:initdefault:

# STEP 8 

```
reboot

```

## Notes and tips
If everything goes ok , you should have gdm on your boot screen, maybe with a delay some times if you have potato pc like me...

Now you should enable pipewire using Pat`s  Slackbuild script that you have on your system.
```
pipewire-enable.sh
reboot

```


## IF everything didn`t goes ok!!!! and you are on "Gnome white screen of death" then the safe and best choice I THINK is to hit

```
Ctrl+Alt+f2 

```
login as root and cd to the gnome-slackware-current folder which include all these binaries.
Then command 

```
removepkg *.* 

```

after that edit again 

```
nano /etc/slackpkg/blacklist
```

and delete that 

```
[0-9]+_rtz
```
now command 

```
updatedb 
slackpkg update
slackpkg upgrade-all
```
after that you can reboot , probably your system is back...
If not I m sorry but I wrote on the top "DO NOT INSTALL THEM".







