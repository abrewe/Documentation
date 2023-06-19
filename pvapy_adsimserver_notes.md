## **pvapy adSimServer.py enhancement**

Added ability to read in additional file formats (original read hdf5 and numpy files). Uses fabio. Can read ge5, tiff, cbf, raw binary files, etc. 

  - In /home/beams/ABREWE/usr/pvaPy/pvapy/cli
  - modified to read in files (tiff, cbf, ge, raw tested) with fabio and streams frames to given channel name. 
  - Ignores files it cannot read. Uses fabio. 
  - loads single image tiff, cbf, ge files, and multi image raw files.
  - adSimServer publishes at different max frame rates depending on image/file size. 
  - number of files can be changed by specifying -nf (not for raw files)
  - -aff must be set to larger than 0 to specify that the files read in are not hdf5 or numpy files.
  - -cfg is for yaml configuration file for reading in raw binary files (need to specify format of the file to be able to read it in).

```shell
cd /home/beams/ABREWE/usr/pvaPy/pvapy/cli

#reads in cbf files
/home/beams/ABREWE/miniconda3/envs/simEnv/bin/python ./adSimServer.py -id /home/beams1/AMICELI/GMCA-Eiger-Pilatus-Data/eiger_M13/ -cn abrewe:image:adsim -aff=1 -fps=10 -nf=50

#random
/home/beams/ABREWE/miniconda3/envs/simEnv/bin/python ./adSimServer.py -cn abrewe:image:adsim -fps=10

#raw binary files 
/home/beams/ABREWE/miniconda3/envs/PVAPY_NEW/bin/python ./adSimServer.py -id /net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/ -cn abrewe:image:adsim -aff=1 -fps=10 -cfg=full_scan.yaml -rt=400
```

Need a yaml file to read in raw binary data. Give header_offset in bytes, width and height in pixels. 
Example yaml file - reads in 3 binary files in the specified order:

```yaml
file_info:
  header_offset: 8192
  width: 1475
  height: 1679
  datatype: int32
  endian: "<"
  ordered_files:
    - "/net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/dark_before_000379.raw"
    - "/net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/ffcontTi64APS2_000380.raw"
    - "/net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/dark_after_000381.raw"

```

If you don't need to specify file order, ie only one binary file, order already in correct loading sequence in the input directory, set ordered_files to null. Number of images per file is calculated with header and width/height to allow for loading of multiple different sized raw files.

"Loading . . ." message added while loading binary files, because there may be a delay while loading large binary files. (how long of a delay depends on the size of files).

FabIO, yaml, optional (necessary for new file formats, but can use hdf5 and numpy functionality without them). 