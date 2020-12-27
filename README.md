# sshare

> Simple file sharing concept using ssh, bash and a web server

Pipe the file you want to share into ssh and optionally provide a desired file name. The SSH server will return a link to the shared file.

```
$ ssh -T example.com "README.txt" < somefile.txt
https://example.com/dl/frKeu0hLSiHF5ru6-README.txt
```

A small utility script `share` is included to simplify sharing of files and directories.

```
$ ./share
Usage: ./share FILE/DIRECTORY [NAME]
$ ./share somefile.txt "README.txt"
https://example.com/dl/OjO9lAKgDgdGW6Ie-README.txt
$ ./share somedirectory/ "documents"
https://example.com/dl/YQWzKq3rObfkBzmk-documents.zip
```

## Installation

An OpenSSH and web server are required.

1. Create a system user with SSH login permission.
2. Copy the `share-ssh` script into the user's home directory.
3. Add your ssh key(s) to the `$HOME/.ssh/authorized_keys` file and prepend them with command=...  
   `command="./share-ssh",restrict ssh-ed25519 AA... mykey`
4. Edit `SHARE_DIR` and `SHARE_HOST` in `share-ssh` to match your setup. Configure your web server to serve static files from `SHARE_DIR`.

On your client machine, edit `$HOME/.ssh/config` to use the correct user and key:
```
Host example.com
	HostName example.com
	User share
	IdentityFile ...
```

You should now able to use `ssh -T example.com < FILE` or the provided `share` script (adjust `SHARE_HOST`) to share files.

## Deleting old files

To regularly delete old shared files, you can use the `share-clean` script in a cronjob. By default, files older than 7 days are deleted.
```
MAILTO=""

0 * * * * ~/share-clean
```
