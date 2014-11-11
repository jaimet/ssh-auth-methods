ssh-auth-methods
================

A simple, single-function Python 3 script that returns a list of the authentication methods supported by an SSH server.

## Use

The next two subsections assume that you're using this script as
a stand-alone shell script. The function can be imported to Python 3
code. See the section *Function* for a discussion of this.

### Input

The script reads from standard input. To test Google, you can simply use:

`echo 'google.com' | python3 ssh_auth_methods.py`

The input can be domain names or IP addresses.

Multiple inputs are expected to be line-delimited. To test each
in a file of line-delimited addresses, simply use:

`cat my_addrs.txt | python3 ssh_auth_methods.py`

### Output

The ouput is one address per line. Each line is tab-delimited, with
the supplied address as its first field, followed by each authentication method
supported by the corresponding SSH server.

Nothing is printed after the address in the case that the address
could not be contacted, or that there was an error parsing the response
received.

The auth method 'none' implies that the SSH server let us in without any
authentication at all. I suspect this is possible, although I haven't encountered
it. In this case, we can't find what the other potential auth methods
are (nor do they really matter), so 'none' will be the only one returned.
Additionally, we assume that any non-error (i.e. not `255`) SSH exit status
means the login was successful. This is because 255 is OpenSSH's only
error status, and all other statuses are those of the remotely executed
command.

### Function

The sole function in this module is `get_auth_methods()`. It takes the
hostname scanned, an optional port number (defaulting to `22`), and a
boolean determining verbosity (defaulting to False).

Support will soon be added for threaded calls on multiple addresses.

## Dependencies

This currently only runs on Python 3. It's short and simple, so porting it should be simple.
I may do so myself at some point if people use this.

As mentioned in the *Output* section, using a client other than OpenSSH
may cause unpredictable results because of exit statuses. Namely, if
failed authentication is ever indicated by an exit status other than `255`,
a servers will be falsely reported as allowing unauthenticated login.