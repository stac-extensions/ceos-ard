# CEOS-ARD for Optical data <!-- omit in toc -->

- **Title:** CEOS-ARD for Optical
- **Identifier:** <https://stac-extensions.github.io/ceos-ard/v0.2.0/optical.json>
- **Field Name Prefix:** -
- **Scope:** Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This extension specifies how to create [STAC](https://github.com/radiantearth/stac-spec) Items that
comply to the [CEOS-ARD](http://ceos.org/ard/) product family specification for optical sensors:

- [Aquatic Reflectance (AR), 1.0](https://ceos.org/ard/files/PFS/AR/v1.0/CARD4L_Product_Family_Specification_Aquatic_Reflectance-v1.0.pdf)
- LiDAR Terrain and Canopy Top Height, draft
- [Nighttime Lights Surface Radiance (NTSR), 1.0](https://ceos.org/ard/files/PFS/NLSR/v1.0/CARD4L_Product_Family_Specification_Nighttime_Light_Radiance-v1.0.pdf)
- [Surface Reflectance (SR), v5.0](https://ceos.org/ard/files/PFS/SR/v5.0/CARD4L_Product_Family_Specification_Surface_Reflectance-v5.0.pdf)
- [Surface Temperature (ST), v5.0](https://ceos.org/ard/files/PFS/ST/v5.0/CARD4L_Product_Family_Specification_Surface_Temperature-v5.0.pdf)

**Additional resources:**

- Examples (ToDo)
- JSON Schema (ToDo)
  
  *Please note that the schema gives only a first indication on whether your metadata is correctly formatted
   as we can't provide a full schema for validation at the moment.*
  *For example, the assets are not fully validated yet. Passing the schema also does not imply that you are CEOS-ARD compliant!*

**Table of Contents:**

- [Document Structure](#document-structure)
- [STAC Extensions](#stac-extensions)
- [STAC Collections](#stac-collections)
- [STAC Items](#stac-items)
  - [CEOS-ARD](#ceos-ard)
  - [Common Metadata](#common-metadata)
    - [Fields](#fields)
    - [Links](#links)
  - [Accuracy](#accuracy)
    - [Fields](#fields-1)
    - [Links](#links-1)
  - [EO (Electro-Optical)](#eo-electro-optical)
    - [Fields](#fields-2)
      - [Bands](#bands)
    - [Links](#links-2)
  - [Processing](#processing)
    - [Fields](#fields-3)
    - [Links](#links-3)
  - [Projection](#projection)
  - [View](#view)
  - [STAC Item Links](#stac-item-links)
  - [STAC Item Assets](#stac-item-assets)
    - [Additional Asset Properties](#additional-asset-properties)
      - [raster:bands](#rasterbands)
- [Notes](#notes)

## Document Structure

In general, the fields required in this extension are required to either meet
the *minimum requirements (threshold)* by the CEOS-ARD specification *or* are required fields in STAC.
Any additional optional field provided may lead to a higher percentage for the CEOS-ARD *desired requirements (target)*.

The column *Field Name* refers to the STAC field names. The column *Req.* refers to the requirement number in the CEOS-ARD specification.

## STAC Extensions

This profile doesn't define new STAC fields, it's just a profile that uses
[existing STAC extensions](#stac-extensions) to map and fulfill the CEOS-ARD requirements.

| Name | Schema URI for `stac_extensions` | Required |
| ---- | -------------------------------- | -------- |
| [Accuracy](https://github.com/stac-extensions/accuracy)     | `https://stac-extensions.github.io/accuracy/v1.0.0-beta.1/schema.json`  | ✗ |
| [CEOS-ARD](https://github.com/stac-extensions/ceos-ard)     | `https://stac-extensions.github.io/ceos-ard/v0.2.0/optical/schema.json` | ✓ |
| [Electro Optical](https://github.com/stac-extensions/eo)    | `https://stac-extensions.github.io/eo/v1.1.0/schema.json`               | ✓ |
| [File](https://github.com/stac-extensions/file)             | `https://stac-extensions.github.io/file/v2.0.0/schema.json`             | ✗ |
| [Processing](https://github.com/stac-extensions/processing) | `https://stac-extensions.github.io/processing/v1.1.0/schema.json`       | ✗ |
| [Projection](https://github.com/stac-extensions/projection) | `https://stac-extensions.github.io/projection/v1.0.0/schema.json`       | ✓ |
| [Raster](https://github.com/stac-extensions/raster)         | `https://stac-extensions.github.io/raster/v1.1.0/schema.json`           | ✓ |
| [View Geometry](https://github.com/stac-extensions/view)    | `https://stac-extensions.github.io/view/v1.0.0/schema.json`             | ✓ |

## STAC Collections

CEOS-ARD lists a lot of requirements (and fields) that have common values across all generated STAC Items and assets.
Thus, it is **recommended** to provide a STAC Collection for the Items and put common fields (in the STAC Item `properties`)
into [Collection `summaries`](https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#collection-fields).
While the STAC Item fields still need to be in the Item, too, you can de-duplicate links and assets by putting common
links once into the STAC Collection links. Also, common assets can be just put once into the STAC Collection using the
STAC extension [Collection Assets](https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#assets).
All this is still CEOS-ARD compliant as it doesn't require all information to be in a single file.

## STAC Items

STAC Items must always be valid, but not all STAC Item requirements are covered here,
only additional requirements and mappings to fulfill the CEOS-ARD requirements are listed here:

| Field Name      | Req.  | Description |
| --------------- | ----- | ----------- |
| stac_extensions | *n/a* | **REQUIRED.** Must contain [all STAC extensions](#stac-extensions) implemented. |
| geometry        | 1.4   | **REQUIRED.** The geometry of the acquisition. |
| bbox            | 1.4   | **REQUIRED.** The bounding box of the acquisition. |
| links           | *n/a* | **REQUIRED.** Various specific links must be provided, see below for information about DOIs and relation types. |

Generally, CEOS-ARD requests to provide DOIs for various resources.
For STAC, DOIs (e.g. `10.1109/5.771073`) must be converted to URLs (e.g. `https://doi.org/10.1109/5.771073`).
See [Resolve a DOI name](https://dx.doi.org/) for details.

### CEOS-ARD

See the separate [CEOS-ARD extension README](README.md#fields) for details.

### Common Metadata

#### Fields

| Field Name     | Req.  | Description |
| -------------- | ----- | ----------- |
| license        | *n/a* | Recommended to be specified in a STAC Collection. |
| datetime       | 1.3   | **REQUIRED.** The time of the acquisition, usually the central timestamp between `start_datetime` and `end_datetime`. |
| start_datetime | 1.3   | Start time of the acquisition. |
| end_datetime   | 1.3   | End time of the acquisition. |
| instruments    | 1.9   | **REQUIRED.** Instruments in lower-case. |
| constellation  | 1.9   | Constellation name in lower-case. |
| platform       | 1.9   | Platform (mission) name in lower-case. |

#### Links

| Relation Type          | Req. | Description |
| ---------------------- | ---- | ----------- |
| platform               | 1.9  | URL to a [CEOS Missions Database](https://database.eohandbook.com/database/missiontable.aspx) record. |
| instrument             | 1.9  | URL to a [CEOS Instruments Database](https://database.eohandbook.com/database/instrumenttable.aspx) record. |
| measurement            | 1.9  | URL to a [CEOS Measurements Database](https://database.eohandbook.com/measurements/overview.aspx) record. |
| instrument-calibration | 1.11 | URL to the instrument / sensor calibration parameters. |

### Accuracy

#### Fields

| Field Name                  | Data Type | Req. | Description |
| --------------------------- | --------- | ---- | ----------- |
| accuracy:geometric_y_bias   | number    | 1.8  | An estimate of the northern geometric accuracy: Bias, in meters. |
| accuracy:geometric_y_stddev | number    | 1.8  | An estimate of the northern geometric accuracy: Standard deviation, in meters. |
| accuracy:geometric_x_bias   | number    | 1.8  | An estimate of the eastern geometric accuracy: Bias, in meters. |
| accuracy:geometric_x_stddev | number    | 1.8  | An estimate of the eastern geometric accuracy: Standard deviation, in meters. |
| accuracy:geometric_rmse     | number    | 1.8  | Radial root mean square error (rRMSE) for sub-sample accuracy, in meters. |

#### Links

| Relation Type        | Req. | Description |
| -------------------- | ---- | ----------- |
| geometric-correction | 1.7  | URL to the Geometric Correction algorithm details. |
| geometric-accuracy   | 1.8  |	URL describing the assessed geometric accuracy of the data. |
| radiometric-accuracy | 1.12 | URL describing the assessed absolute radiometric uncertainty of the data. |
| elevation-model      | 1.14 | URLs to the Digital Elevation Model (DEM). |
| surface-model        | 1.14 | URL to the Digital Surface Model (DSM). |

The relation types `elevation-model` and `surface-model` can be provided in any file format (e.g., HTML or PDF),
but preferrably point to a STAC Collection or Item with additional metadata for the DEM/DSM.

### EO (Electro-Optical)

#### Fields

| Field Name     | Req. | Description |
| -------------- | ---- | ----------- |
| eo:cloud_cover | 1.17 | **REQUIRED** in AR. Cloud cover as quality flag. |
| eo:snow_cover  | 1.17 | Snow/Ice cover as quality flag. |
| eo:bands       | 1.10 | **REQUIRED.** All spectral bands must be specified in the assets. |

More data quality flags than `eo:cloud_cover` and `eo:snow_cover` should usually be set for CEOS-ARD compliance,
but other quality flag are not standardized in STAC yet. Please fall back to custom fields for now.

##### Bands

**Deprecation Notice:** In a future version of the version of STAC and the EO extension `eo:bands` will be replaced by `bands`.
It is recommended to provide both for now.

For `eo:bands` (soon: `bands`) it is **required** provide the `name` and `central_wavelength` (soon: `eo:central_wavelength`).
For AR it is also **required** to provide `full_width_half_max` (soon: `eo:full_width_half_max`).
Additional band information such as spectral response details are optional.

#### Links

| Relation Type | Req.                       | Description |
| ------------- | -------------------------- | ----------- |
| cloud         | 1.17 / 2.5                 | URL to documentation about the cloud detection. |
| cloud-shadow  | 1.17 / 2.6                 | URL to documentation about the cloud shadow detection. |
| snow-ice      | 1.17 / 2.7 (ST) / 2.8 (SR) | URL to documentation about the snow and ice mask. |

### Processing

Requirement 1.13 **requires** all algorithms, and the sequence in which they were applied, to be identified in the metadata.
This can be expressed in various ways:
- A written description as ordered CommonMark list in `processing:lineage`.
- A set of algorithms in `processing:software`.
  In this case you need to use another of these options to define the sequence in which they were applied.
- A link to a document that describes the algorithms, or an Algorithm Theoretical Basis document (relation type: `processing-description`).
- A link to processing instructions that allow to execute the algorithms (relation type: `processing-expression`).
  This would cover requirements 1.13 and 1.15 at the same time.

#### Fields

| Field Name            | Req.        | Description |
| --------------------- | ----------- | ----------- |
| processing:software   | 1.13 / 1.15 | A set of relevant software and algorithms, each with the applicable version number. |
| processing:expression | 1.13 / 1.15 | Machine-readable algorithms and/or processing chain. |
| processing:lineage    | 1.13 / 1.15 | Human-readable descriptions of the algorithms and/or processing chain. |

#### Links

| Relation Type          | Req. | Description |
| ---------------------- | ---- | ----------- |
| processing-expression  | 1.13 / 1.15 | A processing chain (or script) that describes how the data has been processed. |
| processing-description | 1.13 | A processing chain (or script) that describes how the data has been processed. |
| derived_from           | 1.15 | Points back to the source's STAC Item. May be multiple items, if the product is derived from multiple acquisitions. |

### Projection

The metadata is **required** to specify the coordinate reference system (1.5) and the map projection (1.6) through
either `proj:epsg` or one of the alternatives. The map projection is not required for ST.

**Deprecation Notice:** In a future version of the version of the projection extension `proj:epsg` will be replaced by `proj:code`.
It is recommended to provide both for now.

| Field Name                            | Req.      | Description                                                  |
| ------------------------------------- | --------- | ------------------------------------------------------------ |
| proj:epsg / proj:wkt2 / proj:projjson | 1.5 / 1.6 | **REQUIRED**. One of the fields is required to be provided. If there's no suitable EPSG code, set `proj:epsg` to `null` and add either `proj:wkt2` or `proj:projjson`. |

### View

| Field Name           | Req.  | Description                                                  |
| -------------------- | ----- | ------------------------------------------------------------ |
| view:off_nadir       | *n/a* | The average off-nadir angle, for per-pixel angles. In degrees. |
| view:incidence_angle | \[2]  | **REQUIRED.** The average incidence angle, for per-pixel angles, refer to the asset with the key `incidence-angle`. In degrees. |
| view:azimuth         | \[2]  | **REQUIRED.** The average azimuth angle, for per-pixel angles, refer to the asset with the key `azimuth`. In degrees. |
| view:sun_azimuth     | \[2]  | **REQUIRED.** The average sun azimuth angle, for per-pixel angles, refer to the asset with the key `sun-azimuth`. In degrees. |
| view:sun_elevation   | \[2]  | **REQUIRED.** The average sub elevation angle, for per-pixel angles, refer to the asset with the key `sun-elevation`. In degrees. |

\[2] Requirement 2.8 for Surface Temperature (ST) / 2.11 for Surface Reflectance (SR)

### STAC Item Links

In addition to the links defined in specific extensions above, the links can or must be specified:

| Relation Type             | Req.                | Description                                                  |
| ------------------------- | ------------------- | ------------------------------------------------------------ |
| related                   | 1.14                | URL to the sources of auxiliary data used in the generation process. This is **required** if auxiliary data is used in the generation process. Excludes DEMs and DSMs, which must use [separate relation types](#accuracy) instead. |
| cloud                     | 1.17 / 2.5          | URL to documentation about the cloud detection. |
| cloud-shadow              | 1.17 / 2.6          | URL to documentation about the cloud shadow detection. |
| snow-ice                  | 1.17 / 2.7 (ST) / 2.8 (SR) | URL to documentation about the snow and ice mask. |
| land-water                | 2.7 (SR)            | URL to documentation about the land and water mask (SR only). |
| atmosphere-emissivity     | 3.2 (ST)            | **REQUIRED.** URL to documentation about corrections for atmosphere and emissivity (ST only). |
| measurement-normalization | 3.3 (SR)            | URL to documentation about measurement normalization (SR only). |
| atmospheric-scattering    | 3.4 (SR)            | **REQUIRED.** URL to documentation about the directional atmospheric scattering algorithms (SR only). |
| water-vapor               | 3.5 (SR)            | **REQUIRED.** URL to documentation about the water vapour corrections (SR only). |
| ozone                     | 3.6 (SR)            | URL to documentation about the ozone corrections (SR only). |

### STAC Item Assets

**Data Access:** Requirement 1.16 **requires** information about data access.
This requirement is automatically fulfilled by STAC if the referenced assets are publicly accessible.
If authentication or other steps are required to access the data, this should at least be described in the Collection or Item description.
For a more machine-readable way implementors could also use other means, such as the
[Authentication extension](https://github.com/stac-extensions/authentication).

**Per-Pixel Metadata:** Whether the metadata is provided in a single record relevant to all pixels,
or separately for each pixel, is at the discretion of the data provider.

**Roles:**: The role names specify the values to be used in the Asset's `roles`.
Each of the assets can either be exposed individually or grouped together in any form.
In the latter case the role names can simply be merged to a set of unique role names.
Roles can also be combined for a single file.
For example, a cloud mask which is also including cloud shadows 
can use the roles `cloud` and `cloud-shadow` for a single file.
The `values` in `raster:bands` property can the specify which value(s) correspond to clouds and
which value(s) correspond to cloud shadows respectively.
The *italic* role names could be used as the asset's key.

**Additional Properties:** The **bold** additional properties are required.

| Role Name(s)                           | Req.                 | Additional properties                                        | Description                                                  |
| -------------------------------------- | -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| data                             | 3.1 / 2.2            | `type`, `created`, **`eo:bands`**, **`raster:bands`** (with `data_type`, `bits_per_sample` and **`nodata`**), `file:byte_order` | **REQUIRED.** Points to the actual measurements. The value(s) for pixels that do not correspond to an observation must be provided in the property `nodata` in `raster:bands`. |
| *date*, metadata                 | 1.3                  | `type` | Points to a file with per-pixel acquisition timestamps. |
| *incomplete-testing*, metadata   | 2.3                  | `type`, **`raster:bands`** (with **`values`**)               | **REQUIRED.** Points to a file that identifies pixels for which the per-pixel tests have not all been successfully completed. See CEOS-ARD req. 2.3 for details. |
| *saturation*, metadata           | 2.4                  | `type`, `eo:bands`, **`raster:bands`** (with **`values`**)   | **REQUIRED.** Points to a file that indicates where pixels in the input spectral bands are saturated. If the saturation is given per band, either `eo:bands` or `values` in `raster:bands` is **required** to indicate which pixels are saturated for each spectral band. |
| *cloud*, metadata                | 2.5                  | `type`, **`raster:bands`** (with **`values`**)               | **REQUIRED.** Points to a file that indicates whether a pixel is assessed as being cloud. |
| *cloud-shadow*, metadata         | 2.6                  | `type`, **`raster:bands`** (with **`values`**)               | **REQUIRED.** Points to a file that indicates whether a pixel is assessed as being cloud shadow. |
| *snow-ice*, metadata             | 2.7 (ST) / 2.8 (SR)  | `type`, **`raster:bands`** (with **`values`**)               | Points to a file that indicates whether a pixel is assessed as being snow/ice or not. |
| *land-water*, metadata           | 2.7 (SR)             | `type`, **`raster:bands`** (with **`values`**)               | Points to a file that indicates whether a pixel is assessed as being land or water. |
| *incidence-angle*, metadata      | 2.8 (ST) / 2.11 (SR) | `type`, **`raster:bands`** (with `data_type`, `bits_per_sample` and **`unit`**), `file:byte_order` | Points to a file with per-pixel incidence angles. `unit` is usually `degree`. |
| *azimuth*, metadata              | 2.8 (ST) / 2.11 (SR) | `type`, **`raster:bands`** (with `data_type`, `bits_per_sample` and **`unit`**), `file:byte_order` | Points to a file with per-pixel azimuth angles. `unit` is usually `degree`. |
| *sun-azimuth*, metadata          | 2.8 (ST) / 2.11 (SR) | `type`, **`raster:bands`** (with `data_type`, `bits_per_sample` and **`unit`**), `file:byte_order` | Points to a file with per-pixel sun azimuth angles. `unit` is usually `degree`. |
| *sun-elevation*, metadata        | 2.8 (ST) / 2.11 (SR) | `type`, **`raster:bands`** (with `data_type`, `bits_per_sample` and **`unit`**), `file:byte_order` | Points to a file with per-pixel sun elevation angles. `unit` is usually `degree`. |
| *terrain-shadow*, metadata       | 2.9 (SR)             | `type`, **`raster:bands`** (with **`values`**)               | Points to a file that indicates whether a pixel is not directly illuminated due to terrain shadowing. |
| *terrain-occlusion*, metadata    | 2.10 (SR)            | `type`, **`raster:bands`** (with **`values`**)               | Points to a file that indicates whether a pixel is not visible to the sensor due to terrain occlusion during off-nadir viewing. |
| *terrain-illumination*, metadata | 2.12 (SR)            | `type`, `raster:bands` (with `data_type`, `bits_per_sample`), `file:byte_order` | Points to a file with coefficients used for terrain illumination correction are provided for each pixel. |

Note: `raster:bands[*].values` is not standardized yet in STAC, this could change to
`file:values` or something different with a similar structure in the future.

#### Additional Asset Properties

The following table lists properties that may occur in the assets.
The list doesn't specify which fields apply to which asset and it also doesn't specify which fields are required.
For those details please refer to the ["Additional properties" column in the table above](#stac-item-assets).

| Field Name      | Req.      | Data Type                                                    | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| type            | *n/a*     | string                                                       | STRONGLY RECOMMENDED. The [media type](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#asset-media-type) of the file format.     |
| created         | *n/a*     | string                                                       | The time of the processing is specified via the `created` property of the asset as specified in the [STAC Common metadata](https://github.com/radiantearth/stac-spec/tree/v1.0.0/item-spec/common-metadata.md#date-and-time). |
| eo:bands        | 1.10      | \[[Band Object](https://github.com/stac-extensions/eo/blob/v1.0.0/README.md#band-object)\] | see [Bands in EO](#eo-electro-optical) |
| raster:bands    | see below | \[[Raster Band Object](https://github.com/stac-extensions/raster/blob/v1.1.0/README.md#raster-band-object)\] | Bands with at least the required fields for the corresponding asset role (see above and below). |
| file:byte_order | *n/a*     | string                                                       | One of `big-endian` or `little-endian`.                      |

##### raster:bands

| Field Name      | Req.  | Data Type                                                    | Description                                                  |
| --------------- | ----- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| data_type       | *n/a* | string                                                       | One of the [Data Types](https://github.com/stac-extensions/raster/blob/v1.1.0/README.md#data-types). |
| unit            | *n/a* | string                                                       | The unit of the values in the asset, preferably compliant to [UDUNITS-2](https://ncics.org/portfolio/other-resources/udunits2/) units (singular). |
| bits_per_sample | *n/a* | integer                                                      | Actual number of bits per sample (e.g., 8, 16, 32, ...) |
| values          | *n/a* | \[[Mapping Object](https://github.com/stac-extensions/file/blob/v2.1.0/README.md#mapping-object)\] | Lists the value that are in the file and describes their meaning. |
| nodata          | 2.2   | \[any]                                                       | Value(s) for no-data. |

*ToDo: values should be replaced by the classification extension.*

## Notes

Some additional notes on the requirements

- Requirements 1.1, 1.2, and 2.1 are generally covered by implementing and publishing STAC metadata for the data.
- 1.12 (AR): CEOS-ARD requires the number of bits.
  This is not reflected in the STAC profile and must be added by implementors manually to be fully compliant (unless CEOS-ARD gets updated).
- 2.13 (SR): CEOS-ARD lists no specific requirements thus it's missing in this document.
- 3.2 (SR) / 3.3 (ST): Measurement Uncertainty is not required and it was not clear in which form this should be provided. 
  Also the CEOS-ARD specification states for SR that
  
  > "\[i]n current practice, users determine fitness for purpose based on knowledge of the lineage of the data,
  > rather than on a specific estimate of measurement uncertainty."
  
  Thus this requirement is not captured in this document.
- 3.3 (SR): Measurement Normalisation is not required and it was not clear in which form this should be provided. 
  Thus this requirement is not captured in this document, but we have added the link relation type
  `measurement-normalization` to link to information on measurement normalisation.
- 4.1 (SR): CEOS-ARD lists no specific requirements thus it's missing in this document.
