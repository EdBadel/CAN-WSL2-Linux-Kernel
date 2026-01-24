# WSL Kernel for CAN development
This repo is forked from [WSL2-Linux-Kernel](https://github.com/microsoft/WSL2-Linux-Kernel), and includes an action that compiles the
kernel with many drivers necessary for the development of can-itnerfacing software on WSL.

# Download & Install
1. Visit the [Actions](https://github.com/EdBadel/CAN-WSL2-Linux-Kernel/actions) page of this repo
2. Click on the latest monthly build that succeeded
3. Find `Artifacts` > `wsl2-kernel-[version]-WSL2-can+` and download it
4. Extract the archive somewhere nice on your desktop (`C:\Users\yourname\wsl2-kernel-can`).
   > This should give you a `bzImage` file and a `modules.tar.gz` archive. The path to both those files is
   > important for later steps.
6. Create a `.wslconfig` (no extension) text file in your windows user folder `C:\Users\yourname\`
7. Point windows to the kerenl `bzImage` file you just downloaded by editing `.wslconfig` with the following:
```
[wsl2]
kernel=C:\\Users\\yourname\\wsl2-kernel-can\\bzImage
```
8. Open an admin command prompt on windows and run
```
wsl --shutdown
```
9. Launch wsl and verify you have the CAN kernel installed by running `uname -r` in a wsl terminal. You should get `6.6.87.2-WSL2-can+` or similar
10. Extract the modules somewhere safe in wsl:
```
cd ~
mkdir can-temp-modules
tar -xzf /mnt/c/Users/yourname/wsl2-kernel-can/modules.tar.gz ./can-temp-modules
```
> This should create `~/can-temp-modules/lib/modules/6.6.87.2-WSL2-can+`
11. Copy the new modules folder into your user modules folder
```
sudo cp -r ~/can-temp-modules/lib/modules/6.6.87.2-WSL2-can+ /usr/lib/modules
```
12. Restart WSL with `wsl --shutdown`
13. Verify you have the new modules. Running `sudo modprobe vcan` should result in no output.
> If you have an error similar to `modprobe: FATAL: Module vcan not found in directory /lib/modules/...` you likely haven't installed the modules folder in the correct place
14. Delete the temp dir you extracted the modules in: `rm -rf ~/can-temp-modules`
