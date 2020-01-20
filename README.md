source file from GStreamer/gst-plugins-good Mirror [Repo](https://github.com/GStreamer/gst-plugins-good/tree/1.16.2/ext/qt).

## build  
``` bash
# debug version
mkdir debug
cd debug
cmake -GNinja -DCMAKE_BUILD_TYPE=Debug ..
ninja

# release version
mkdir release
cd release
cmake -GNinja -DCMAKE_BUILD_TYPE=Release ..
ninja

# release version with debug info
mkdir release
cd release
cmake -GNinja -DCMAKE_BUILD_TYPE=RelWithDebInfo ..
ninja

# min size release version
mkdir release
cd release
cmake -GNinja -DCMAKE_BUILD_TYPE=MinSizeRel ..
ninja
```
