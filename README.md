# rocky-linux-wikibox


## Prerequisites

This Vagrant configuration is written to be used with VirtualBox as as the provider and has been developed and tested on...

- macOS Monterey (12.3.1)
- HashiCorp Vagrant (2.2.19)
- Oracle VirtualBox (6.1.30r148432) w/ Guest Additions
- Rocky Linux 8 Official Vagrant Box (https://app.vagrantup.com/rockylinux/boxes/8)


## Overview

This Vagrant configuration will help you provision a Vagrant machine with a full `mkdocs` environment isolated from your host machine. If you have VirtualBox Guest Additions installed the documentation will be cloned into the default shared folder causing it to be resident on your host and **not** inside the guest. This will persist your documentation through reprovisioning cycles of the guest.


## Starting the box

Follow the sequence below to download the Rocky Linux 8 base box, provision your VirtualBox vm and clone the Rocky Linux documentation.


```
➜  rocky-linux-wikibox vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'rockylinux/8' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: 4.0.0
==> default: Loading metadata for box 'rockylinux/8'
    default: URL: https://vagrantcloud.com/rockylinux/8
==> default: Adding box 'rockylinux/8' (v4.0.0) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/rockylinux/boxes/8/versions/4.0.0/providers/virtualbox.box
Progress: 30% (Rate: 27.8M/s, Estimated time remaining: 0:00:22)
```

...after the download completes...

```
==> default: Successfully added box 'rockylinux/8' (v4.0.0) for 'virtualbox'!
==> default: Importing base box 'rockylinux/8'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'rockylinux/8' version '4.0.0' is up to date...
==> default: Setting the name of the VM: rockylinux_default_1624755268355_99340
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 8000 (guest) => 8000 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
```

...once VM has started Vagrant will login and mount the shared folders...

```
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Setting hostname...
==> default: Mounting shared folders...
    default: /vagrant => /Users/tcooper/boxes/rockylinux/rocky-linux-wikibox
```

...next the shell provisioners will run. First, the VM is updated and required software is installed. Some content is removed for brevity...

```
==> default: Running provisioner: update (shell)...
    default: Running: script: update
    default: Rocky Linux 8 - AppStream                       6.0 MB/s | 7.1 MB     00:01
    default: Rocky Linux 8 - BaseOS                          2.1 MB/s | 2.5 MB     00:01
    default: Rocky Linux 8 - Extras                          4.4 kB/s | 2.7 kB     00:00
    default: Dependencies resolved.
    default: Nothing to do.
    default: Complete!
    default: Last metadata expiration check: 0:00:01 ago on Sun 27 Jun 2021 12:54:55 AM UTC.
    default: Dependencies resolved.
    default: ================================================================================
    default:  Package               Architecture    Version            Repository       Size
    default: ================================================================================
    default: Installing:
    default:  epel-release          noarch          8-10.el8           extras           22 k
    default:
    default: Transaction Summary
    default: ================================================================================
    default: Install  1 Package
    default: Total download size: 22 k
    default: Installed size: 32 k
    default: Downloading Packages:
    default: epel-release-8-10.el8.noarch.rpm                 60 kB/s |  22 kB     00:00
    default: --------------------------------------------------------------------------------
    default: Total                                            41 kB/s |  22 kB     00:00
    default: Running transaction check
    default: Transaction check succeeded.
    default: Running transaction test
    default: Transaction test succeeded.
    default: Running transaction
    default:   Preparing        :                                                        1/1
    default:
    default:   Installing       : epel-release-8-10.el8.noarch                           1/1
    default:
    default:   Running scriptlet: epel-release-8-10.el8.noarch                           1/1
    default:
    default:   Verifying        : epel-release-8-10.el8.noarch                           1/1
    default:
    default:
    default: Installed:
    default:   epel-release-8-10.el8.noarch
    default:
    default: Complete!
    default: Extra Packages for Enterprise Linux Modular 8 - 469 kB/s | 663 kB     00:01
    default: Extra Packages for Enterprise Linux 8 - x86_64  6.4 MB/s | 9.7 MB     00:01
    default: Dependencies resolved.
    default: =========================================================================================
    default:  Package                  Arch    Version                                Repo        Size
    default: =========================================================================================
    default: Installing:
    default:  git                      x86_64  2.27.0-1.el8                           appstream  163 k
    default:  python36                 x86_64  3.6.8-2.module+el8.3.0+120+426d8baf    appstream   18 k
    default:  screen                   x86_64  4.6.2-12.el8                           epel       581 k
    default:  vim-enhanced             x86_64  2:8.0.1763-15.el8                      appstream  1.4 M
    default: Installing dependencies:
    default:  emacs-filesystem         noarch  1:26.1-5.el8                           baseos      68 k
    default:  git-core                 x86_64  2.27.0-1.el8                           appstream  5.7 M
    default:  git-core-doc             noarch  2.27.0-1.el8                           appstream  2.5 M

    ...<snip>...

    default:  python3-setuptools       noarch  39.2.0-6.el8                           baseos     162 k
    default:  vim-common               x86_64  2:8.0.1763-15.el8                      appstream  6.3 M
    default:  vim-filesystem           noarch  2:8.0.1763-15.el8                      appstream   47 k
    default: Installing weak dependencies:
    default:  perl-IO-Socket-IP        noarch  0.39-5.el8                             appstream   46 k
    default:  perl-IO-Socket-SSL       noarch  2.066-4.module+el8.4.0+512+d4f0fc54    appstream  297 k
    default:  perl-Mozilla-CA          noarch  20160104-7.module+el8.4.0+529+e3b3e624 appstream   14 k
    default: Enabling module streams:
    default:  python36                         3.6
    default:
    default: Transaction Summary
    default: =========================================================================================
    default: Install  56 Packages
    default: Total download size: 29 M
    default: Installed size: 112 M
    default: Downloading Packages:
    default: (1/56): git-2.27.0-1.el8.x86_64.rpm             408 kB/s | 163 kB     00:00
    default: (2/56): gpm-libs-1.20.7-17.el8.x86_64.rpm       337 kB/s |  38 kB     00:00
    default: (3/56): perl-Digest-1.17-395.el8.noarch.rpm     348 kB/s |  26 kB     00:00

    ...<snip>...

    default: (54/56): python3-setuptools-39.2.0-6.el8.noarch 3.4 MB/s | 162 kB     00:00
    default: (55/56): screen-4.6.2-12.el8.x86_64.rpm         2.0 MB/s | 581 kB     00:00
    default: (56/56): perl-interpreter-5.26.3-419.el8.x86_64 8.8 MB/s | 6.3 MB     00:00
    default: --------------------------------------------------------------------------------
    default: Total                                           8.9 MB/s |  29 MB     00:03
    default: Running transaction check
    default: Transaction check succeeded.
    default: Running transaction test
    default: Transaction test succeeded.
    default: Running transaction
    default:   Preparing        :                                                        1/1
    default:
    default:   Installing       : git-core-2.27.0-1.el8.x86_64                          1/56
    default:
    default:   Installing       : git-core-doc-2.27.0-1.el8.noarch                      2/56
    default:
    default:   Installing       : perl-Digest-1.17-395.el8.noarch                       3/56
    default:

    ...<snip>...

    default:   Installing       : screen-4.6.2-12.el8.x86_64                           56/56
    default:
    default:   Running scriptlet: screen-4.6.2-12.el8.x86_64                           56/56
    default:
    default:   Running scriptlet: vim-common-2:8.0.1763-15.el8.x86_64                  56/56
    default:
    default:   Verifying        : git-2.27.0-1.el8.x86_64                               1/56
    default:   Verifying        : git-core-2.27.0-1.el8.x86_64                          2/56
    default:   Verifying        : git-core-doc-2.27.0-1.el8.noarch                      3/56

    ...<snip>...

    default:   Verifying        : perl-threads-shared-1.58-2.el8.x86_64                54/56
    default:   Verifying        : python3-setuptools-39.2.0-6.el8.noarch               55/56
    default:   Verifying        : screen-4.6.2-12.el8.x86_64                           56/56
    default:
    default:
    default: Installed:
    default:   emacs-filesystem-1:26.1-5.el8.noarch
    default:   git-2.27.0-1.el8.x86_64
    default:   git-core-2.27.0-1.el8.x86_64

    ...<snip>...

    default:   screen-4.6.2-12.el8.x86_64
    default:   vim-common-2:8.0.1763-15.el8.x86_64
    default:   vim-enhanced-2:8.0.1763-15.el8.x86_64
    default:   vim-filesystem-2:8.0.1763-15.el8.noarch
    default:
    default: Complete!
```

...next mkdocs and additional requirements are installed.  Some content is removed for brevity...

```
==> default: Running provisioner: setup_mkdocs (shell)...
    default: Running: script: setup_mkdocs
    default: Collecting mkdocs
    default:   Downloading https://files.pythonhosted.org/packages/07/61/bdf4f713e78e0e058c75900b1d20bd09a2139f0b0526fef05634f50e3a0f/mkdocs-1.2.1-py3-none-any.whl (6.3MB)
    default: Collecting importlib-metadata>=3.10 (from mkdocs)
    default:   Downloading https://files.pythonhosted.org/packages/23/5d/f2151217058e7d5c5b4b584bc6052c2ae478c5a36b58a056364351613bfb/importlib_metadata-4.5.0-py3-none-any.whl
    default: Requirement already satisfied: PyYAML>=3.10 in /usr/lib64/python3.6/site-packages (from mkdocs)
    default: Collecting Markdown>=3.2.1 (from mkdocs)
    default:   Downloading https://files.pythonhosted.org/packages/6e/33/1ae0f71395e618d6140fbbc9587cc3156591f748226075e0f7d6f9176522/Markdown-3.3.4-py3-none-any.whl (97kB)

    ...<snip>...

    default: Installing collected packages: mkdocs-windmill, mkdocs-material-extensions, pygments, pymdown-extensions, mkdocs-material, termcolor, mkdocs-macros-plugin, bracex, wcmatch, mkdocs-awesome-pages-plugin
    default:   Running setup.py install for mkdocs-windmill: started
    default:     Running setup.py install for mkdocs-windmill: finished with status 'done'
    default:   Running setup.py install for termcolor: started
    default:     Running setup.py install for termcolor: finished with status 'done'
    default: Successfully installed bracex-2.2.1 mkdocs-awesome-pages-plugin-2.7.0 mkdocs-macros-plugin-0.7.0 mkdocs-material-8.2.8 mkdocs-material-extensions-1.0.3 mkdocs-windmill-1.0.5 pygments-2.11.2 pymdown-extensions-9.1 termcolor-1.1.0 wcmatch-8.3
```

...next the Rocky Linux wiki repository is cloned with git...

```
==> default: Running provisioner: clone_wiki (shell)...
    default: Running: script: clone_wiki
    default: /vagrant ~
    default: Cloning into 'wiki.rockylinux.org'...
    default: ~
```

...and finally, instructions are provided on how to login to the box and start `mkdocs` in local server mode.

```
==> default: Running provisioner: mkdocs_serve (shell)...
    default: Running: script: mkdocs_serve
    default:
    default: Login to the docbox with the command...
    default:
    default:     vagrant ssh -- -L 8000:localhost:8000
    default:
    default: Then run mkdocs with the command sequence...
    default:
    default:     cd /vagrant/wiki.rockylinux.org
    default:     screen
    default:     mkdocs serve 2>&1 | tee ./mkdocs.serve.log
    default:
    default: Then, optionally detach from screen with the key sequence...
    default:
    default:     [CTRL-a] d
    default:
    default: From the host you'll be able to watch the mkdocs serve log with...
    default:
    default:     tail -f wiki.rockylinux.org/mkdocs.serve.log
    default:
    default: If you want to trigger a rebuild of the docs inside the guest you
    default: _must_ touch a monitored file/directory inside the guest. Filesystem
    default: events are not passed from the host to the guest even though actual
    default: content changes will be seen. Do that with the following command...
    default:
    default:     touch /vagrant/wiki.rockylinux.org/docs/docs/index.md
    default:
    default: Even if that isn't the file you have modified the entire documentation
    default: tree will be rebuilt. The page you are viewing in the brower in your
    default: host will update automatically when the docs are rebuilt.
```

## Running mkdocs

The VirtualBox guest is now running and ready to serve the Rocky Linux documentation in your local development system.


### Login to the box

Depending on your host configuration (ie. firewall) you may not be able to access the web server running inside the box. A simple way around this issue is to use ssh to create a tunnel into the box when you login to start the `mkdocs` server. That method is shown below...

```
➜  rocky-linux-wikibox vagrant ssh -- -L 8000:localhost:8000
[vagrant@rockylinux wiki.rockylinux.org ~]$
```

### Change to wiki.rockylinux.org directory and start mkdocs

```
[vagrant@rockylinux wiki.rockylinux.org ~]$ cd /vagrant/wiki.rockylinux.org
[vagrant@rockylinux wiki.rockylinux.org wiki.rockylinux.org]$ screen
[vagrant@rockylinux wiki.rockylinux.org]$ mkdocs serve 2>&1 | tee ./mkdocs.serve.log
INFO     -  Building documentation...
INFO     -  [macros] - Macros arguments: {'module_name': 'main', 'modules': [], 'include_dir': 'docs/include', 'include_yaml': [], 'j2_block_start_string': '', 'j2_block_end_string': '', 'j2_variable_start_string': '', 'j2_variable_end_string': '', 'on_undefined': 'keep', 'on_error_fail': False, 'verbose': False}
INFO     -  [macros] - Extra filters (module): ['pretty']
INFO     -  [macros] - Includes directory: docs/include
INFO     -  Cleaning site directory
INFO     -  Documentation built in 0.83 seconds
INFO     -  [macros] - We will also watch: ['docs/include']
INFO     -  [02:50:09] Watching paths for changes: 'docs', 'mkdocs.yml', 'docs/include'
INFO     -  [02:50:09] Serving on http://127.0.0.1:8000/
INFO     -  [02:50:26] Browser connected: http://127.0.0.1:8000/

Press [CTRL-A] then d

[detached from 8011.pts-0.rockylinux]
[vagrant@rockylinux wiki.rockylinux.org wiki.rockylinux.org]$
```

You are now detached from the `mkdocs` process and it is running as if it were a daemonized server.

You can watch the `mkdocs` output by tailing the file we redirected the output to as follows...

#### From inside the guest...

```
[vagrant@rockylinux ~]$ tail -f /vagrant/wiki.rockylinux.org/mkdocs.serve.log
INFO     -  Building documentation...
INFO     -  [macros] - Macros arguments: {'module_name': 'main', 'modules': [], 'include_dir': 'docs/include', 'include_yaml': [], 'j2_block_start_string': '', 'j2_block_end_string': '', 'j2_variable_start_string': '', 'j2_variable_end_string': '', 'on_undefined': 'keep', 'on_error_fail': False, 'verbose': False}
INFO     -  [macros] - Extra filters (module): ['pretty']
INFO     -  [macros] - Includes directory: docs/include
INFO     -  Cleaning site directory
INFO     -  Documentation built in 0.83 seconds
INFO     -  [macros] - We will also watch: ['docs/include']
INFO     -  [02:50:09] Watching paths for changes: 'docs', 'mkdocs.yml', 'docs/include'
INFO     -  [02:50:09] Serving on http://127.0.0.1:8000/
INFO     -  [02:50:26] Browser connected: http://127.0.0.1:8000/
```

#### From outside the guest...

```
➜  rocky-linux-wikibox tail -f wiki.rockylinux.org/mkdocs.serve.log
INFO     -  Building documentation...
INFO     -  [macros] - Macros arguments: {'module_name': 'main', 'modules': [], 'include_dir': 'docs/include', 'include_yaml': [], 'j2_block_start_string': '', 'j2_block_end_string': '', 'j2_variable_start_string': '', 'j2_variable_end_string': '', 'on_undefined': 'keep', 'on_error_fail': False, 'verbose': False}
INFO     -  [macros] - Extra filters (module): ['pretty']
INFO     -  [macros] - Includes directory: docs/include
INFO     -  Cleaning site directory
INFO     -  Documentation built in 0.83 seconds
INFO     -  [macros] - We will also watch: ['docs/include']
INFO     -  [02:50:09] Watching paths for changes: 'docs', 'mkdocs.yml', 'docs/include'
INFO     -  [02:50:09] Serving on http://127.0.0.1:8000/
INFO     -  [02:50:26] Browser connected: http://127.0.0.1:8000/
```


## Forcing rebuild

Due to the nature of the way `mkdocs serve` watches for changes in the local filesystem when you modify the documentation in your development system the changes will appear inside your guest but `mkdocs` will not see the update and rebuild the documentation automatically.

You can trigger a re-build by touching any file or directory that `mkdocs` is watching _from inside the guest_ as follows...

```
[vagrant@rockylinux ~]$ touch /vagrant/wiki.rockylinux.org/docs/index.md
```

This will generate an updated build...

```
INFO     -  [02:54:23] Detected file changes
INFO     -  Building documentation...
INFO     -  [macros] - Macros arguments: {'module_name': 'main', 'modules': [], 'include_dir': 'docs/include', 'include_yaml': [], 'j2_block_start_string': '', 'j2_block_end_string': '', 'j2_variable_start_string': '', 'j2_variable_end_string': '', 'on_undefined': 'keep', 'on_error_fail': False, 'verbose': False}
INFO     -  [macros] - Extra filters (module): ['pretty']
INFO     -  [macros] - Includes directory: docs/include
INFO     -  [02:54:25] Reloading browsers
INFO     -  [02:54:25] Browser connected: http://127.0.0.1:8000/
```

...and the browser session will reload the page it has open.
