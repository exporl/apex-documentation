Preparing a release version of Apex
===================================

Making the changes
------------------

The linux-new-version script in tools makes all the needed changes to the
worktree, it will:

* Spawn an editor to edit the version header.
* Update the schema version of the XML and XSD files found in data/schema,
  data/config, and in examples.
* Call the apex-schema-documentation script, which&mdash;when supplied with
  the `--oxygen` directory&mdash;will make the necessary changes to the
  apex3-documentation-schemas worktree.

The changes will still need to be committed. Start in
apex3-documentation-schemas, which is a submodule of apex3-documentation. Then
commit the updated submodule to apex3-documentation. As a final step commit the
documentation submodule together with the changes made in the Apex root project.

Releasing binaries
------------------

Binaries should be released for Windows and Android. Do this after the
changes have been committed and a master build has been completed.

### Windows

Copy the installer (`.msi` file) built by the buildserver to the folder with
similar files. Then add a link on the downloads page to the new installer.

### Android

To release a new Android version to the F-Droid repository, trigger the
apex-android-release project on the buildserver. It's best to do this right
after the master build for the Windows binaries as it will build the latest
commit of the master branch.

When the build is completed the buildserver will trigger the
apex-android-release-deploy project on the buildserver. This will call the
deploy script on the buildserver. See the tools/fdroid folder as an example.
