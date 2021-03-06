ipk-builder is a libary that creates IPK packages for either the [ipkg](https://en.wikipedia.org/wiki/Ipkg) or [opkg](https://en.wikipedia.org/wiki/Opkg) package management systems. It may also be useful for generating simple packages for the [dpkg](https://en.wikipedia.org/wiki/Dpkg) system.

# Requirements 

This library assumes a *nix system with tar, gzip and fakeroot installed. These should be standard on most GNU/Linux systems.

# Example usage:

    var IPKBuilder = require('ipk-builder');
    var builder = IPKBuilder();
    builder.setBasePath('./foo'); // file paths in ipk will be relative to the base path
    builder.addFiles('./foo/bin', './foo/var');
    builder.addConfFiles('./foo/etc/foo.conf');
    builder.addPostScripts('myscript.sh');
    builder.setMeta({  
      package: "foo",
      version: "0.1",
      maintainer: "Foo Bar <foobar@example.com>",
      architecture: "ar71xx",
      description:  "Foo is a package for stuff and things.\n It is very convenient."
    });
    builder.build('foo-0.1.ipk');

# Functions

## IPKBuilder(opts)

Creates an ipk builder object. Valid opts:

```
  ignoreMissing: Ignore missing files and directories if set to true (default: false).
```

## setBasePath(path)

Set the base path. File paths in the ipk will be relative to this path. As an example, if you want to package the file

    /home/fungi/myproject/usr/bin/foo

such that the file "foo" appears in /usr/bin/foo when the package is installed, then the base path should be set to:

    /home/fungi/myproject

The base path can be changed between subsequent calls to addFiles/addConfFiles. If unset, the base path will default to the current working directory.

## addFiles(path_to_file_or_dir1, path_to_file_or_dir2, ...)

Add files or directories to package. Note that configuration files should be added with addConfFiles instead.

## addConfFiles(path_to_file1, path_to_file2, ...)

Add configuration files to package. You can also add configuration files using addFiles but the config files are treated differently e.g: When a package is upgraded and its configuration file has been modified by the user, the user is asked which version of the configuration file to use. 

## addPostScripts(path_to_file1, path_to_file2, ...)

Add scripts to be run at the end of the package installation. If multiple scripts are specified these will all be combined into a single post script.

## setMeta(obj)

Example of a meta object:

    {  
      package: "foo",
      version: "0.1",
      depends: "libc, libncurses",
      provides: "foobar",
      source: "feeds/packages/libs/avahi",
      section: "net"
      status: "unknown ok not-installed",
      essential: "no",
      priority: "optional",
      maintainer: "Foo Bar <foobar@example.com>",
      architecture: "ar71xx",
      installed-Size: "2048",
      homepage: "https://foo.example.com",
      description:  "Foo is a package for stuff and things.\n It is very convenient."
    }

Note that only the following fields are mandatory:

*package
*version
*maintainer
*architecture
*description

A description of the fields is available [here](https://www.debian.org/doc/debian-policy/ch-controlfields.html).

## setControl(control)

Alias for setMeta

## build(output_filepath) 

Build the ipk file and write it to output_path.

# License

Licensed under GPLv3.

Copyright 2014 Marc Juul
