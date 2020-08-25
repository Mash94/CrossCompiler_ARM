last update: 25/08/2020

# CrossCompiler_ARM
A small tutorial about how to install and use crosscompilation from x86 to arm

Source: [bbx - Documentacion](http://wiki.electron.frba.utn.edu.ar/doku.php?id=td3:bbx)

## Instalacion del cross compiler
Como cross-compiler se utiliza GCC Linaro. De esa página, acceder al link: [Binaries](https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/) en Support/Downloads (utilizamos arquitectura ARM-v7), descargar la última versión disponible y descomprimir:

```
cd 
mkdir CrossCompilerARM
cd CrossCompilerARM
wget https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/gcc-linaro-X.X.X-YYYY.MM-x86_64_arm-linux-gnueabihf.tar.xz
tar xf gcc-linaro-X.X.X-YYYY.MM-x86_64_arm-linux-gnueabihf.tar.xz
```
Dónde, por ejemplo:

X.X.X = 7.5.0,
YYYY.MM = 2019.12
Para utilizar el nuevo toolchain, se disponen de dos opciones a elección del desarrollador

1-Temporal
De esta manera  el crosscompiler funciona en la terminal hasta que esta se cierre.
```
export PATH=$PATH:/home/<su usuario>/CrossCompilerARM/gcc-linaro-X.X.X-YYYY.MM-x86_64_arm-linux-gnueabihf/bin/
```

```
user@machine://home/<su usuario>/CrossCompilerARM/$ arm-linux-gnueabihf-gcc --version
arm-linux-gnueabihf-gcc (Linaro GCC X.X-YYYY.MM) X.X.X YYYYMMDD [linaro-X.X.X-YYYY.MM revision 7b15d0869c096fe39603ad63dc19ab7cf035eb70]
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
``` 

2-Persistente para todos los usuarios
```
su
cd /etc
nano bash.bashrc
```
Moverse hacia el final del archivo y agregar
```
export PATH=$PATH:/home/<su usuario>/CrossCompilerARM/gcc-linaro-X.X.X-YYYY.MM-x86_64_arm-linux-gnueabihf/bin/
```
Ctrl+O, Ctrl+X para salir y reiniciar el terminal
```
arm-linux-gnueabihf-gcc --version
arm-linux-gnueabihf-gcc (Linaro GCC X.X-YYYY.MM) X.X.X YYYYMMDD [linaro-X.X.X-YYYY.MM revision 7b15d0869c096fe39603ad63dc19ab7cf035eb70]
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
``` 

## Proceso de Compilacion
```makefile
exec=server
func=funciones
arm:
 arm-linux-gnueabihf-gcc -c src/$(exec).c -o obj_arm/$(exec).o
 arm-linux-gnueabihf-gcc -E src/$(exec).c -o sup_arm/$(exec).i
 arm-linux-gnueabihf-gcc -c src/$(func).c -o obj_arm/$(func).o
 arm-linux-gnueabihf-gcc -E src/$(func).c -o sup_arm/$(func).i
 arm-linux-gnueabihf-gcc obj_arm/$(exec).o obj_arm/$(func).o -o $(exec)_arm	
```
## SSH a BeagleBone
passwd: temppwd
```
ssh ubuntu@arm.local
```

## Envio del ejecutable hacia el host
Sobre el arm:
```
cd
mkdir remoteRcv
```
Sobre el host:
```
scp server_arm ubuntu@arm.local:~/remoteRcv
``` 