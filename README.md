# gnupg-win32-proxy

Do you
* use GnuPG on Windows?
* also use Cygwin?
* want to forward your gpg-agent socket to another host with SSH?

No? That's a relief.

---

[GnuPG builds for Windows use a TCP socket][lolwindows] since UNIX sockets kind of don't exist on Windows. To prevent other users on the same system from using the socket, a 16-byte nonce is used to authenticate the connection.

This Python 3 script is meant to be run within Cygwin. It opens a "UNIX" "socket", and upon receiving a connection, it opens a connection to the gpg-agent's TCP socket and writes the nonce to it, then proxies the rest of the communication.

You should only need python3 installed in Cygwin, and [a Windows binary release of GnuPG](https://www.gnupg.org/download/) on your system. You can use `gpg-connect-agent /bye` to make sure the agent is running.

After running this script, you can use something like
```
ssh -vNR $HOME/.gnupg/S.gpg-agent:$HOME/.S.gpg-agent-relay $HOSTNAME
```
to forward your gpg-agent to another host.

[lolwindows]: https://lists.gnupg.org/pipermail/gnupg-devel/2007-October.txt
