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

## Making plugins (short guide)

You can pack your own plugin (or plugin with your modifications) by this command: `nativelib --pack <path to plugin root>`

This will pack and copy plugin files into your local repository. So after that you can install this new plugin by regular way.

## Making plugins (longer guide)

Coming soon.
