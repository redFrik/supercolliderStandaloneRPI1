#notes

Instructions for building these binaries and set up a similar standalone repository...

First build and install SuperCollider on a RPi1 (or RPi0) following the instructions in the ScIDE section [here](http://supercollider.github.io/development/building-raspberrypi.html). One can also use an existing sc install assuming all the files are in their default directories.

* `mkdir supercolliderStandaloneRPI1 && cd supercolliderStandaloneRPI1`
* `cp /usr/local/bin/sc* .` #this will copy scide, sclang and scsynth
* `cp -r /usr/local/lib/SuperCollider/plugins .` #copies all plugins
* `mkdir -p share/system`
* `cp -r /usr/local/share/SuperCollider/* share/system` #copies help, classes, examples etc
* `mkdir -p share/system/Extensions/SystemOverwrites`
* `curl -o share/system/Extensions/SystemOverwrites/extLinuxPlatform.sc https://raw.githubusercontent.com/redFrik/supercolliderStandaloneRPI1/master/share/system/Extensions/SystemOverwrites/extLinuxPlatform.sc`
* `mkdir share/user`
* `curl -o share/user/archive.sctxar https://raw.githubusercontent.com/redFrik/supercolliderStandaloneRPI1/master/share/user/archive.sctxar`
* `curl -o sclang.yaml https://raw.githubusercontent.com/redFrik/supercolliderStandaloneRPI1/master/sclang.yaml`
* `curl -o autostart.sh https://raw.githubusercontent.com/redFrik/supercolliderStandaloneRPI1/master/autostart.sh`
* `curl -o run.sh https://raw.githubusercontent.com/redFrik/supercolliderStandaloneRPI1/master/run.sh`
* `curl -o share/system/sc_ide_32.png https://raw.githubusercontent.com/redFrik/supercolliderStandaloneRPI1/master/share/system/sc_ide_32.png`

Now this directory should contain what is needed to run ScIDE standalone (if started as in the README.md). Copy it to another machine with the same system and try.

sc3-plugins
--

How to build and include sc3-plugins...

* `git clone --recursive https://github.com/supercollider/sc3-plugins.git --depth 1`
* `cd sc3-plugins`
* `mkdir build && cd build`
* `export CC=/usr/bin/gcc-4.8`
* `export CXX=/usr/bin/g++-4.8`
* `cmake -L -DCMAKE_BUILD_TYPE="Release" -DCMAKE_C_FLAGS="-mfpu=vfp -mfloat-abi=hard -march=armv6 -mtune=arm1176jzf-s" -DCMAKE_CXX_FLAGS="-mfpu=vfp -mfloat-abi=hard -march=armv6 -mtune=arm1176jzf-s" -DSC_PATH=../../supercollider/ -DCMAKE_INSTALL_PREFIX=~/supercolliderStandaloneRPI1/share/system/Extensions/SC3plugins ..`
* `make`
* `sudo make install`
* `cd ~/supercolliderStandaloneRPI1/share/system/Extensions/`
* `sudo chown -R pi SC3plugins`
* `sudo chgrp -R pi SC3plugins`
* `mkdir SC3plugins/bin`
* `mv SC3plugins/lib/SuperCollider/plugins/*.so SC3plugins/bin/`
* `mv SC3plugins/share/SuperCollider/Extensions/SC3plugins/* SC3plugins/`
* `rm -rf SC3plugins/lib`
* `rm -rf SC3plugins/share`
* `rm -rf SC3plugins/local`

publish
--

My own additional notes for this git repository...

* note which supercollider master commit was used in README.md
* note which raspbian image was used in README.md
* copy the files over to laptop...
  * `cd supercolliderStandaloneRPI1`
  * `scp pi@raspberrypi.local:supercolliderStandaloneRPI1/sc* .`
  * `rm -rf plugins`
  * `scp -r pi@raspberrypi.local:supercolliderStandaloneRPI1/plugins .`
  * `rm -rf share`
  * `scp -r pi@raspberrypi.local:supercolliderStandaloneRPI1/share .`
  * and possibly the yaml and desktop files as well if something changed
* git commit and sync
