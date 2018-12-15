Studies
=======

Studies within APEX are a way of conveniently managing experiments and results.
Using Libgit2, APEX can download experiments from a git repository and
optionally upload results to a branch in that repository.

Public studies only download experiments, while private studies will also
attempt to upload their results to the repository.

Note that this documentation refers to an internal page for managing studies,
and as such is not (completely) applicable to external installations [^1].

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

2. Press the `Share` button and a browser with this page will be opened, with
   the url already filled in.

3. Give the device access to your study. At
   `https://exporl.med.kuleuven.be/apex-study/` go to `Link device to study`.
   Click on a device (or multiple ones), select a study and one or more branches
   and press `Link`.

4. Then go to `New...` on the `Studies` tab.

5. Paste the url to your git repository inside the `Url` field.
   Libgit2 can use http/https[^2] or ssh. 
   In the the `https://exporl.med.kuleuven.be/apex-study/` web interface, this 
   url can be copied from the 'Study' tab. The interface will create a user with the same name as your device, so an example url 
   could be `ssh://3102237ea402b301@exporl-ssh.med.kuleuven.be:8444/name-of-repo`

6. Uncheck `Upload results`. Select the branch containing the experiment files,
   and click `Link study`.

### linking to private study

1. In APEX go to `file > link a new study`. If no studies are present you will
   be sent to the `Secure Shell keys` tab.

2. Press the `Share` button and a browser with this page will be opened, with
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

### uploading experiments

To upload new experiments all that is needed is to push experiments to a
specific branch of the git repository. For instance if a study was created with
an experiment branch called `experiments-1` the workflow would be as followed:

```shell
# Clone the repository
# you can find the url on
# https://exporl.med.kuleuven.be/apex-experiments/
# under projects.
git clone <url>
cd <repository>
# First create a commit.
git add <experiment files and stimuli>
git commit

# Push to the experiment branch.
git push origin HEAD:refs/heads/experiments-1
```

If there is a flowrunner (.apf file) present in the root directory of the
project, Apex will open that file when starting the study. If no flowrunners are
present in the project root directory, the user will be able to choose out of a
list containing all the apf and apx files present in the project (so including
subfolders) each time the study is started.

If multiple flowrunners are present in the project root directory, *index.apf*
will take precedence. If the latter is not present, one is chosen at random.

### fetching results

Fetching the results is somewhat complex because the result branches don't have
any shared history. Result branches are prefixed with `results-`. `git branch
--list -a origin/results-*` lists all result branches.

```shell
# Clone the repository
# you can find the url on
# https://exporl.med.kuleuven.be/apex-experiments/
# under projects.
git clone <url>
cd <repository>

# Checkout a branch to collect the results on.
git checkout --orphan collected-results
git rm --cached -r .
git clean -fd

# Create a base commit, merging won't work without one.
git commit --allow-empty -m "Base commit for merge."

# List all result branches.
BRANCHES=$(git branch -a --list origin/results-*)
# Merge all branches onto collected-results.
# The actual merge will fail,
# but it will give the right history.
git merge --allow-unrelated-histories -s octopus $BRANCHES
# Read the content of each branches into the index.
git read-tree $BRANCHES
# Commit them into collected-results.
git commit -m "Merge $BRANCHES into collected-results."
# Reset index.
git reset --hard
```

The results should now be collected on the `collected-results` branch, and
present in your working directory. To update the branch with new results, do
`git fetch origin; git checkout collected-results;` and then repeat the steps in
the above script starting from *"\#List all result branches."*.

### manually fetching the results

All Apex files associated with a study and their results are saved to a specific folder associated with the study. These folders can be found inside the apexStudies folder, which in term can always be found in a user-visible location on all platforms. These files can always be retrieved manually in case the user wants to check them manually.
On Android devices a special folder is made in the sdcard root (the first visible location in the Android file system). On windows, the study folder can be found in `..\AppData\Local\apexStudies\` .

Note that in older versions of Apex the study save path on Android devices used to be a hidden in the Apex data folder. Old studies are not automatically moved to the new location, so in order to manually access this location you must use Android debug tools or Android Studio.

Results created by Apex experiments inside a study are always saved to the same location. In the case of public studies results are saved to the general study folder, in the case of private studies this is a dedicated study results folder. This is important in the latter case, since the user expects results to be properly synchronized through the study functionality. This is only possible when the results are saved to the correct location.
Note that while using a flowrunner, one can usally choose a custom path to save the results of an experiment. In order to respect the previous rule however, only the specified file name is actually used. Any user-specified result path is ignored when using a flowrunner inside an active study.

Security
--------

For private studies it is recommended to use the ssh protocol. And to set up the
server in such a way that a single device can only access the experiment branch
(read-only) and the results branch belonging to that device. This way a device
cannot read results belonging to other devices.

#### Notes
[^1]: For more technical information see [Setting up Gerrit for use with Apex
    studies](../development/misc/studies-gerrit#Setting up Gerrit for use with
    Apex studies)

[^2]: Note that on Linux Libgit2 provided by the package manager might not be
    able to use the https protocol because of OpenSSL license issues.
