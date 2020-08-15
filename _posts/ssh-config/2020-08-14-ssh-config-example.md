## man ssh config

create `config` under `.ssh/`

### example

```bash
Host pi1 omv pihole
    Hostname 192.168.1.95
    User pi

Host pi2
    Hostname 192.168.1.250
    User pi
```


### Documentation

```
Host    Restricts the following declarations (up to the next Host key‐
        word) to be only for those hosts that match one of the patterns
        given after the keyword.  If more than one pattern is provided,
        they should be separated by whitespace.
```

```
 HostName   Specifies the real host name to log into.  This can be used to
            specify nicknames or abbreviations for hosts.  If the hostname
            contains the character sequence ‘%h’, then this will be replaced
            with the host name specified on the commandline (this is useful
            for manipulating unqualified names).
```