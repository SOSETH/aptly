# Aptly
Create and serve a debian repostitory from a bunch of `*.deb`s.
Tested on:
 * Debian 9

## Usage
Start by listing the repositories you want to create in `aptly_repos`. You will
need a separate repostitory for each distribution you want to serve. You will
also need to think about key management, you can either leave
`aptly_gpg_generate_key` set to `true` which will result in a passwordless 2048
bit RSA key being generated or set it to false and put your own keyring in
`files`.
After running the role, su to `aptly_repo_user` and use `aptly repo add <name> <debs>`
to add packages. If you use your repostitory for the first time, you'll need to
tell aptly to publish it:
`aptly publish repo -distribution="<distribution, e.g. stretch>" -architectures=<architectures, e.g. 'amd64,all'> <name> filesystem:default:<name>`
This will inform aptly about both the distribution and the architectures for
each repostitory which cannot be changed afterwards. After subsequent adds, you
can to update the repostitory using
`aptly publish update <distribution> filesystem:default:<name>`

## Configuration
| Variable | Default value | Description |
| -------- | ------------- | ----------- |
| `aptly_repo_user` | `repo` | How is the repo user supposed to be called? |
| `aptly_repo_user_home` | `/home/repo` | Where is the repo users' home supposed to be? |
| `aptly_repo_group` | `repo` | How is the default group of the repo user supposed to be called? |
| `aptly_gpg_generate_key` | `True` | Whether to have this role generate a passwordless RSA key |
| `aptly_gpg_key_name` | `repo` | Name under which the GPG key should be stored in the webroot ({{ name }}.asc) |
| `aptly_repos` | `[]` | An array of repositories to create |
| `aptly_webroot_path` | `/var/www/html/aptly` | Where to put the finished repo |
