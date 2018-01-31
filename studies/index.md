Studies
=======

Studies within APEX are a way of conveniently managing experiments and results.
Using Libgit2, APEX can download experiments from a git repository and
optionally upload results to a branch in that repository.

Public studies only download experiments, while private studies will also
attempt to upload their results to the repository.

Getting started
---------------

### public study

1. Create a git repository on a server. Commit your experiments to a branch in
   this repository.

2. In APEX go to `file > link a new study`. If no studies are present you will
   be sent to the `Secure Shell keys` tab.

3. For a public study you can ignore this and go straight to `New...` on the
   `Studies` tab.

4. Simple paste the url to your git repository there. Libgit2 can use
   http/https[^1] or ssh.

5. Uncheck `Upload results`. Select the branch containing the experiment files,
   and click `Link study`.

### private study

1. Create a git repository on a server. Commit your experiments to a branch in
this repository.

2. In APEX go to `file > link a new study`. If no studies are present you will
   be sent to the `Secure Shell keys` tab.

3. For a private study use the secure shell key to give your machine access[^2]
   to your repository.

4. Then go to `New...` on the `Studies` tab.

5. Paste the url to your git repository here. It is recommended to use the ssh
   protocol.

6. Select the branch containing the experiment files. If no branches show up,
   make sure the machine has access to your repository. Once a branch is
   selected click `Link study`.

Security
--------

For private studies it is recommended to use the ssh protocol. And to set up the
server in such a way that a single device can only access the experiment branch
(read-only) and the results branch belonging to that device. This way a device
cannot read results belonging to other devices.

#### Notes

[^1]: Note that on Linux Libgit2 provided by the package manager might not be
    able to use the https protocol because of OpenSSL license issues.

[^2]: Example [Github
    guide](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/#platform-windows). Use
    the public key from the studies dialog.
