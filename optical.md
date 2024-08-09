# CEOS-ARD for Optical <!-- omit in toc -->

- **Scope:** Item, Collection
- **Extension [Maturity Classification]:** Proposal
- **Owner**: @m-mohr

This extension specifies how to create [STAC] Items that
comply to the [CEOS-ARD product family specifications] (PFS) for optical sensors:

- [Aquatic Reflectance (AR), 1.0]
- LiDAR Terrain and Canopy Top Height, draft
- [Nighttime Lights Surface Radiance (NLSR), 1.0]
- [Surface Reflectance (SR), v5.0.1] (or [Surface Reflectance (SR), v5.0])
- [Surface Temperature (ST), v5.0]

> \[!NOTE]  
> This document is close to being completed,
> we just await some further inputs from the CEOS ARD working group and the JSON Schemas are missing.
> We encourage implementations and welcome feedback from early adopters.

**Additional resources:**

- Examples:
  - [Collection example](examples/optical-sr/collection.json): A STAC Collection for a Sentinel-2 L2A data using the STAC CEOS-ARD Optical profile
  - [Item example](examples/optical-sr/item.json): An examplary Item for the Sentinel-2 L2A Collection
- JSON Schema:
  - [AR](json-schema/optical-ar/schema.json) (ToDo)
  - [NLSR](json-schema/optical-nlsr/schema.json) (ToDo)
  - [SR](json-schema/optical-sr/schema.json) (ToDo)
  - [ST](json-schema/optical-st/schema.json) (ToDo)

**Table of Contents:**

- [Document Structure](#document-structure)
- [STAC Extensions](#stac-extensions)
- [STAC Collections](#stac-collections)
- [STAC Items](#stac-items)
  - [Common Metadata](#common-metadata)
  - [Accuracy](#accuracy)
  - [EO (Electro-Optical)](#eo-electro-optical)
  - [Processing](#processing)
  - [Projection](#projection)
  - [View](#view)
  - [Assets](#assets)
    - [Data](#data)
    - [Per-Pixel Metadata](#per-pixel-metadata)
    - [Bands](#bands)
- [Links](#links)
- [Notes](#notes)

## Document Structure

In general, the fields required in this extension are required to either meet
the *minimum requirements (threshold)* by the CEOS-ARD specification *or* are required fields in STAC.
Any additional optional field provided may lead to a higher percentage for the CEOS-ARD *desired requirements (target/goal)*.

The column *Field Name* refers to the STAC field names. The column *Req.* refers to the requirement number in the CEOS-ARD specification.

CEOS-ARD requirements 1.1, 1.2, and 2.1 are generally covered by implementing and publishing STAC metadata for the data.

## STAC Extensions

This profile doesn't define new STAC fields, it's just a profile that uses
[existing STAC extensions](#stac-extensions) to map and fulfill the CEOS-ARD requirements.

As the identifier for this profile is just a collection of existing extensions and only defines required fields,
you get a good bit of validation already without providing the identifier/schema of this profile in `stac_extensions`.
If your metadata is already compliant to this profile, you can omit the identifier for this profile in
`stac_extensions` to avoid costly regeneration of the Items. You won't get validation whether all required 
fields are present, but this could be checked manually in the CEOS assessment/review.

The following STAC extensions are relevant for this profile:

| Name              | Schema URI for `stac_extensions`                                        | Required |
| ----------------- | ----------------------------------------------------------------------- | :------: |
| [Accuracy]        | `https://stac-extensions.github.io/accuracy/v1.0.0-beta.1/schema.json`  | ✗        |
| [CEOS-ARD]        | `https://stac-extensions.github.io/ceos-ard/v0.2.0/optical/schema.json` | ✓        |
| [Classification]  | `https://stac-extensions.github.io/classification/v2.0.0/schema.json`   | ✗        |
| [Electro Optical] | `https://stac-extensions.github.io/eo/v2.0.0/schema.json`               | ✓        |
| [Processing]      | `https://stac-extensions.github.io/processing/v1.2.0/schema.json`       | ✗        |
| [Projection]      | `https://stac-extensions.github.io/projection/v2.0.0/schema.json`       | ✓        |
| [Raster]          | `https://stac-extensions.github.io/raster/v2.0.0/schema.json`           | ✓        |
| [View Geometry]   | `https://stac-extensions.github.io/view/v1.1.0/schema.json`             | ✓        |

For details about the CEOS-ARD extension, please see the separate [CEOS-ARD extension README](README.md#fields).
The CEOS-ARD extension fields can either be provided in the Collection or in the Items.

## STAC Collections

CEOS-ARD lists a lot of requirements (and fields) that have common values across all generated STAC Items and assets.
Thus, it is **recommended** to provide a STAC Collection for the Items and
put common fields (in the STAC Item `properties`) into [Collection Summaries].
While the STAC Item fields still need to be in the Item, too,
you can de-duplicate links and assets by putting common links once into the STAC Collection links.
Also, common assets can be just put once into the STAC Collection using [Collection Assets].
All this is still CEOS-ARD compliant as it doesn't require all information to be in a single file.

## STAC Items

STAC Items must always be valid, but not all STAC Item requirements are covered here,
only additional requirements and mappings to fulfill the CEOS-ARD requirements are listed here:

| Field Name      | Req.  | Description |
| --------------- | ----- | ----------- |
| stac_extensions | *n/a* | **REQUIRED.** Must contain [all implemented STAC extensions](#stac-extensions). |
| geometry        | 1.4   | **REQUIRED.** The geometry of the acquisition in WGS84. |
| bbox            | 1.4   | **REQUIRED.** The bounding box of the acquisition in WGS84. |

Generally, CEOS-ARD requests to provide DOIs for various resources.
For STAC, DOIs (e.g. `10.1109/5.771073`) must be converted to URLs (e.g. `https://doi.org/10.1109/5.771073`).
See [Resolve a DOI name](https://dx.doi.org/) for details.

### Common Metadata

| Field Name     | Req. | Description |
| -------------- | ---- | ----------- |
| datetime       | 1.3  | **REQUIRED.** The time of the acquisition, usually the central timestamp between `start_datetime` and `end_datetime`. |
| start_datetime | 1.3  | Start time of the acquisition. |
| end_datetime   | 1.3  | End time of the acquisition. |
| instruments    | 1.9  | **REQUIRED.** Instruments in lower-case. |
| constellation  | 1.9  | Constellation name in lower-case. |
| platform       | 1.9  | Platform (mission) name in lower-case. |

Requirement 1.9 asks to provide links to:

- [CEOS Missions Database] record(s)
- [CEOS Instruments Database] record(s)
- [CEOS Measurements Database] record(s)

### Accuracy

The following fields may be provided:

| Field Name                  | Req. | Description |
| --------------------------- | ---- | ----------- |
| accuracy:geometric_y_bias   | 1.8  | An estimate of the northern geometric accuracy: Bias, in meters. |
| accuracy:geometric_y_stddev | 1.8  | An estimate of the northern geometric accuracy: Standard deviation, in meters. |
| accuracy:geometric_x_bias   | 1.8  | An estimate of the eastern geometric accuracy: Bias, in meters. |
| accuracy:geometric_x_stddev | 1.8  | An estimate of the eastern geometric accuracy: Standard deviation, in meters. |
| accuracy:geometric_rmse     | 1.8  | Radial root mean square error (rRMSE) for sub-sample accuracy, in meters. |

The following links may be provided in the Collection or in the Items:

| Relation Type   | Req. | Description |
| --------------- | ---- | ----------- |
| elevation-model | 1.14 | URLs to the Digital Elevation Model (DEM). |
| surface-model   | 1.14 | URLs to the Digital Surface Model (DSM). |

The relation types `elevation-model` and `surface-model` can be provided in any file format (e.g., HTML or PDF),
but preferrably point to a STAC Collection or Item with additional metadata for the DEM/DSM.

### EO (Electro-Optical)

| Field Name     | Req. | Description |
| -------------- | ---- | ----------- |
| eo:cloud_cover | 1.17 | **REQUIRED for AR**. Cloud cover as quality flag. |
| eo:snow_cover  | 1.17 | Snow/Ice cover as quality flag. |

More data quality flags than `eo:cloud_cover` and `eo:snow_cover` should usually be set for CEOS-ARD compliance,
but other quality flag are not standardized in STAC yet. Please fall back to custom fields for now.

### Processing

Requirement 1.13 **requires** all algorithms, and the sequence in which they were applied, to be identified in the metadata.
This can be expressed in various ways:

- A written description as ordered CommonMark list in `processing:lineage`.
- A set of algorithms in `processing:software`.
  In this case you need to use another of these options to define the sequence in which they were applied.
- A link to a document that describes the algorithms, or an Algorithm Theoretical Basis document (relation type: `processing-description`).
- A link to processing instructions that allow to execute the algorithms (relation type: `processing-expression`).
  This would cover requirements 1.13 and 1.15 at the same time.

The following fields may be provided:

| Field Name            | Req.        | Description |
| --------------------- | ----------- | ----------- |
| processing:software   | 1.13 / 1.15 | A set of relevant software and algorithms, each with the applicable version number. |
| processing:expression | 1.13 / 1.15 | Machine-readable algorithms and/or processing chain. |
| processing:lineage    | 1.13 / 1.15 | Human-readable descriptions of the algorithms and/or processing chain. |

The following links may be provided:

| Relation Type          | Req.        | Description |
| ---------------------- | ----------- | ----------- |
| processing-expression  | 1.13 / 1.15 | A processing chain (or script) that describes how the data has been processed. |
| processing-description | 1.13        | A document that describes the algorithms, or an Algorithm Theoretical Basis document. |
| derived_from           | 1.15        | Points back to the source's STAC Item. May be multiple items, if the product is derived from multiple acquisitions. |

### Projection

The metadata is **required** to specify the coordinate reference system (1.5) and the map projection (1.6) through
either `proj:code` or one of the alternatives. The map projection is not required for ST.

| Field Name                            | Req.      | Description                                                  |
| ------------------------------------- | --------- | ------------------------------------------------------------ |
| proj:code / proj:wkt2 / proj:projjson | 1.5 / 1.6 | **REQUIRED**. One of the fields is required to be provided. If there's no suitable CRS code, set `proj:code` to `null` and add either `proj:wkt2` or `proj:projjson`. |

### View

| Field Name           | Req.      | Description |
| -------------------- | --------- | ----------- |
| view:incidence_angle | see below | **REQUIRED for AR, ST and SR.** The average incidence angle. |
| view:azimuth         | see below | **REQUIRED for AR, ST and SR.** The average azimuth angle. |
| view:sun_azimuth     | see below | **REQUIRED for AR, ST and SR.** The average sun azimuth angle. |
| view:sun_elevation   | see below | **REQUIRED.** The average sun elevation angle. |
| view:moon_azimuth    | see below | **REQUIRED for NLSR.** The average moon azimuth angle. [**TBD**](https://github.com/stac-extensions/view/pull/7) |
| view:moon_elevation  | see below | **REQUIRED for NLSR.** The average moon elevation angle. [**TBD**](https://github.com/stac-extensions/view/pull/7) |

The requirements regarding solar, lunar and viewing geometry have different requirement numbers:

- AR: 2.12
- NLSR: 2.11 and 2.16
- SR: 2.11
- ST: 2.8

If provided, all values **must** be in degrees.

### Assets

**Data Access:** Requirement 1.16 **requires** information about data access.
This requirement is automatically fulfilled by STAC if the referenced assets are publicly accessible.
If authentication or other steps are required to access the data, this should at least be described in the Collection or Item description.
For a more machine-readable way implementors could also use other means, such as the [Authentication] extension.

**Roles:** The role names specify the values to be used in the Asset's `roles`.
Each of the assets can either be exposed individually or grouped together in any form.
In the latter case the role names can simply be merged to a set of unique role names.
Roles can also be combined for a single file.
For example, a cloud mask which also includes cloud shadows  can use the roles `cloud` and `cloud-shadow` for a single file.
The `classification:classes` and/or `bands` properties can specify which value(s) correspond to clouds and/or cloud shadows respectively.

#### Data

For non-metadata requirements that refer to how the data must be created and processed
please consult the relevant CEOS-ARD specification.

There can be one or multiple data assets, e.g. one file per band or all bands in one file.

For data assets, the following properties are relavant in the context of CEOS-ARD:

| Field Name | Req.  | Description |
| ---------- | ----- | ----------- |
| roles      | *n/a* | **REQUIRED.** All data assets must to include the `data` role. |
| bands      | 1.10  | **REQUIRED.** All (spectral) bands must be specified in the assets as [Band Object]s. See [Bands](#bands) for details. |
| type       | *n/a* | STRONGLY RECOMMENDED. [Media Type] of the file format. |

#### Per-Pixel Metadata

This section describes required and optional per-pixel metadata.
Per-pixel metadata can be encoded in different way, for example:

1. multiple files with a mask per file (only requires the corresponding roles to be set, see below)
2. a single file with a mask per band (requires `bands`, see [Bands] in [Common metadata])
3. a single-band file with masks as bit-fields (requires one of the fields in the [Classification] extension)

In any way, the per-pixel metadata and its values must be clearly identifiable, e.g. by providing 
clear titles, roles, band information, bitfields, classes, and/or unit  where applicable.

For per-pixel metadata assets, the following properties are relavant in the context of CEOS-ARD:

| Field Name | Req.  | Description |
| ---------- | ----- | ----------- |
| roles      | *n/a* | **REQUIRED.** All data assets must to include the `metadata` role. |
| bands      | 1.10  | **REQUIRED.** All bands must be specified in the assets as [Band Object]s. See [Bands](#bands) for details. |
| type       | *n/a* | STRONGLY RECOMMENDED. [Media Type] of the file format. |

##### Incomplete Testing / Per-Pixel Assessment <!-- omit in toc -->

- **PFS:** all
- **Requirement:** 2.3
- **Role:** `incomplete-testing`

A per-pixel mask **must** be provided that identifies the pixel for which the per-pixel metadata check have not completed.
The asset **must** identify the values of the mask using the `classification:classes` or `classification:bitfield` properties.
The mask **may** be provided per band, in this case the `bands` property must be provided.

##### Saturation <!-- omit in toc -->

- **PFS:** all
- **Requirement:** 2.4
- **Role:** `saturation`

A per-pixel mask **must** be provided that identifies pixel for which at least one band is saturated.
The asset **must** identify the values of the mask using the `classification:classes` or `classification:bitfield` properties.
The mask **may** be provided per band, in this case the `bands` property must be provided.

##### Cloud / Cloud Shadow <!-- omit in toc -->

- **PFS:** all
- **Requirement:** 2.5 / 2.6
- **Role:** `cloud` / `cloud-shadow`

Separate per-pixel masks **must** be provided that identify pixels which are identified as cloud or cloud shadow respecitvely.
The asset **must** identify the values of the mask using the `classification:classes` or `classification:bitfield` properties.

The following additional assets are **required** for **NLSR only**:

| Role Name                | Req. | Description |
| ------------------------ | ---- | ----------- |
| `land-water`             | 2.7  | Indicates whether a pixel is assessed as being land or water. |
| `snow-ice`               | 2.8  | Indicates whether a pixel is assessed as being snow or ice. |
| `brightness-temperature` | 2.15 | Provides the brightness temperature from thermal bands per pixel. |
| `sun-azimuth`            | 2.16 | Per-pixel sun azimuth angles. `unit` is usually `degree`. |

The following additional assets are **required** for **AR only**:

| Role Name                 | Req. | Description |
| ------------------------- | ---- | ----------- |
| `land-water`              | 2.7  | Indicates whether a pixel is assessed as being land or water. |
| `waterbody-ice`           | 2.8  | Indicates whether a pixel is assessed as being sea, lake, or river ice. |
| `sun-glint`               | 2.9  | Indicates whether a pixel is assessed as absent or correctable (moderate), or uncorrectable (severe) Sun glint. Optionally, indicates the amount of Sun glint for each pixel and band. |
| `whitecap-foam`           | 2.11 | Indicates whether a pixel is assessed as affected by whitecaps or foam as a function of the wind speed or other. |
| `surface-scum`            | 2.14 | Indicates whether a pixel is assessed as affected by floating vegetation/surface scum. |
| `aod`                     | 2.15 | Indicates either per-pixel spectral Aerosol Optical Depth (AOD), or per-pixel AOD (550nm) and Angstrom exponent. |
| `optically-deep-shallow`  | 2.17 | Indicates, based on likelihood (bathymetry maps and average Kd (preferred) or based on turbidity or Secchi disk transparency), whether water pixels may be optically deep or optically shallow. This will most likely be bathymetry map contour based. |
| `turbid-water`            | 2.18 | Indicates whether a pixel is assessed as being turbid or not. |
| `asl`                     | 2.20 | Indicates approximate altitude (ASL) of water body pixels is required for atmospheric correction (range = -430 to ~6500m). |
| `surface-scum-correction` | 3.12 | Indicates whether a pixel has been corrected for floating vegetation/surface scum or not. |
| `turbid-water-correction` | 3.13 | Indicates whether the atmospheric correction accounted for a pixel being turbid or not. |

##### Optional <!-- omit in toc -->

The following assets are **optional** for **all of the PFS**:

| Role Name(s)      | Req. (PFS)                            | Description |
| ----------------- | ------------------------------------- | ----------- |
| `date`            | 1.3 (all)                             | Per-pixel acquisition timestamps. |
| `incidence-angle` | 2.8 (ST) / 2.11 (SR/NLSR) / 2.12 (AR) | Per-pixel incidence angles. `unit` is usually `degree`. |
| `azimuth`         | 2.8 (ST) / 2.11 (SR/NLSR) / 2.12 (AR) | Per-pixel azimuth angles. `unit` is usually `degree`. |
| `sun-azimuth`     | 2.8 (ST) / 2.11 (SR/NLSR) / 2.12 (AR) | Per-pixel sun azimuth angles. `unit` is usually `degree`. |
| `sun-elevation`   | 2.8 (ST) / 2.11 (SR/NLSR) / 2.12 (AR) | Per-pixel sun elevation angles. `unit` is usually `degree`. |

The following assets are **optional** for **either SR, ST, or NLSR**:

| Role Name(s)           | Req. (PFS)          | Description |
| ---------------------- | ------------------- | ----------- |
| `snow-ice`             | 2.7 (ST) / 2.8 (SR) | Indicates whether a pixel is assessed as being snow or ice.  |
| `land-water`           | 2.7 (SR)            | Indicates whether a pixel is assessed as being land or water. |
| `terrain-shadow`       | 2.9 (SR/NLSR)       | Indicates whether a pixel is not directly illuminated due to terrain shadowing. |
| `terrain-occlusion`    | 2.10 (SR/NLSR)      | Indicates whether a pixel is not visible to the sensor due to terrain occlusion during off-nadir viewing. |
| `terrain-illumination` | 2.12 (SR/NLSR)      | Coefficients used for terrain illumination correction are provided for each pixel. |

The following assets are **optional** for **AR**:

| Role Name(s)        | Req. | Description |
| ------------------- | ---- | ----------- |
| `sky-glint`         | 2.10 | Indicates the amount of sky glint for each pixel and band. |
| `adjacency-effects` | 2.13 | Provides the risk of per-pixel adjacency effects contamination, through flagging to denote per-pixel minimum, medium or high adjacency effects contamination. |
| `bottom-depth`      | 2.16 | Indicates where available: The bottom depth referenced to the mean sea level for the oceans and referenced to mean levels for lakes. |
| `brdf`              | 2.19 | Indicates which pixels are corrected for BRDF effects. |

#### Bands

**Note:** In this document we only refer to the new `bands` construct that was introcued in STAC 1.1.
Please check the [Raster] and [Electro Optical] extensions for details.

| Field Name             | Req.        | Description |
| ---------------------- | ----------- | ----------- |
| nodata                 | 2.2         | **REQUIRED.** Value(s) for no-data. |
| unit                   | 3.1         | The unit of the values in the asset, preferably compliant to [UDUNITS-2] units (singular). |
| classification:classes | *n/a*       | Lists the value that are in the file and describes their meaning. |
| lunar_illumination     | 2.14 (NLSR) | **REQUIRED for NLSR.** The average moon illumination, in %. [**TBD**](https://github.com/stac-extensions/eo/issues/31) |

For **data**:

- It is **required** to provide the `name` and `eo:central_wavelength`.
- For AR it is also **required** to provide `eo:full_width_half_max`.
- Additional band information such as spectral response details are optional.
- For ST the `unit` must be `kelvin`, for NLSR the `unit` is usually `nit`.

For **metadata**:

- It is **required** to provide the `name`.

See the STAC [Bands] for further details.

## Links

All links can be provided either in the Collection or in the Item,
unless the links are specific for each Item and/or differ between the Items.
If that's the case, they must be provided per Item.

In addition to the links defined in specific extensions above, the relation types listed in the table below can be used.

| Relation Type | Req.      | Description |
| ------------- | --------- | ----------- |
| related       | 1.14      | URL to the sources of auxiliary data used in the generation process, ideally as STAC Items. |
| describedby   | *various* | URL to documentation, see below for details. |

### related <!-- omit in toc -->

Links to the sources of auxiliary data used in the generation process.
This is **required** if auxiliary data is used in the generation process.
Excludes DEMs and DSMs, which must use [separate relation types](#accuracy) instead.

### describedby <!-- omit in toc -->

Various CEOS-ARD requirements ask for documentation about some specifics of the data. 
Those links should use the relation type `describedby` and have a clear title that clearly gives information about what the link documents.
Although we recommend to use `describedby` as relation type,
implementors can choose to use another relation type such as `about` if it suits their implementation better.
The requested documentation can in principle all be available through a single link, 
so you don't necessarily need to provide one link per bullet point.

The following links are **required**:
- 3.2 (ST): Documentation about corrections for atmosphere and emissivity.
- 3.4 (AR): Documentation about the atmospheric reflectance correction.
- 3.4 (SR): Documentation about the directional atmospheric scattering algorithms.
- 3.4 (NLSR): Documentation about the atmospheric corrections.
- 3.5 (AR/SR): Documentation about the water vapour corrections.
- 3.5 (NLSR): Documentation about the lunar radiance corrections.
- 3.6 (AR): Documentation about the ozone corrections.
- 3.6 (NLSR): Documentation about the stray light corrections.
- 3.7 (AR): Documentation about other trace faseous absorption corrections.

The following links are **optional**:
- 1.7 (all): Geometric Correction algorithm details.
- 1.8 (all): Description of the assessed geometric accuracy of the data.
- 1.11 (all): Instrument / sensor calibration parameters.
- 1.12 (all): Description of the assessed absolute radiometric uncertainty of the data.
- 1.17 (all) / 2.5 (all): Documentation about the cloud detection.
- 1.17 (all) / 2.6 (all): Documentation about the cloud shadow detection.
- 1.17 (all) / 2.7 (ST) / 2.8 (AR/NLSR/SR): Documentation about the (snow and) ice mask.
- 2.7 (AR/NLSR/SR): Documentation about the land and water mask.
- 3.2 (AR/SR/NLSR): Information about the measurement uncertainty.
- 3.3 (AR/SR/NLSR): Documentation about measurement normalization.
- 3.6 (SR): Documentation about the ozone corrections.
- 3.11 (AR): Information on adjacency effect correction.
- 3.12 (AR): Information on floating vegetation/surface scum water mask and/or correction
- 3.13 (AR): Information on turbid water mask and/or correction

## Notes

Some additional notes on the requirements

- 1.12 (AR): CEOS-ARD requires the number of bits.
  This is not reflected in the STAC profile and must be added by implementors manually
  via the `raster:bits_per_sample` property to be fully compliant
  (unless CEOS-ARD gets updated as it looks like an error to us).
- 3.8, 3.9 and 3.13 (AR): It is not clear how the sun/sky glint correction target requirements and the turbid water correction shall be provided.
- Requirement 4.1 lists no specific metadata requirements, but refers back to 1.x requirements.

[CEOS-ARD product family specifications]: <http://ceos.org/ard/>
[Aquatic Reflectance (AR), 1.0]: <https://ceos.org/ard/files/PFS/AR/v1.0/CARD4L_Product_Family_Specification_Aquatic_Reflectance-v1.0.pdf>
[Nighttime Lights Surface Radiance (NLSR), 1.0]: <https://ceos.org/ard/files/PFS/NLSR/v1.0/CARD4L_Product_Family_Specification_Nighttime_Light_Radiance-v1.0.pdf>
[Surface Reflectance (SR), v5.0]: <https://ceos.org/ard/files/PFS/SR/v5.0/CARD4L_Product_Family_Specification_Surface_Reflectance-v5.0.pdf>
[Surface Reflectance (SR), v5.0.1]: <https://ceos.org/ard/files/PFS/SR/v5.0.1/CEOS-ARD_Product_Family_Specification_Surface_Reflectance-v5.0.1.pdf>
[Surface Temperature (ST), v5.0]: <https://ceos.org/ard/files/PFS/ST/v5.0/CARD4L_Product_Family_Specification_Surface_Temperature-v5.0.pdf>

[CEOS Missions Database]: <https://database.eohandbook.com/database/missiontable.aspx>
[CEOS Instruments Database]: <https://database.eohandbook.com/database/instrumenttable.aspx>
[CEOS Measurements Database]: <https://database.eohandbook.com/measurements/overview.aspx>

[STAC]: <https://github.com/radiantearth/stac-spec>
[Maturity Classification]: <https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity>
[Common metadata]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/item-spec/common-metadata.md>
[Media Type]: <https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#asset-media-type>
[Collection Summaries]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#collection-fields>
[Collection Assets]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#assets>
[Bands]: <https://github.com/radiantearth/stac-spec/blob/f9b3c59ba810541c9da70c5f8d39635f8cba7bcd/item-spec/common-metadata.md#bands>
[Band Object]: <https://github.com/radiantearth/stac-spec/blob/f9b3c59ba810541c9da70c5f8d39635f8cba7bcd/item-spec/common-metadata.md#band-object>

[Accuracy]: <https://github.com/stac-extensions/accuracy>
[Authentication]: <https://github.com/stac-extensions/authentication>
[Classification]: <https://github.com/stac-extensions/classification>
[CEOS-ARD]: <https://github.com/stac-extensions/ceos-ard>
[Electro Optical]: <https://github.com/stac-extensions/eo>
[Processing]: <https://github.com/stac-extensions/processing>
[Projection]: <https://github.com/stac-extensions/projection>
[Raster]: <https://github.com/stac-extensions/raster>
[View Geometry]: <https://github.com/stac-extensions/view>

[UDUNITS-2]: <https://ncics.org/portfolio/other-resources/udunits2/>
