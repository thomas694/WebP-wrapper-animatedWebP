
# `libwebp.dll`


Downloaded version 1.3.0 from
 * [libwebp-1.3.0.tar.gz](https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-1.3.0.tar.gz)


1. open VS command line tools for x64:
   * tools for x64 are named `x64 Native Tools Command Prompt for VS 2019`  
     e.g. open windows start menu and type "x64"
   * a link for these tools should be located in something like  
     ```
     C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019\Visual Studio Tools\VC
     ```
2. change to extracted libwebp directory
3. run build command
   ```
   nmake /f Makefile.vc CFG=release-dynamic RTLIBCFG=static OBJDIR=output ARCH=x64
   ```
4. open VS command line tools for x86:
   * tools for x86 are named `x86 Native Tools Command Prompt for VS 2019`  
     e.g. open windows start menu and type "x86"
   * a link for these tools should be located in something like  
     ```
     C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019\Visual Studio Tools\VC
     ```
5. change to extracted libwebp directory
6. run build command
   ```
   nmake /f Makefile.vc CFG=release-dynamic RTLIBCFG=static OBJDIR=output ARCH=x86
   ```
 
then copy & rename:
```
<libwebp-dir>\output\release-dynamic\x64\bin\*.dll	->	<here>\WebPWrapperLib\x64\*.dll
<libwebp-dir>\output\release-dynamic\x86\bin\*.dll	->	<here>\WebPWrapperLib\x86\*.dll
```

For more details and instructions for compiling the library see page 
[DevGoogle/webp/docs/compiling](https://developers.google.com/speed/webp/docs/compiling).
