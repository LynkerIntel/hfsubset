
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Hydrofabric Subsetter

## CLI Option

For those interested in using the NOAA NextGen fabric as is, we have
provided a Go-based CLI
[here](https://github.com/LynkerIntel/hfsubset/releases)

This utility has the following syntax:

``` bash
hfsubset - Hydrofabric Subsetter

Usage:
  hfsubset [OPTIONS] identifiers...
  hfsubset (-h | --help)

Example:
  hfsubset -l divides -o ./poudre-divides.gpkg -r "pre-release" -t hl "Gages-06752260"
  hfsubset -o ./poudre-all.gpkg -t hl "Gages-06752260"

Options:
  -l string
        Layers to subset (default "divides,nexus,flowpaths,network,hydrolocations")
  -o string
        Output file name (default "hydrofabric.gpkg")
  -r string
        Hydrofabric version (default "pre-release")
  -t string
        One of: hf, hl, or comid (default "hf")
```

## NextGen Needs GeoJSON

While GPKG support is the end goal for NextGen, it current requires
GeoJSON and CSV inputs.

Fortunately, `ogr2ogr` provides easy ways to extract these sub
layer/formats from the GPKG file.

Here is a full-stop example of extracting a subset for a hydrolocation,
using the CLI, and generating the needed files for NextGen

``` bash
mkdir poudre
cd poudre

hfsubset -l divides,nexus,flowpath_attributes -o ./poudre-subset.gpkg -r "pre-release" -t hl "Gages-06752260"

ogr2ogr -f GeoJSON catchments.geojson poudre-subset.gpkg -nln divides  
ogr2ogr -f GeoJSON nexus.geojson poudre-subset.gpkg -nln nexus
ogr2ogr -f CSV flowpath_attributes.csv poudre-subset.gpkg -nln flowpath_attributes

ls poudre
```
