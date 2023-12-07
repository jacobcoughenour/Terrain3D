Building from Source
====================

## 0. Nightly Builds

Github is configured to build automatically on every commit and PR push. If you want a more recent build than the releases, you can download these builds. They are exactly the same as the releases except for the commit used. However, they are inherently less tested and may have more problems than the release builds.

1. [Click here](https://github.com/TokisanGames/Terrain3D/actions/workflows/build.yml?query=branch%3Amain) for `Github Actions`, `Build All` workflow, `main` branch.

2. Click the most recent successful build:

```{image} images/build_workflow.png
:target: ../_images/build_workflow.png
```

3. Download the artifact, which is just a zip file with a "nightly" build.

```{image} images/build_artifact.png
:target: ../_images/build_artifact.png
```

Otherwise, continue on to build from source on your own system. This is necessary for development, and recommended for macOS.

## 1. Install dependencies

Follow Godot's instructions to set up your system to build Godot. You don't need the Godot source code, so stop before then. You only need the build tools, specifically `scons`, `python`, a compiler, and any other tools these pages identify. They provide easy installation instructions.

* [Windows](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_windows.html)
* [Linux/BSD](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_linuxbsd.html)
* [macOS](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_macos.html)


## 2. Download this repository

You can either grab the zip file, or clone it on the command line. Only type in the commands after the $ prompts.

```
$ git clone git@github.com:TokisanGames/Terrain3D.git

Cloning into 'Terrain3D'...
Enter passphrase for key:
remote: Enumerating objects: 125, done.
remote: Counting objects: 100% (125/125), done.
remote: Compressing objects: 100% (79/79), done.
remote: Total 125 (delta 56), reused 94 (delta 36), pack-reused 0
Receiving objects: 100% (125/125), 42.20 KiB | 194.00 KiB/s, done.
Resolving deltas: 100% (56/56), done.

$ git lfs install
Updated Git hooks.
Git LFS initialized.

$ git lfs pull

$ cd Terrain3D

Terrain3D$ git submodule init
Submodule 'godot-cpp' (https://github.com/godotengine/godot-cpp.git) registered for path 'godot-cpp'

Terrain3D$ git submodule update
Cloning into 'C:/GD/Terrain3D/godot-cpp'...
Submodule path 'godot-cpp': checked out '9d1c396c54fc3bdfcc7da4f3abcb52b14f6cce8f'

```
Note the version it checked out: **9d1c396**...

This number is important for the next section.

## 3. Identify the appropriate godot-cpp version

Your version of godot-cpp needs to match the version of your Godot engine. e.g. Godot Engine 4.0.2 and godot-cpp 4.0.2. We are including updated versions in our updates so this step may not be necessary unless you get a lot of build errors or Godot crashes on load.

You can check the version of your godot-cpp by going into that directory and typing `git log`.

In the past, matching this was very strict. At this time, it seems that 4.0 and 4.0.2 are interchangeable. Use one of these steps below to find and select the correct tag or commit hash, then move on to step 4.


### Using tags
On the [repository page](https://github.com/godotengine/godot-cpp), click the branch selector, then `Tags` to identify available tags that match the Godot engine binary you wish to use. If your engine version is in this list, e.g. `godot-4.0.2-stable`, great, move on to step 4. Otherwise explore the commit history on the website or command line as shown below.

```{image} images/build_tags.png
:target: ../_images/build_tags.png
```

### Using the commit history
If your engine version doesn't have a tag assigned, you can look at the [godot-cpp commit history](https://github.com/godotengine/godot-cpp/commits/master) for a commit that syncs the repository to the upstream engine version. Search for entries named `Sync with upstream commit...`.

Eg, from Godot 4.0-stable.

```{image} images/build_commit_history.png
:target: ../_images/build_commit_history.png
```

Clicking the `...` on the right expands the description which shows `4.0-stable`. This is the correct entry, and you want the commit string on the right in blue. Click the two overlapping squares on the right to copy the commit hash (`9d1c396`).


### Using the command line
Alternatively, you can use git to search on the command line. This will search the server (origin) for all commit messages with `stable` allowing you to find the commits. Make sure to grab the top commit message (`9d1c396..`), not the upstream commit shown at the bottom, which is from the Godot repository.

```
Terrain3D/godot-cpp$ git log origin -Gstable
commit 9d1c396c54fc3bdfcc7da4f3abcb52b14f6cce8f (HEAD -> master, tag: godot-4.0-stable, origin/master, origin/HEAD, origin/4.0)
Author: R mi Verschelde <rverschelde@gmail.com>
Date:   Wed Mar 1 15:32:44 2023 +0100

    gdextension: Sync with upstream commit 92bee43adba8d2401ef40e2480e53087bcb1eaf1 (4.0-stable)

```

Note, you may need to update your git repo before it will find the latest tags:

```
Terrain3D/godot-cpp$ git fetch
From https://github.com/godotengine/godot-cpp
 * [new tag]         godot-4.0.1-stable -> godot-4.0.1-stable
 * [new tag]         godot-4.0.2-stable -> godot-4.0.2-stable
```

Now we can search the logs above, or use the new tags that were found, eg, godot-4.0.2-stable.


## 4. Check out the correct version
Once you have the proper tag or commit string, you just need to check it out. If using a commit string, you may use either the full hash or just the first 6-8 characters, so `9d1c396` would also match 4.0-stable.

These examples will change the godot-cpp repository to 4.0-stable and 4.02-stable, respectively:

```
Terrain3D$ cd godot-cpp

Terrain3D/godot-cpp$ git checkout 9d1c396c54fc3bdfcc7da4f3abcb52b14f6cce8f
HEAD is now at 9d1c396 gdextension: Sync with upstream commit 92bee43adba8d2401ef40e2480e53087bcb1eaf1 (4.0-stable)
```

or

```
Terrain3D/godot-cpp$ git checkout godot-4.0.2-stable
Previous HEAD position was 9d1c396 gdextension: Sync with upstream commit 92bee43adba8d2401ef40e2480e53087bcb1eaf1 (4.0-stable)
HEAD is now at 7fb46e9 gdextension: Sync with upstream commit 7a0977ce2c558fe6219f0a14f8bd4d05aea8f019 (4.0.2-stable)

```

## 5. Build the extension

By default `scons` will build the debug library which works for the editor and debug exports. You can add `target=template_release` to build the release version.

```
Terrain3D/godot-cpp$ cd ..

# To build the debug library
Terrain3D$ scons

# To build both debug and release versions sequentially (bash command line)
Terrain3D$ scons && scons target=template_release

```

Upon success you should see something like this at the end:

```
Creating library project\addons\terrain_3d\bin\libterrain.windows.debug.x86_64.lib and object project\addons\terrain_3d\bin\libterrain.windows.debug.x86_64.exp
scons: done building targets.
```

## 6. Set up the extension in Godot

* Close Godot. (Not required the first time, but necessary when updating the files on subsequent builds.)
* Copy `project/addons/terrain_3d` to your own project folder as `/addons/terrain_3d`. 
* Upon opening or switching back to Godot, it should prompt to restart. Do so.
* To finalize resource importing and clear out error messages, it's best to reload the project a second time. 
* Create or open a 3D scene and add a new Terrain3D node.
* Select Terrain3D in the scene tree. In the inspector, click the down arrow to the right of the storage resource and save it as a binary .res file. This is optional, but highly recommended. Otherwise it will save terrain data as text in the current scene file. The other resources can be left as is or saved as text .tres. These external files can be shared with other scenes.

## Other Build Options

The `scons` build system has additional useful options. These come from the GDExtension template we are using, so some options may not be supported or work properly for this particular plugin. e.g. The platform.

### Clean up build files
```
# Linux, other Unix, Git bash on Windows
scons --clean
rm project/addons/terrain_3d/bin/*
find . -iregex '.+\.\(a\|lib\|o\|obj\|os\)' -delete

# Windows
scons --clean
del /q project\addons\terrain_3d\bin\*.*
del /s /q *.a *.lib *.o *.obj *.os
```

### Manually specify the target platform
This plugin supports Windows, Linux and macOS. We've received successful reports of Android (samsung galaxy tab) and IOS with modification. Other platforms are unknown.

```
# platform: Target platform (linux|macos|windows|android|ios|javascript)

scons platform=linux
```

### See all options
```
# Godot custom build options
scons --help

# Scons application options
scons -H
```

## Troubleshooting

### When running scons, I get these errors:

```
Terrain3D$ scons
scons: Reading SConscript files ...

scons: warning: Calling missing SConscript without error is deprecated.
Transition by adding must_exist=False to SConscript calls.
Missing SConscript 'godot-cpp\SConstruct'
File "C:\gd\Terrain3D\SConstruct", line 6, in <module>
AttributeError: 'NoneType' object has no attribute 'Append':
  File "C:\gd\Terrain3D\SConstruct", line 9:
    env.Append(CPPPATH=["src/"])

```

Your godot-cpp directory is probably empty. Review the instructions above for updating the submodule.

### I can build the plugin, however Godot instantly crashes. 
Your godot-cpp version probably does not match your engine version. At this time, they must match. Review the instructions above for switching versions. Test the example project in the next question.

### How can I make sure godot-cpp is the right version and working?
You'll find a test project in `godot-cpp/test/`. Make sure this test project works with your Godot version first, then come back and try Terrain3D again.
  * Build the example plugin by typing `scons` while in the `godot-cpp/test/` directory.
  * Copy `example.gdextension` and `bin` into the root folder of your project.
  * Run Godot. If it crashes, you're on the wrong version, or Godot-cpp has a problem that the maintainers will need to resolve.
  * Create a new scene.
  * Add a new `Example` node. When clicking the node, you should see an `Example` section and `Property From List` and various `Dproperty#` variables in the inspector.
