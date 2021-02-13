# NativeLib command line interface

NativeLib is a plugin management system for [Godot engine](http://godotengine.org/). It designed to easy operate with native libraries for iOS/Android. Also it takes care about plugin dependencies and provides hasslefree native libs updating.

If you prefer GUI instead of command line, consider using [NativeLib Addon](https://github.com/DrMoriarty/nativelib).

# Update notice

After upgrade from version 0.1.x you should reload all repository info. Just make this command `nativelib -U` and wait a couple of minutes.

## Installation (easy way)

Just do it: 

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/DrMoriarty/nativelib-cli/HEAD/install.sh)"
```

## Installation (common way)

1. Download this repository as ZIP file and unpack it. 
2. Move `nativelib` into some folder which is in your `$PATH`.
3. Enjoy!

## Requirements

You should have python3 in your system.

## Getting started

Go to your project directory and setup nativelib. You should provide parameters for mobile platforms you are using.

For iOS do this command: `nativelib --ios`

For Android: `nativelib --android`

Or this if you are using both: `nativelib --ios --android`

## Remote repository

Before using NativeLib you should fetch information from the remote repository: 

```nativelib --update```

NativeLib won't check new packages and new versions until you again do `nativelib --update`.

It's time for search something useful. Do `nativelib --search facebook` in order to find all packages related to Facebook.
You will get:
```
facebook-plugin@0.1.0
```

This means that full package name is `facebook-plugin` and it's latest version is `0.1.0`.

## Installing packages

Then let's go into your project folder and do `nativelib -i facebook-plugin`. The latest version of this package will be installed in your project. Usually packages are splitted into several parts, one part for each mobile platform and one part for universal code. NativeLib installs only such platforms which you are inited before.

Now let's see what installed in our project:

```
bash-3.2$ nativelib --list
Default platforms: all, ios, android

facebook-plugin@0.1.0
  platforms: all, ios, android

nativelib-export-plugin@0.1.0
  platforms: all
```

You can see `nativelib-export-plugin` on which depends `facebook-plugin` and installed platforms for every package and defaults for project.

## Project maintenance

NativeLib plugins usually contains binary libraries and frameworks and they can be very large. Sometimes `.gitignore` files are added in plugins in order to prevent uploading such libraries into your git repository.

When you clone your repository into another machine you should restore all your installed plugins. You can do this by:

```
nativelib --prepare
```

It will install all plugins with the same versions into your new cloned repo. 

Sometimes you can find yourself in situation when some of your plugins was broken and compilation (for example) fails. You can reinstall plugin and all it's dependencies using force: `nativelib --force --overwrite --install <package_name>`. It also overwrites all plugin files so if they was modified it will return all in initial state.

## Making plugins

You can use [any of existing plugins](https://github.com/topics/nativelib) as example to making your own. The plugin must have meta file usually named `nativelib.json`. Also it may have gd scripts and other binary files.

The first thing you should do after any modifications is meta package validation. Do this in plugin's folder:

```
nativelib --validate .
```
Packing and publishing will be blocked until you fixes all found errors. Still you could make a release with warnings.

The next step is pack your plugin. Run this command: `nativelib --pack <path to plugin root>` (it will always validate plugin meta in order to ensure that it is correct). This command will produce archives in your local repository at `~/.nativelib/packages/<package_name>/<package_version>/`. You can check them if they contain all needed resources.

After that you can install this new plugin into your project and check it in real work.

When your plugin is ready you can make it available to all other NativeLib users. In order to upload your binary files you should have [bintray](https://bintray.com) account or use your GitHub repository for binary releases.

### Publishing at GitHub

If you prefer to use GitHub you should:
- install [GitHub-CLI](https://cli.github.com)
- login to your GitHub account (gh auth login)
- ensure that you already pushed your plugin repository into GitHub

After that run command: `nativelib --github --publish <path to plugin>`. It will again validate your meta, then it will repack you archives, then it will make new release in you repository and upload binary files. At last, it will upload package meta to [Godot Asset Index](https://github.com/godot-asset/index).

### Publishing at Bintray

*WARNING: It is not recommended because Bintray will be deprecated soon (https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/)*

- Register at [Bintray](https://bintray.com) if you didn't registered yet
- Create new Generic repository
- Make Api Key
- Create file `.nativelib/local.properties` with this content:
```
bintray.subject=<organisation name>
bintray.repo=<repository name>
bintray.user=<username>
bintray.apikey=<api key>
```

After that you will be able to run `nativelib --bintray --publish <path to plugin>`. It will make new release and upload binary files. Also it will upload meta to Godot Asset Index.

## Godot Asset Index

The [Godot Asset Index](https://github.com/godot-asset/index) is designed as universal storage of any godot's plugins and assets meta data. Since NativeLib 0.3.0 it used as primary packages index.

When you first time publish your plugin the file `~/.nativelib/publisher.key` will be created. This is your personal publisher ID, keep it in a safe place. Only you (with your ID) can make new releases for this plugin. If you acidentaly use plugin name which is used by somebody then you can not publish your package meta. You can see all registered plugin names at https://github.com/godot-asset/index/tree/master/meta

It can be wise to use plugin name as `<your-special-prefix>.<plugin-name>`. But escape using `_` in plugin name! Using it can break plugin packaging and installation.
