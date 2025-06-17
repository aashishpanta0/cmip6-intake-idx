# NEX-GDDP-CMIP6 Intake Catalog using OpenVisus

This repository provides a custom [Intake](https://intake.readthedocs.io/) data source for loading and subsetting the NASA NEX-GDDP-CMIP6 dataset using [OpenVisus](https://github.com/sci-visus/OpenVisus). It allows easy access to daily downscaled climate projections with support for:

- Custom `model`, `variable`, `scenario`, and `timestamp`
- Downsampled resolutions via `quality`
- Subsetting by latitude/longitude bounds (`lat_range`, `lon_range`)
- Output in `xarray.DataArray` format

---

##  Installation

Clone the repo and ensure the following dependencies are installed:

```bash
pip install intake xarray numpy
# and OpenVisus if not already installed:
pip install openvisus
```

---

## 🔧 Parameters

| Parameter     | Type   | Required | Description                                   |
|---------------|--------|----------|-----------------------------------------------|
| `model`       | str    | ✅        | CMIP6 model name (e.g. `ACCESS-CM2`)          |
| `variable`    | str    | ✅        | Variable name (e.g. `tas`, `pr`, `rhs`)       |
| `scenario`    | str    | ✅        | Emissions scenario (e.g. `historical`, `ssp585`) |
| `timestamp`   | str    | ✅        | Date in `YYYY-MM-DD` format                   |
| `quality`     | int    | ❌        | Resolution level (`0`=full, `-1`=half, etc.) default=0  |
| `lat_range`   | tuple  | ❌        | Latitude range `(min, max)` in degrees, default=entire region        |
| `lon_range`   | tuple  | ❌        | Longitude range `(min, max)` in degrees, default= entire region       |


--- 

## 🧪 Usage Example

```python
import intake

cat = intake.open_catalog("cmip6_catalog.yml")

ds = cat.nex_gddp_cmip6(
    model="CMCC-CM2-SR5",
    variable="tas",
    scenario="historical",
    timestamp="2005-06-15",
    quality=-2,
    lat_range=(0, 40),
    lon_range=(60, 120)
).read()

ds.plot()
```

