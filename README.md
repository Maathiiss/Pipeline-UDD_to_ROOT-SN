# Little Help to Reconstruct Data

This document describes how to use a really basic pipeline for SuperNEMO data reconstruction, from UDD to PCD and ROOT.

## Basic Use of the Code

The code is divided into several Falaise modules (many of them written by other members of the collaboration) and can be launched using the following commands:

```bash
source /sps/nemo/scratch/chauveau/software/falaise/develop/this_falaise.sh
flreconstruct -p UDD_to_root_data.conf -i UDD.brio 
```

This will produce a ROOT file called `extracted_data.root`, where a `TTree` will contain all the information you need.

## Understanding Each Module

The pipeline consists of 5 main modules:

- `udd2pcd`
- `pcd2cd`
- `TKReconstruct`: converts CD to TTD bank
- `ChargedParticleTracker`: converts TTD to PTD bank
- `Module0`: converts PTD to ROOT

Let’s start with the first two: `udd2pcd` and `pcd2cd`.

These modules were written by Manu and are used to convert from the UDD bank to the CD bank. Example files are located at:

```
/sps/nemo/snemo/snemo_data/reco_data/UDD/delta-tdc-10us-v3/snemo_run-XXXX_udd.brio
```

### `udd2pcd`

Source code available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/udd2pcd_module.cc#L35)

### `pcd2cd`

This module uses two files:

- **Time calibration file**: currently provided by Manu and stored in my space, but the path can be changed.
- **Energy calibration file**: created by me, using only the "a" parameter from the calibration; also stored in my space.

Source code available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/pcd2cd_module.cc)

---

### `TKReconstruct`

This module clusters and fits Geiger cell hits to reconstruct particle trajectories.

- Source code: [CimrmanModule GitHub](https://github.com/TomasKrizak/CimrmanModule.git)
- You can clone this repository and set your own path in `Falaise_TKReconstruct.directory`
- Many parameters can be modified — to be explored.

---

### `ChargedParticleTracker`

This module extrapolates particle trajectories to the foil and calorimeter to help identify particle types.

- Source code: **[Link missing – to be added]**

---

### `Module0`

This module extracts data from PTD to a ROOT file, generating `extracted_data.root` in the current directory.

- Simplified version available here: [Easy_ptd_to_root](https://github.com/Maathiiss/Easy_ptd_to_root.git)
- Original version: [FalaiseSkeletonModules](https://github.com/emchauve/FalaiseSkeletonModules.git)

You can modify this module to add more branches to the output `TTree`. Relevant resources:

- PTD extraction data structures: [Falaise Data Models](https://github.com/SuperNEMO-DBD/Falaise/tree/develop/source/falaise/snemo/datamodels)
- Example visualization code: [flvisualize - Event Browser](https://github.com/emchauve/Falaise/blob/develop/programs/flvisualize/EventBrowser/browser_tracks.cc)

Clone the repository and set your path in `FalaiseSkeletonModules.directory`.
