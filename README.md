# Mentoring sessions for fixing STOICAL for today's machines

Or maybe rather "coworking", since I'm not going to be the perfect
mentor, and attendees will probably know things that I don't.

I'll post here if/when sessions will be happening.

Send a mail with subject "STOICAL mentoring" to Christian Jaeger
<ch@christianjaeger.ch> if you'd like to be notified when there are
news. (If you've got a Github account you could also enable
notifications on this repo.)

I'm currently planning to do the sessions via Jitsi meet, and
(optionally) allow anyone interested to access the VNC session on the
shared server directly. I'll also share my desktop in Jitsi for those
who don't want to or can't use VNC (VNC does have better frame rates
and perfectly sharp pictures, though).

## VNC client setup

Again, you can ignore this section if you're happy with the quality of
the screen share that I'll offer via Jitsi and don't want to interact
actively with the session.

Since standard VNC doesn't offer encryption, it's usually used via
some kind of tunneling, I'm normally using SSH tunneling. Thus to
access our VNC session directly (not just looking at it in Jitsi),
I'll ask for your SSH public key, which I'll install on the
server. Also, VNC itself uses a password, which will be the value
`12345678` here.

### Linux and alikes

*Note: stoical-mentoring.christianjaeger.ch is not active yet. I'll
let you know when it is so you could test in advance if you'd like.*

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
    to go to the server.
    
    The MenuKey, F7, will allow you to open a menu locally that will
    allow you to disable full-screen mode or exit the VNC viewer.

Then, to access the session, you need to first run
`stoical-mentoring-tunnel` in a terminal and keep it open, and then
run `stoical-mentoring-vnc`.

### Other systems

I'm not using other systems and don't know the best ways there. I'll
be happy if you do and let me know.
