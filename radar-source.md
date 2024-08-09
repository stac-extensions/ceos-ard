# CEOS-ARD for Radar - Source Data <!-- omit in toc -->

- **Scope:** Item, Collection
- **Extension [Maturity Classification]:** Proposal
- **Owner**: @m-mohr

This extension specifies how to create [STAC] Items for Source Data that
comply to the [CEOS-ARD product family specifications] (PFS) for radar sensors.

> \[!IMPORTANT]  
> See also the [main specification document](radar.md) for CEOS ARD for Radar,
> which includes the specification for [Product Data](radar.md#stac-items-ard-product).

> \[!NOTE]  
> This document is a draft and not finalized yet.
> We welcome feedback from early adopters.

**Additional resources:**

- Examples (ToDo)
- JSON Schema (ToDo)

**Table of Contents:**

- [STAC Items (Source Data)](#stac-items-source-data)
  - [Extensions](#extensions)
  - [Common Metadata](#common-metadata)
  - [Processing](#processing)
  - [Projection](#projection)
  - [SAR](#sar)
  - [Satellite](#satellite)
  - [View Geometry](#view-geometry)
  - [Assets](#assets)
    - [Data](#data)
- [Notes / ToDo](#notes--todo)

## STAC Items (Source Data)

Metadata for the source data could also be provided in a different form,
e.g. in the CEOS ARD XML metadata format or in the source metadata.
Nevertheless, it is recommended to provide them as STAC Items for a consistent interface.

STAC Items must always be valid, but not all STAC Item requirements are covered here,
only additional requirements and mappings to fulfill the CEOS-ARD requirements are listed here:

| Field Name      | Req.  | Description                                                  |
| --------------- | ----- | ------------------------------------------------------------ |
| stac_extensions | *n/a* | **REQUIRED.** Must contain [all implemented STAC extensions](radar.md#stac-extensions). |
| id              | 1.6.6 | **REQUIRED.** The source product ID / file name. If another type of ID is provided, the file name can also be extracted from the asset with role `data`. |
| geometry        | 1.6.7 | **REQUIRED.** The geometry of the acquisition in WGS84.      |

Generally, CEOS-ARD requests to provide DOIs for various resources.
For STAC, DOIs (e.g. `10.1109/5.771073`) must be converted to URLs (e.g. `https://doi.org/10.1109/5.771073`).
See [Resolve a DOI name](https://dx.doi.org/) for details.

### Extensions

The following [STAC extensions](radar.md#stac-extensions) are relevant for the source Items:

| Name            | Required |
| --------------- | :------: |
| [CEOS-ARD]      |    ✓     |
| [Processing]    |    ✓     |
| [Projection]    |    ✓     |
| [SAR]           |    ✓     |
| [Satellite]     |    ✓     |
| [View Geometry] |    ✗     |

Providing the CEOS-ARD [fields](README.md#fields) and [links](README.md#relation-types) fulfills req. 1.3 and 1.4.
The fields can be provided either in the [Collection](radar.md#stac-collections) or in each Item.

### Common Metadata

| Field Name  | Req.  | Description                                                  |
| ----------- | ----- | ------------------------------------------------------------ |
| datetime    | 1.6.3 | **REQUIRED.** The start date and time of the source data. Can also be expressed as `start_datetime` and `end_datetime` instead. |
| instruments | 1.6.2 | **REQUIRED.** Instruments in lower-case.                     |
| platform    | 1.6.2 | **REQUIRED.** Satellite name in lower-case.                  |

Requirement 1.6.2 asks to provide links to:

- [CEOS Missions Database] record(s)
- [CEOS Instruments Database] record(s)
- [CEOS Measurements Database] record(s)

### Processing

| Field Name      | Req.  | Description                  |
| --------------- | ----- | ---------------------------- |
| processing:facility | 1.6.6 | **REQUIRED.** Processing facility |
| processing:datetime | 1.6.6 | **REQUIRED.** Processing date and time |
| processing:software / processing:version | 1.6.6 | **REQUIRED.** Software version(s) |
| processing:level | 1.6.6 | **REQUIRED.** Processing level |
| processing:facility | 1.6.6 | **REQUIRED.** Processing facility |
| processing:facility | 1.6.6 | **REQUIRED.** Processing facility |

### Projection

| Field Name                            | Req.  | Description                                                  |
| ------------------------------------- | ----- | ------------------------------------------------------------ |
| proj:code / proj:wkt2 / proj:projjson | 1.6.7 | **REQUIRED**. Coordinate Reference System (CRS). One of the fields is required to be provided. |
| proj:bbox                             | 1.7.7 | **REQUIRED**. The source data geometry in the given CRS. |

### SAR

| Field Name           | Req.  | Description                                                  |
| -------------------- | ----- | ------------------------------------------------------------ |
| sar:frequency_band   | 1.6.4 | **REQUIRED.** Radar band                                     |
| sar:center_frequency | 1.6.4 | **REQUIRED.** Center frequency                               |
| sar:polarizations    | 1.6.4 | **REQUIRED.** Polarization(s)                                |
| sar:instrument_mode  | n/a   | **REQUIRED.** Name of the sensor acquisition mode            |
| sar:product_type     | n/a   | **REQUIRED.** The product type for the source data, e.g. `GRD`. |
| sar:looks_azimuth    | 1.6.6 | **REQUIRED.** Azimuth number of looks                        |
| sar:looks_range      | 1.6.6 | **REQUIRED.** Range number of looks                          |

### Satellite

| Field Name      | Req.  | Description                  |
| --------------- | ----- | ---------------------------- |
| sat:orbit_state | 1.6.5 | **REQUIRED.** Pass direction |

### View Geometry

| Field Name   | Req.  | Description            |
| ------------ | ----- | ---------------------- |
| view:azimuth | 1.6.5 | Platform heading angle |

### Assets

**Data Access:** Requirement 1.6.1 **requires** information about data access.
This requirement is automatically fulfilled by STAC if the referenced assets are publicly accessible.
If authentication or other steps are required to access the data, this should at least be described in the Collection or Item description.
For a more machine-readable way implementors could also use other means, such as the [Authentication] extension.

**Roles:** The role names specify the values to be used in the Asset's `roles`.
Each of the assets can either be exposed individually or grouped together in any form.
In the latter case the role names can simply be merged to a set of unique role names.
Roles can also be combined for a single file.

#### Data

For non-metadata requirements that refer to how the data must be created and processed
please consult the relevant CEOS-ARD specification.
There can be one or multiple data assets per acquisition.

For data assets, the following properties are relavant in the context of CEOS-ARD:

| Field Name | Req.      | Description                                                  |
| ---------- | --------- | ------------------------------------------------------------ |
| roles      | *various* | **REQUIRED.** All data assets must to include the `data` role. |

## Notes / ToDo

- Req. 1.6.4: The following requirements can't be expressed in STAC yet:
  - Observation mode (i.e., Beam mode name)
  - Antenna pointing (Right/Left)
  - Beam ID (i.e., Beam mode Mnemonic)
- Req. 1.6.5: The orbit data source can't be specified in STAC yet.
- Req. 1.6.6: STAC can only specify a single value in `sar:looks_range` (i.e. for a single beam),
  but for multiple beams CEOS ARD requires to provide separate values for each beam.

[CEOS-ARD product family specifications]: <http://ceos.org/ard/>

[CEOS Missions Database]: <https://database.eohandbook.com/database/missiontable.aspx>
[CEOS Instruments Database]: <https://database.eohandbook.com/database/instrumenttable.aspx>
[CEOS Measurements Database]: <https://database.eohandbook.com/measurements/overview.aspx>

[STAC]: <https://github.com/radiantearth/stac-spec>
[Maturity Classification]: <https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity>

[Authentication]: <https://github.com/stac-extensions/authentication>
[CEOS-ARD]: <https://github.com/stac-extensions/ceos-ard>
[Processing]: <https://github.com/stac-extensions/processing>
[Projection]: <https://github.com/stac-extensions/projection>
[SAR]: <https://github.com/stac-extensions/sar>
[Satellite]: <https://github.com/stac-extensions/sat>
[View Geometry]: <https://github.com/stac-extensions/view>
