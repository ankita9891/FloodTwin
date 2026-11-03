# Input data

The notebook needs the India Meteorological Department (IMD) daily gridded data for **1951-2025**. They are distributed by IMD and are not included in this repository.

## Download

Download the yearly binary files from the IMD Pune data portal (https://www.imdpune.gov.in, gridded data section):

| Variable | Resolution | File name expected by the notebook | Years |
|---|---|---|---|
| Rainfall | 0.25° × 0.25° | `Rainfall_ind<year>_rfp25.grd` | 1951-2025 |
| Maximum temperature | 1° × 1° | `Maxtemp_MaxT_<year>.GRD` | 1951-2025 |
| Minimum temperature | 1° × 1° | `Mintemp_MinT_<year>.GRD` | 1951-2025 |

This gives 3 × 75 = 225 files.

## Folder layout

Place the files in three folders inside `BASE_DIR`, which is set in Step 2 of the notebook (default on Colab: `MyDrive/ISRO_Digital_Twin`; locally: the folder that contains the notebook):

```
BASE_DIR/
├── Rainfall/     # Rainfall_ind1951_rfp25.grd ... Rainfall_ind2025_rfp25.grd
├── Max_temp/     # Maxtemp_MaxT_1951.GRD ... Maxtemp_MaxT_2025.GRD
└── Min_temp/     # Mintemp_MinT_1951.GRD ... Mintemp_MinT_2025.GRD
```

If the folders are elsewhere, edit `BASE_DIR` or `RAIN_DIR`, `MAXTEMP_DIR` and `MINTEMP_DIR` in Step 2.

## References

* Pai, D.S., et al., 2014. Development of a new high spatial resolution (0.25° × 0.25°) long period (1901-2010) daily gridded rainfall data set over India and its comparison with existing data sets over the region. *Mausam* 65, 1-18.
* Srivastava, A.K., Rajeevan, M., Kshirsagar, S.R., 2009. Development of a high resolution daily gridded temperature data set (1969-2005) for the Indian region. *Atmospheric Science Letters* 10, 249-254.
