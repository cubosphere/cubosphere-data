# Cubosphere [reborn] — data repository
This repo contain data files for cubosphere. Pretty useless without code.

## How build system works?
This `CMakeLists.txt` is incuded from code repo one. Then:

* All `raw` folder content will be installed to `DATADIR` as is
* Data in `zip` folder will be zipped to `native_data.zip` which will be installed as `data000.zip` to `DATADIR`

This is done for easier data editing.
