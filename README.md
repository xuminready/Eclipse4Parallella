# Eclipse4Parallella
Using Eclipse IDE to cross compile for Parallella board

I have 2 Parallella Embedded boards for almost 1 year now. It's an very interesting heterogeneous system. Unfortunately, some development tools are out of date. Recently I was able to use latest Eclipse IDE for C/C++ Developers to cross compile for my parallella. And I realized the Epiphany RISC is easy to program.

My idea is using Eclipse IDE to improve my productivity. I'm going to use [epiphany-examples/apps/hello-world](https://github.com/adapteva/epiphany-examples/tree/2016.11/apps/hello-world) as example here. I hope this is useful for some people. 

## Prepare

You must use a Linux system. Epiphany-ESDK only supports Linux system. I'm using [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/desktop).

### Download tools

1. **[Eclipse IDE for C/C++ Developers](https://www.eclipse.org/downloads/eclipse-packages/)** Or you can install Eclipse CDT(C/C++ Development Tooling) to your existing Eclipse.
Eclipse need [Java RunTime Environment(JRE)](http://www.oracle.com/technetwork/java/javase/downloads/index.html). You can download JRE linux-x64_tar.gz file, Extract it to *eclipse/jre* folder. So you don't need to install anything to your system.

2. **[Linaro Toolchain](https://releases.linaro.org/components/toolchain/binaries/latest/arm-linux-gnueabihf/)** 
This is Linaro Toolchain for cross compiling source code for ARM(Zynq). I'm using the latest one gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf.tar.xz.  If you have xilinx vivado installed. You may not need to download this. And Please notice  Parabuntu 2016.11.1 comes with gcc 4.9.2. The latest gcc 7.2.1 works fine for me now. 

3. **[epiphany-sdk esdk.2016.11.x86_64.tar.gz](https://github.com/adapteva/epiphany-sdk/releases)**
This is Adapteva Toolchain for cross compiling source code for Epiphany on your x86_64 machine.


Download and extract these files.


## E C project

Open your eclipse IDE. Set your workspace. You will create an Eclipse project for Epiphany (Device side)

### 1. Create a C project
File-> New -> Project... We are going to create a C project.

![create a C project](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/1.create_project.png)

### 2. Project name and type
Project Name: **`hello_world_device`**

Toolchains: Cross GCC

Hello World ANSI C Project

![device_project](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/2.device_project.png)

### 3. Basic Setting
You don't need to change anything, just click Next

![setting](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/3.setting.png)

### 4. Configuration

Click Next

![configuration](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/4.configuration.png)

### 5. Configure the Cross GCC
Cross Compilier prefix:**`e-`**

Cross Compile Path: **`/home/minxu/Desktop/esdk.2016.11/tools/e-gnu`**

***Change to your epiphany-sdk esdk Path.***

Click Finish.

![e_cross_gcc](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/5.e_cross_gcc.png)

### 6. link e-lib
Project -> Properties -> C/C++ Build -> Settings -> Tool Settings -> Cross GCC Linker -> Libraries

Add **`e-lib`** to Libraries(-l)

![e-lib](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/6.e-lib.png)

### 7. link LDF file
Project -> Properties -> C/C++ Build -> Settings -> Tool Settings -> Cross GCC Linker -> Miscellaneous

Linker flags: **`-T /home/minxu/Desktop/esdk.2016.11/bsps/current/fast.ldf`**

***Change to your epiphany-sdk esdk Path.***

All the setting is done. Click Apply and Close.

![link LDF](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/7.link_ldf.png)

### 8. Build your source code

Copy and paste source code [e_hello_world.c](https://github.com/adapteva/epiphany-examples/blob/2016.11/apps/hello-world/src/e_hello_world.c)

Save your file. Click Build button(mouse cursor position in the screenshot below)

If no error output in the CDT Build Console, you just built an executable file *hello_world_device* in Debug folder.  This is the `e_hello_world.elf` file. You can rename it if you like. (Notice: Linux does not need .elf to indicate it's executable file)

![build](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/8.build.png)


## ARM C project

You will create an Eclipse project for ARM(Zynq) (Host side)

### 1. Create a C project
File-> New -> Project... We are going to create a C project.

![create a C project](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/1.create_project.png)

### 2. Project name and type
Project Name: **`hello_world_host`**

Toolchains: Cross GCC

Hello World ANSI C Project

![device_project](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/10.create_project.png)

### 3. Basic Setting
You don't need to change anything, just click Next


### 4. Configuration

Click Next


### 5. Configure the Cross GCC
Cross Compilier prefix:**`arm-linux-gnueabihf-`**

Cross Compile Path: **`/home/minxu/Desktop/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf`**

***Change to your epiphany-sdk esdk Path.***

Click Finish.

![e_cross_gcc](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/11.arm_cross_gcc.png)

### 6. include Path
Project -> Properties -> C/C++ Build -> Settings -> Tool Settings -> Cross GCC Compiler -> Includes

Add **`/home/minxu/Desktop/esdk.2016.11/tools/host/include`** to Include paths(-I)

***Change to your epiphany-sdk esdk Path.***

![e-lib](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/12.include_path.png)

### 7. link e-lib
Project -> Properties -> C/C++ Build -> Settings -> Tool Settings -> Cross GCC Linker -> Libraries

Add to Libraries(-l)
```
pal
e-hal
e-loader
pthread
```
Library search path (-L): **`/home/minxu/Desktop/esdk.2016.11/tools/host.armv7l/lib`**

***Change to your epiphany-sdk esdk Path.***

![e-lib](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/13.libs.png)


### 8. Build your source code

Copy and paste source code [hello_world.c](https://github.com/adapteva/epiphany-examples/blob/2016.11/apps/hello-world/src/hello_world.c)

***If you rename your `hello_world_device` to `e_hello_world.elf` in E C project, you don't need to modified the e_load file name.***
*modified e_load file name*
```C
if ( E_OK != e_load("e_hello_world.elf", &dev, 0, 0, E_TRUE) ) {
```
to your executable file *hello_world_device*
```C
if ( E_OK != e_load("hello_world_device", &dev, 0, 0, E_TRUE) ) {
```

Save your file. Click Build button(mouse cursor position in the screenshot below)

If no error output in the CDT Build Console, you finish building an executable file *hello_world_host* in Debug folder. 

![build](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/14.build.png)

## Test it on your parallella board
Open your workdspace path, find your project folder *hello_world_device* and *hello_world_host*, inside these folders,

scp/copy **Debug/hello_world_device** and **Debug/hello_world_host** to the same folder to your parallella SD card.

Power on your parallella board,

Type **'./hello_world_host'** in your terminal/ssh terminal

![build](https://github.com/xuminready/Eclipse4Parallella/raw/master/screenshots/15.run_results.png)
