## Graphical Software within a Docker container

This project is a demo for running any program with a GUI from within a Docker container.

Docker normally doesn't handle the "X server", as for most "headless servers", and therefore fails at opening many GUI programs and crashes.

This demo is essentially based on this [post (2013-07-08)](https://blog.docker.com/2013/07/docker-desktop-your-desktop-over-ssh-running-inside-of-a-docker-container/) on Docker by [Robert Hashioka](<roberto.hashioka@dotcloud.com>):


which led me to those links:
- [a github repository](https://github.com/rogaha/docker-desktop#usage) (2014-09-26)
- [a wiki post by Rich on Fluxbox, X11vnc and Xvbf](http://www.richud.com/wiki/Ubuntu_Fluxbox_GUI_with_x11vnc_and_Xvfb) (2014-12-24)
- [biggest docker meetup ever: Clocker, Storage Drivers, Docker on the Desktop, ...](https://blog.docker.com/tag/desktop/) (2015-03-31)

## What it does
This Docker image enables to run GUI software without crashing, and more importantly enables to display the GUI as you could do between a remote host and your local machine  (thinking of a Docker container similar to an headless server).

More precisely, these tools (Xpra, Xephyr, Fluxbox, Rox-filer) can let you remotely access your machine throught a Fluxbox GUI, via VNC (5900) and Xvfb independently from your 'normal' desktop GUI.
+ Xpra is a tool that allows to run X clients (e.g. graphical windows) usually on a remote host, and to direct their display to your local machine without "loosing any state" (i.e. it allows disconnection and reconnection without disrupting the forwarded application. It appears as a normal window on the localc desktop as opposed to VNC for example, and is managed by your local window manager.)
+ Xephyr is a display server implementing the X11 display server which targets a window (X client) on a host X server as its framebuffer.
(How is it talking to/working with Xpra????)
Xpra+Xephyr display the application (X client) running inside the container (i.e. seen as the host machine) with "recovery session capabilities" (i.e. ??? not crashing if disconnected, simply reconnecting from where it left).
+ Xvfb (X virtual framebuffer) is an in-memory display server for UNIX-like operating system. It allows to run graphical applications without a display while having the ability to take screenshots.
+ Fluxbox is a window manager for X. Therefore providing window manager capabilities (tabbing/grouping/keycommands/chainable keygrabber and editable menus).
+ Rox-filer is a graphical file manager for the X window system. It can be used as a small and fast filer with your (remote/virtual) desktop.

A drawing similar to [this one](http://upload.wikimedia.org/wikipedia/commons/thumb/0/03/X_client_server_example.svg/606px-X_client_server_example.svg.png) should soon summarized the interactions between those tools.
![X architecture](http://upload.wikimedia.org/wikipedia/commons/thumb/0/03/X_client_server_example.svg/606px-X_client_server_example.svg.png)

## Other interesting links
- https://linuxmeerkat.wordpress.com/2014/10/17/running-a-gui-application-in-a-docker-container/,




## Getting started
### Docker installation
easy: https://docs.docker.com/installation/
### 

You can try the script outside Docker and check what it does:
```shell
$ python firefox.py
```

You should see Firefox opening the webpage, and when finished, printing HTML to your terminal.

Now running within Docker, you can see that:

```shell
$ sudo docker run -ti patrickmerlot/xvfb_demo
```

- the page is loaded,
- the HTML source printed,
- but the Firefox GUI doesn't appear! At least the script can proceed without crashing.

## Commands

To rebuild the Docker image:
```shell
$ sudo docker build --rm -t patrickmerlot/xvfb_demo .
```

Note: To run docker in detached mode (read [this](https://docs.docker.com/reference/commandline/cli/#option-types))

To run the app within Docker:
```shell
$ sudo docker run -ti patrickmerlot/xvfb_demo
```