# TimeHub

## Prepareing

A long time ago i started with my first C++ projekt as part of a cource in the univerity. After i finished this course i never touched C++ again, because in the following course we used different languages like Java, Python or something else. Even after I had successfully completed my masters degree in business informatics, i stopped working with C++. In my opinion C++ to be a very interesting language, even if it has more hurdles and difficulties than other languages, i would like to use this small project to take another step in the direction of C++. That is the reason why i want to documemt the necessary steps to setup everything you need to work with C++.

First of all we have to make some decision with which software parts we want to work. We need a IDE, compiler and builder. As IDE i choose <b>Visual Studio Code</b>, because it is free to use, easy to use and their is a big community to get questions answered. The compiler i choose was <b>Clang</b> based on the criteria that i want something which is used in the modern practice and for the builder i chosse based on the sam criteria <b>CMake</b>. Now i will go threw the steps how i installed and setup everything.

### Visual Studio Code

Visual Studio Code is a free IDE and can be used for different programming languages and got a huge amount of different plugins to make it easer to work with languages or technologies. For example their are plugins for each programming language which can be used for the text highlighting or for docker to see the builded images or available containers. To get Visual Studio Code you can go to the offical website of Visual Studio Code and download it for your specific operating system:

https://code.visualstudio.com/

You find a 'Download' button in the upper right corner and after you downloaded it you can follow the wizard. In my case i downloaded it for windows.

### Generate machine code with CLang

We write our programs in human readable code (also called Source Code) but these can not be interpreted by the computer. The mother tounge of a computer is binary code or binary instructions which contains only 0 and 1. These binary code is also called machine language or machine code. These kinds of code are highly optimized to be executed on the hardware we use and it it not possible to execute the source code directly on the system, because it abract code and these constructs can not be interpreted by the computer. That is the reason why we need a compiler.

I choose Clang as compiler but their are also other possible ones which can be used like GNU (also called G++ or GNU Compiler Collection), MSVC (Microsoft Visual C++) or Intel C++ Compiler (ICC/ICX). There are also niche-specific/experimental compilers like TCC (Tiny C Compiler) and Herb Sutter’s cppfront or patform specific compilers like MinGW (Minimalist GNU for Windows) and ARM Compiler.

To use Clang on Winows the first step is to visit the offical LLVM website () and download the windows version of it https://llvm.org/.

After you downloaded it you can follow the installation steps of the wizard. It is important to look for the option <b>'Add LLVM to the system PATH for all users'</b> while installing it. The reason why have to check this option is, that then path to Clang is added to the system enviroment variables what is necessary for the use. If you forgot to check this box it is also possible to add this path later directly to the system enviroment variables.

```bash
clang --version
```

The output of this should look like this:

```text
clang version 19.1.0
Target: x86_64-pc-windows-msvc
Thread model: posix
InstalledDir: C:\Program Files\LLVM\bin
```

*in the output you will see at the <i>Target</i> tag which adress the msvc (Microsofrt Visual C++), the reason for that is tha tthe MSVC is used by Clang to compile code that runs on Windows with MSVC-compatible binary formats and libraries. If you use linux as operating system, your output will look slightly different like <i>x86_64-pc-linux-gnu</i>, bcause Clang is using another toolchain on your platform.

## Get Qt running

The visualization is build with the Qt libary. Because Qt has a dependency to the compiler, it is important to know which compiler is installed on the system and should be use. In the following are guidelines for some compilers and operating system combinations.

### Clang

In this section will be describe how to use Clang as Compiler for the Qt libary. The instructions will be shown for the linux and windows os.

#### Windows

1. Check if Clang is installed correctly. Use the following command to check that:

```bash
clang --version
```
2. CMake should be installed and part of the system enviroment variables
3. Clone qt6 via git (qt6 is part of qt5) outside of the project:

```bash
git clone --branch v6.8.1 --depth 1 https://github.com/qt/qt5.git
cd qt5
./init-repository
```
4. Create a subfolder in the qt5 folder called <i>build</i> and enter it
```bash
mkdir build
cd build
```
5. Install Ninja as build-tool on Windows using the powershell as admin

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco install ninja

ninja --version
```

5. Configure CMake to build it with Ninja for Clang (a lot of warning can occur, but don´t worry while the process is still running)
```bash
cmake .. -G "Ninja" -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_BUILD_TYPE=Release
```
6. Restart bash window
6. Use Ninja, CMake or another build tool to start the build process

```bash
cmake --build . --config Release
```