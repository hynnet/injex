# Introduction #

Injex.exe is the injector that injects your specified DLL. It uses the "Jeffrey Richter" method:

  1. **`VirtualAllocEx`** - to create a space inside the target process to contain the path to your DLL.
  1. **`WriteProcessMemory`** - to copy the path of your DLL into the newly created memory.
  1. **`GetProcAddress(GetModuleHandle("kernel32.dll"), "LoadLibraryA")`** - Get the address of `LoadLibrary` (we must not hard code this because of ASLR).
  1. **`CreateRemoteThread`** - Create a thread as the address of LoadLibraryA and pass in a pointer to the DLL path in the target process' memory space.


# Details #

`Usage: injex -d <dllName> < -b <binary name> [-a <arguments>] | -p <Process ID> > [-w <milliseconds>]`
  * **-d**: Specify the DLL to inject.
  * **-b**: Specify a binary to run then inject into.
  * **-a**: Used in conjunction with '-b' to provide command line arguments to the program to inject into.
  * **-p**: Use instead of '-b' to inject into an application that is already running.
  * **-w**: Use with '-b' when running an application to start it suspended and allow `<milliseconds>` for the injected DLL to lay hooks before resuming it.