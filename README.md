# useful_shell_scripts
some scripts and snippets for working on the shell

## getlinks.sh
Download the files from a webpage
```bash
./getlinks.sh <URL> | grep <potential other domain> | xargs -n1 curl -LO
```

## rename files
```
for f in *\ *; do mv "$f" "${f// /_}"; done
```
Broken down:
- *\ * selects all files with a space in their name as input for the the for loop.
- the quotes around "$f" are important because we know there's a space in the filename and otherwise it would appear as 2+ arguments to mv.
- ${f//str/new_str} is a bash-specific string substitution feature. All instances of str are replaced with new_str.
