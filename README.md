# Little help to reconstruct data

This document describes how to use a really basic pipeline for SuperNEMO data reconstruction from UDD to PCD and Root.

---

## Basic use of the code

The code is divided in several Falaise module (lots of them from other people in the collaboration) and can be launched using the following command (the git clone should only be done the first time): 

```bash
$> git clone https://github.com/Maathiiss/Pipeline-UDD_to_ROOT-SN
$> source /sps/nemo/scratch/chauveau/software/falaise/develop/this_falaise.sh
$> flreconstruct -p UDD_to_root_data.conf -i UDD.brio 
```

It will produce a Root file called `extracted_data.root` in which a TTree will contain all the information you want.

---

## Understand each module

The pipeline is divided into 5 different modules:

- `udd2pcd`
- `pcd2cd`
- `TKReconstruct` → from CD to TTD bank
- `ChargedParticleTracker` → from TTD to PTD bank
- `Module0` → from PTD to root

Let's start with the two modules `udd2pcd` and `pcd2cd`:

These two modules were written by Manu. This code is used to switch from the UDD bank to the CD bank (files are here: `/sps/nemo/snemo/snemo_data/reco_data/UDD/delta-tdc-10us-v3/snemo_run-XXXX_udd.brio`).

### `udd2pcd`

Available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/udd2pcd_module.cc#L35).  
Pre-calibration, such as z calculation.

### `pcd2cd`

Uses 2 files for the calibrations:

- Time calibration file → file produced by Manu for now, stored in my scratch directory. Easily replaceable.
- Energy calibration file → file produced by me using only the "a" parameter in the calibration, stored in my scratch directory. Easily replaceable.

The code is available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/pcd2cd_module.cc)

### `TKReconstruct`

Clustering of Geiger cells and trajectory reconstruction.  
The code is available [here](https://github.com/TomasKrizak/CimrmanModule.git)

Change the `Falaise_TKReconstruct.directory` entry in your `UDD_to_root_data.conf` if you used your own version of Cimmerman.

### `ChargedParticleTracker`

Particles trajectory extrapolation on the foil and calorimeter to identify particle's nature.

### `Module0`

Extracts PTD to root files. It will create an `extracted_data.root` root file in the current directory.

A simpler version of this code is available [here](https://github.com/Maathiiss/Easy_ptd_to_root.git), but you can find the original one [here](https://github.com/emchauve/FalaiseSkeletonModules.git).

You can easily modify this one to get more branches in the tree, using the PTD extraction in this [directory](https://github.com/SuperNEMO-DBD/Falaise/tree/develop/source/falaise/snemo/datamodels) or some examples in the [flvisualize GitHub](https://github.com/emchauve/Falaise/blob/develop/programs/flvisualize/EventBrowser/browser_tracks.cc)

Change `FalaiseSkeletonModules.directory` in your `UDD_to_root_data.conf` if you use your own extraction file.
