# curl-bashenv

Love it or hate it, we curl-bash everything else... so why not all the bits of our custom, tricked-out shell environments?!

The general idea behind this tool is that one can mirror their shell environment, SSH, and Git configs (and, potentially, others) to another host just by curl-bashing. This is great for situations, including new machines, shorter-lived machines where you want to feel at home, without a lot of muss-and-fuss.

"But I keep all my shell configs in a Git repo, so all I have to do is clone!" curl-bashenv handles that, too, including symlinking and setting up SSH keys.

"Ok... tell me more."

A standard usage case might look like:

```
[you@new-host] $ export SOURCE="some.other.machine"
[you@new-host] $ export SSH_KEY_MODE="createauthorize"
[you@new-host] $ curl https://github.com/you/curl-bashenv/curl-bashenv | bash
```

## Sources

Setting the source let's curl-bashenv know where to grab your configs from; currently two sources are supported:

* **Another machine**: will access and duplicate your configs via ssh/scp; can be of form ```hostname.tld``` or ```user@host```.
* **A Git repository**: will clone a Git repo containing your home directory configs. Supports a remote Git repo (i.e. ```some.machine.tld:/git/repo.git```) or Github, however Github support is kinda janky right now, due to ssh key exchanges

Set the source by exporting the ```SOURCE``` environment variable.

## Steps

```curl-bashenv``` allows you to cutomize the steps it will perform on your behalf. Currently, three steps are implemented:

* **SSH**: this step has a number of different modes that will either duplicate an ssh key pair from the source, create a new ssh key pair, and create a new ssh key pair _and_ authorize it on the source host. For more details, see below. (step name: ```ssh```)
* **bash dot files**: this step copies or symlinks in bash shell config files (step name: ```shdotfiles```)
* **Git**: this step copies your ```.gitconfig``` from the source, or symlinks it in from your git repo (step name: ```gitconfig```)

Which steps are run can be controlled by the ```SETUP_STEPS``` environment variable, i.e. ```export SETUP_STEPS="ssh shdotfiles"``` will only run the SSH and bash dotfiles step. By default, all three steps above are run.


