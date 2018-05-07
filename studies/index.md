Studies
=======

Studies within APEX are a way of conveniently managing experiments and results.
Using Libgit2, APEX can download experiments from a git repository and
optionally upload results to a branch in that repository.

Public studies only download experiments, while private studies will also
attempt to upload their results to the repository.

Note that this documentation refers to an internal page for managing studies,
and as such is not (completely) applicable to external installations.

Getting started
---------------

### creating a study

Go to `https://exporl.med.kuleuven.be/apex-study/` and press the
`Studies` button. On the studies page press `Add study`. Fill in the
form and press `Submit`.

You can also update a study (add branches and view linked
devices) on the studies page.

### linking to public study

1. In APEX go to `file > link a new study`. If no studies are present you will
   be sent to the `Secure Shell keys` tab.

2. Copy paste the contents of the `Public key` box into the device dialog at
   `https://exporl.med.kuleuven.be/apex-study/`. On Android you can press
   the `Share` button and a browser with this page will be opened, with
   the url already filled in.

3. Give the device access to your study. At
   `https://exporl.med.kuleuven.be/apex-study/` go to `Link device to study`.
   Click on a device (or multiple ones), select a study and one or more branches
   and press `Link`.

4. Then go to `New...` on the `Studies` tab.

5. Simple paste the url to your git repository inside the `Url` field.
   Libgit2 can use http/https[^1] or ssh.

6. Uncheck `Upload results`. Select the branch containing the experiment files,
   and click `Link study`.

### linking to private study

1. In APEX go to `file > link a new study`. If no studies are present you will
   be sent to the `Secure Shell keys` tab.

2. Copy paste the contents of the `Public key` box into the device dialog at
   `https://exporl.med.kuleuven.be/apex-study/`. On Android you can press
   the `Share` button and a browser with this page will be opened, with
   the url already filled in.

3. Give the device access to your study. At
   `https://exporl.med.kuleuven.be/apex-study/` go to `Link device to study`.
   Click on a device (or multiple ones), select a study and one or more branches
   and press `Link`.

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
