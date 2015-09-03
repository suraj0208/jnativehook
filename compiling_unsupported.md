## Preamble ##
Official support for the compiler, architecture and/or operating system you wish to target was probably not included do to resource limitations not design limitations.  It is very likely that the code base will compile on your target of choice with very little or no modification.  With that said, I cannot guarantee that it will work or that I will be able to assist you beyond this scope of this article.  If you do run into a problem, please file a bug.  Helpful information about your target as well as patches are greatly appreciated.

## Unsupported Architectures ##

| native.arch | The architecture string to pass to the compiler. |
|:------------|:-------------------------------------------------|
| native.arch.tune | The value passed to to the compiler (-mtune=${native.arch.tune}). |
| native.arch.name | The architecture string used to organize the native library in the jar file. |
| native.arch.model | The architecture model to pass to the compiler and linker (-m${native.arch.model}) |


## Unsupported Compilers ##

The Java compiler has two properties you can adjust.
| java.cc | The type of Java compiler to use.  A complete list of values can be found at http://ant.apache.org/manual/Tasks/javac.html#compilervalues |
|:--------|:------------------------------------------------------------------------------------------------------------------------------------------|
| java.target | The Java class file version to generate.                                                                                                  |


The native compiler is executed as fllows:
`${native.cc} ${native.cc.args} <src_file.c> ${native.cc.args2} <src_file.o>`

| native.cc | The command to run to compile C source files.  The system path variable will be used to resolve the path of the command or an absolute path can be used. |
|:----------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| native.cc.args | The arguments that will be passed to the compiler before the source file.                                                                                |
| native.cc.args2 | The arguments that will be passed to the compiler after the source file but before the output file.                                                      |


The native linker is executed as fllows:
`${native.ld} ${native.ld.args} <obj_file1.o> <obj_file2.o> <obj_fileN.o> ${native.ld.args2} ${native.executable`}

| native.ld | The command to run to link the generated object files.  The system path variable will be used to resolve the path of the command or an absolute path can be used. |
|:----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| native.ld.args | The arguments that will be passed to the linker before the object files.                                                                                          |
| native.ld.args2 | The arguments that will be passed to the linker after all of the object files but before the native output file.                                                  |
| native.executable | The name of the binary output file for the JNI library.                                                                                                           |


## Unsupported Operating Systems ##

| native.os | The organization folder used for native binaries in the jar file.  This is also used for the JNI include subfolder on unix platforms. |
|:----------|:--------------------------------------------------------------------------------------------------------------------------------------|
| dir.src.platform | The folder for the platform compatible source code.                                                                                   |


## Compacting the Jar ##

If you need to reduce the size the library there are a few items that maybe safely removed.  Unfortunately Java does not have a preprocessor so these items will need to be removed manually.

The following classes and packages can safely be removed.
  * `org.jnativehook.example`
  * `org.jnativehook.example.NativeHookDemo`
  * `org.jnativehook.NativeSystem`

You will also need to patch `org.jnativehook.GlobalScreen.loadNativeLibrary()`
```
protected static void loadNativeLibrary() {
    //Try to load the native library assuming the java.library.path was
    //set correctly at launch.
    System.loadLibrary("JNativeHook");
}
```