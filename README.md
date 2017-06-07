# useful_shell_scripts
some scripts and snippets for working on the shell

## getlinks.sh
Download the files from a webpage
```bash
./getlinks.sh <URL> | grep <potential other domain> | xargs -n1 curl -LO
```