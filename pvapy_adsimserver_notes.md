## **pvapy adSimServer.py enhancement**

Added ability to read in additional file formats (original read hdf5 and numpy files). Uses fabio. Can read ge5, tiff, cbf, etc. 

  - In /home/beams/ABREWE/usr/pvaPy/pvapy/cli
  - modified to read in files (tiff, cbf tested, some GE files give incorrect size error) with fabio and streams frames to given channel name. 
     Ignores files it cannot read. Uses fabio. 
  - loads single image tiff, cbf, and ge files
  - adSimServer publishes at different max frame rates depending on image/file size. 
  - number of files can be changed by specifying -nf
  - -aff must be set to larger than 0 to specify that the files read in are not hdf5 or numpy files.
  - -cfg is for yaml configuration files for reading in raw binary files (need to specify format of the file to be able to read it in).

```shell
cd /home/beams/ABREWE/usr/pvaPy/pvapy/cli

#reads in cbf files
/home/beams/ABREWE/miniconda3/envs/simEnv/bin/python ./adSimServer.py -id /home/beams1/AMICELI/GMCA-Eiger-Pilatus-Data/eiger_M13/ -cn abrewe:image:adsim -aff=1 -fps=10 -nf=50

#random
/home/beams/ABREWE/miniconda3/envs/simEnv/bin/python ./adSimServer.py -cn abrewe:image:adsim -fps=10

#raw binary files 
/home/beams/ABREWE/miniconda3/envs/PVAPY_NEW/bin/python ./adSimServer.py -id /net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/ -cn abrewe:image:adsim -aff=1 -fps=10 -cfg=full_scan.yaml -rt=400
```

Need a yaml file if you want to read in raw binary data. 
Example yaml file - readsin a dark before, scan, and dark after:

```yaml
raw_bin_file:
  header_offset: 8192
  between_frames: 0
  width: 1475
  height: 1679
  datatype: int32
  ordered_files:
    - "/net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/dark_before_000379.raw"
    - "/net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/ffcontTi64APS2_000380.raw"
    - "/net/s1data/export/s1a/test_abrewe/pagan_feb23_pilatus/dark_after_000381.raw"

```
if you don't need to specify file order, ie only one binary file or order is already correct, set ordered_files to null. Number of images per file is calculated to allowed for different sized raw files. 
