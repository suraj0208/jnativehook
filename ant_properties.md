# Introduction #
Several build options are supported though the use of Ant properties.  To automatically generate default properties for your platform run the ant configure target.  Properties maybe overridden by passing a `-D<property>=<value>` option directly to ant or by modifying the build.properties file.  The command line arguments will take precedence over the build.properties file and maybe used to configure the project in various ways.

# Project Options #
The following properties enable/disable various project and platform specific options.

| **Property**| **Type** | **Description** |
|:------------|:---------|:----------------|
| project.debug | bool     | Configure the project with debugging options. `(Default: false)` |
| project.optimize | bool     | Whether to optimize the build. `(Default: true)` |
| project.strip | bool     | Whether to strip symbols from the binary. `(Default: true)` |
| project.option.corefoundation | bool     | Flag to enabled the `CoreFoundation` framework on OS X. `(Default: true)` |
| project.option.iokit | bool     | Flag to enabled the `IOKit` framework on OS X. `(Default: true)` |
| project.option.carbon\_legacy | bool     | Flag to enabled legacy functionality in the `Carbon` framework on OS X. This flag is required for OS X version 10.4 and should only be used on 32-bit targets. `(Default: false)` |
| project.option.xkb | bool     | Flag to enabled the X keyboard extension. `(Default: true)` |
| project.option.xt | bool     | Flag to enabled the Xt library. `(Default: true)` |
| project.option.xf86misc | bool     | Flag to enabled the xf86misc library. `(Default: false)` |
| project.option.xrecord\_async | bool     | Flag to enabled asynchronous XRecord API. **Prior to 1.1.4, disabling this option may cause XRecord to crash.** `(Default: false)` |


# Java Options #
Java offers a few compiler options.  The only offically support `java.cc` option is modern.

| **Property**| **Type** | **Description** |
|:------------|:---------|:----------------|
| java.cc     | string   | Java compiler to use. `(classic|modern|jikes|jvc|kjc|gcj|sj) (Default: modern)` |
| java.target | string   | Java byte-code version to target. `(1.5|1.6|1.7|<X.Y>) (Default: 1.5)` |
| java.include | string   | Path to the JDK include folder for your platform. |


# Native Target Options #

| **Property**| **Type** | **Description** |
|:------------|:---------|:----------------|
| native.path | string   | Your environments path variable. |
| native.arch | string   | Native arch to target.  `(x86|x86-64|ppc|ppc64|<ia64>|<sparc>|<arm>)` |
| native.arch.model | string   | Target operating system architecture model. `(32|64)` |
| native.os   | string   | Operating system name. `(windows|linux|osx|<solaris>|<freebsd>)` |


# Native Compiler Options #
` ${native.cc} ${native.cc.args} <src_file.c> ${native.cc.args2} <src_file.o>`

| **Property**| **Type** | **Description** |
|:------------|:---------|:----------------|
| native.cc   | string   | Name or full path of the native compiler to use. |
| native.cc.args | string   | Primary set of arguments passed to the compiler before the source files. `(Default: -c -m${native.arch.model}  ${native.cc.flags} ${native.cc.flags.pic} -Wall -Wextra ${native.cc.flags.standard} -pthread)` |
| native.cc.args2 | string   | Secondary set of arguments passed to the compiler after the source files but before the output file. `(Default: -o)` |
| native.cc.flags | string   | Compiler optimization flags. |
| native.cc.flags.standard | string   | Compiler C standard flags. |
| native.cc.flags.pic | string   | Compiler file to enable position independent code. |
| native.cc.includes | string   | Header include locations to pass to the compiler. |


# Native Linker Options #
` ${native.ld} ${native.ld.args} <src_file1.o> <src_file2.o> <src_fileN.o> ${native.ld.args2} ${native.executable`}

| **Property**| **Type** | **Description** |
|:------------|:---------|:----------------|
| native.ld   | string   | Name or full path of the native linker to use. |
| native.ld.args | string   | Primary set of arguments passed to the linker before the object files. `(Default: -shared -m${native.arch.model} ${native.ld.flags})` |
| native.ld.args2 | string   | Secondary set of arguments passed to the linker after the object files but before the native executable. `(Default: ${native.ld.libs} -o)` |
| native.ld.flags | string   | Linker optimization flags. |
| native.ld.args.libs | string   | List of shared libraries to link against. |
| native.executable | string   | The name of the binary library to create. |