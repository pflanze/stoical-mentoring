# Mentoring sessions for fixing STOICAL for today's machines

As per the [sub-thread](https://news.ycombinator.com/item?id=30958590)
at [Old C code – how to upgrade
it?](https://news.ycombinator.com/item?id=30957273).

Maybe more "coworking" than "mentoring", since I'm not going to be the
perfect mentor, and attendees will probably know things that I don't.

I'll post here if sessions will be happening. Once it's decided a
session will happen, I'm planning to put up a poll to find out which
out of a choice of date/time alternatives will be best.

Send a mail with subject "STOICAL mentoring" to Christian Jaeger
<ch@christianjaeger.ch> if you'd like to be notified when there are
news. If you've got a Github account you could instead enable
notifications on this repo.

I'm currently planning to do the sessions via Jitsi meet, and
(optionally) allow anyone interested to access the VNC session on the
shared server directly. I'll also share my desktop in Jitsi for those
who don't want to or can't use VNC (VNC does offer better frame rates
and perfectly sharp pictures, though).

As per current planning, there will be no recording. I'm still
pondering this point, though.

## VNC client setup

Again, you can ignore this section if you're happy with the quality of
the screen share that I'll offer via Jitsi and don't want to interact
actively with the session (other than talking or sending chat messages).

Since standard VNC doesn't offer encryption, it is usually used via
some kind of tunneling, I'm normally using SSH tunneling. Thus
you'll need to give me (one of) your SSH public key(s), which I'll
install on the server. Additionally, VNC itself uses a password, which
will be the value `12345678` for these sessions (I don't think any
outside actor will be able to get a connection to the VNC server
running on localhost).

*Note: stoical-mentoring.christianjaeger.ch is not active yet. I'll
let you know when it is, so you could test in advance if you'd like.*

### Linux and alikes

1. Install `tigervnc-viewer` and create a VNC passwd file:

        sudo apt install tigervnc-viewer
        ( umask 077; mkdir ~/.vncclient-passwords/ )
        vncpasswd .vncclient-passwords/stoical-mentoring
        
    Enter `12345678` for the password. Say `n` to the view-only
    question if you'd like to be able to click and type into the
    shared session.

1. Create a script `stoical-mentoring-tunnel`:

        #!/bin/bash

        ssh -N -o compression=no -L5901:localhost:5901 vnc@stoical-mentoring.christianjaeger.ch

1. Create a script `stoical-mentoring-vnc`:

        #!/bin/bash

        xvncviewer -FullScreen -RemoteResize=0 -MenuKey F7 -shared -passwd ~/.vncclient-passwords/stoical-mentoring localhost:1

    Note: xvncviewer in fullscreen mode
    [can/does](https://github.com/TigerVNC/tigervnc/issues/1150)
    interact weirdly with (some?) screensavers. When xscreensaver
    locks the screen, xvncviewer apparently keeps its own window atop
    the screensaver's, thus the locking can't be noticed; the keyboard
    appears dead at that point. But simply clicking into the window
    shown by the VNC client may revive the keyboard focus (if it
    doesn't, you may have to switch to another Linux console via
    e.g. Ctl-Alt-F1, log in, kill the screen saver, and switch back
    with Ctl-Alt-F7 or whichever is the console with your X session).
    
    You could disable your screen saver by enabling presentation mode
    on your desktop if available, or using something like
    [run-presentation](https://github.com/pflanze/chj-bin/blob/master/run-presentation).

    Or you could omit the -FullScreen option, but then your local
    window manager will capture some key strokes that you might want
    to direct to the server.
    
    The MenuKey, F7, will allow you to open a menu locally that will
    allow you to disable and enable full-screen mode or exit the VNC
    viewer.

Then, to access the session, you need to first run
`stoical-mentoring-tunnel` in a terminal and keep it open, and then
run `stoical-mentoring-vnc`.

### Other systems

I'm not using other systems and don't know the best ways there. I'll
be happy to include details here if you do and let me know.
