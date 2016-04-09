# filtor

A whitelisting proxy for the Tor control port.

## Rationale
The Tor Browser, when not used in "Transparent Torification" mode, requires
access to the Tor control port so that it can test that the browser is
configured to use Tor correctly and send a NEWNYM signal when the "New
Identity" button is clicked.

While these are nice features to have, exposing the control port to the browser
results in an increased attack surface. For example, an attacker could tamper
with the Tor configuration to only use relays that they control or obtain the
IP address of the machine running Tor by using the `GETINFO address` command.

Inspired by a [similar project written as part of
Whonix](https://www.whonix.org/wiki/Dev/Control_Port_Filter_Proxy), I wrote a
simple proxy that only allows commands necessary for operation of the Tor
Browser and rejects all others.

Note that this simple proxy does not enforce authentication; I do not believe
there are significant anonymity concerns with this, but this does present an
increased attack risk if someone compromises a user account on your machine. In
the future, I may add support for a separate auth cookie for the proxy itself,
so that authentication can be used without exposing the main Tor auth cookie
to your Torified VMs.

## Prerequisites
* Python 3+

## Usage
Note: if you already have Tor's control port running on port 9151
(`ControlPort 9151`), you can use filtor's `-p` option to change the port.

1. Install the proxy. Arch users can find a PKGBUILD in the AUR; other users
   should copy filtor to /usr/bin, and filtor.service to /etc/systemd/system.
   (If you're not using systemd, you'll need to write your own initscript.)

2. Configure Tor; set the following options (and restart if necessary):
   ```
   ControlSocket /run/tor/control
   ControlSocketsGroupWritable 1
   CookieAuthentication 1
   CookieAuthFile /run/tor/control.authcookie
   CookieAuthFileGroupReadable 1
   ```

3. Create a user for filtor: `useradd -r -g tor -d / -s /sbin/nologin filtor`

4. Start filtor: `sudo systemctl start filtor.service`

## Security Advisory
Commit ad6a56520c8ae30d486200bb9c105c2de8b53fc3 introduced support for the
`ADD_ONION` command without filtering the allowed ports. This was on by default
and would allow an attacker to create an onion service for the actual control
port and bypass the filter. This feature was disabled by default with commit
19b7f7828c141bf9e326754e75ffc45ebabcc20b and must be enabled with the
`--allow-add-onion` argument.
