sudo apt-get update
sudo apt-get upgrade


sudo apt-get install git make autoconf libltdl-dev libusb-dev libexif-dev libpopt-dev libxml2-dev libjpeg-dev libgd-dev gettext autopoint

Once all the packages have finished installing we can proceed to grab the libgphoto2 source code and compiling it. Libgphoto2 is the library that the gphoto2 is built upon.

We can grab the latest available version straight off their Github by running the following command to clone it.

git clone https://github.com/gphoto/libgphoto2.git

4. With the libgphoto2 source code now on our Raspberry Pi, we need to compile it.

cd ~/libgphoto2
autoreconf --install --symlink
./configure
make
sudo make install

5. Now that we have compiled the libgphoto2 library we now need to follow that same process for the actual gphoto2 software.

cd ~
git clone https://github.com/gphoto/gphoto2.git
cd ~/gphoto2
autoreconf --install --symlink
./configure
make
sudo make install

7. With the gphoto2 software now compiled we need to ensure that it can find the library that we compiled in the previous steps.
Open up the file by running the following command on your Raspberry Pi.

sudo nano /etc/ld.so.conf.d/libc.conf

# libc default configuration
/usr/local/lib


8. Now that we have ensured that the “/usr/local/lib” directory is being included we need to refresh the config cache so that the directory will be searched by the operating system when linking libraries.

To do this, we need to run the ldconfig tool by running the command below.

sudo ldconfig

9. In the next few steps, we have to generate the udev rules for the cameras that you might be connecting. Without this, the gphoto2 application may not be able to talk with your camera.

/usr/local/lib/libgphoto2/print-camera-list udev-rules version 201 group plugdev mode 0660 | sudo tee /etc/udev/rules.d/90-libgphoto2.rules

10. Finally, we need to generate the hardware database file for udev.

We can do this by running the following command. This command will automatically create the file in the correct location.

/usr/local/lib/libgphoto2/print-camera-list hwdb | sudo tee /etc/udev/hwdb.d/20-gphoto.hwdb

11. Now run the following command to test that gphoto2 is setup correctly, if it returns the version we are free to continue on with this Raspberry Pi DSLR camera control tutorial.

gphoto2 --version
Using gphoto2 to talk with the Camera
