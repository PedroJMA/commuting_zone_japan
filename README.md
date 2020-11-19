# Commuting Zones in Japan

- This repository contains the CZ delineation, source codes and raw data based on [Adachi, Fukai, Kawaguchi, and Saito (2020)](https://daisukeadachi.github.io/assets/papers/commuting_zone_latest.pdf).

# `output` folder

- This folder contains the files of CZ delineations.



- Each file is an output of hierarchical agglomerative clustering (HAC) algorithm with an input of aggregated Population Census data in a year `(yyyy)` with municipality codes `(mcodes)`, either `original` or `harmonized`. 
  - Thus, they follow a naming convention `(yyyy)_(mcodes).csv`.



- Details in municipality codes `(mcodes)`:
  -  `original` means that the raw municipality codes in current census years are used to perform clustering. If you have data at the level of current-year municipality codes, delineation with `original` codes is relevant.
  - `harmonized`  means that the municipality codes are harmonized across census years and then cluster municipalities according to current commuting flows each year. The harmonized codes are based on 2015 municipality codes. This procedure controls the [changes of administrative municipalities](https://en.wikipedia.org/wiki/Municipal_mergers_and_dissolutions_in_Japan) and focuses on the actual evolution of commuting patterns over the years. If your focus is to analyze such patterns, delineation with `harmonized` codes is relevant.



- In `replication_by_tree_heights` sub-folder, results of other configurations of "tree heights" (see the paper for the detail) are stored. One can replicate them using the shared code in the `codes` folder and data in the `data` folder.
  - Our preferred choice of tree height is 0.98, following [Tolbert and Sizer (1996)](https://ageconsearch.umn.edu/record/278812/). One can replicate with this tree height.
  - However, CZ delineations users can replicate are different from ones in our paper published in this `output` folder. See "important note" below for detail.

# `data` folder

- This folder contains the raw data aggregated from Population Census.



- Each `csv` file summarizes Census commuting patterns for years 1980-2015 (for each five Census years) and municipality coding scheme (harmonized/original)
  - These data are the authors' original aggregation of information from the Population Census, provided by [Japan Statistics Bureau, Ministry of Internal Affairs and Communications](https://www.stat.go.jp/english/index.html).
  - Variable `pop` is the number of commuters from a living municipality (`living_mun`) to a commuting municipality (`commute_mun`) in each file.



- Sub-folder `mmm` contains files that contain connections between municipality codes and names generated by [Municipality Map Maker](http://www.tkirimura.com/mmm/).



- **Important note about confidentiality requirement**
  - Due to the confidentiality requirement by the above data provider, we need to conceal the origin-destination cells equal to or lower than ten (10) commuters. 
  - The number of commuters in such cells is replaced with `NA` in the dataset.
  - This concealment makes the clustering result different from those reported in our original paper, which uses non-concealed data for clustering.

# `codes` folder

- This folder contains R source code, `MASTER.R`. It reads the raw data located in the `data` folder, creates distance matrices, performs HAC algorithm and, generates files of CZ delineation.
- To replicate, one should 
  1. Open the `commuting_zone_japan.Rproj` in the mother folder.
  2. Run `MASTER.R`
- The output is in the `output/replicate_by_tree_heights` folder (replace the existing files).