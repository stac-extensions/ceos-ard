# CEOS-ARD for Radar data <!-- omit in toc -->

- **Title:** CEOS-ARD for Radar
- **Identifier:** <https://stac-extensions.github.io/ceos-ard/v0.2.0/radar.json>
- **Field Name Prefix:** -
- **Scope:** Item
- **Extension [Maturity Classification]:** Proposal
- **Owner**: @m-mohr

This extension specifies how to create [STAC] Items that
comply to the [CEOS-ARD product family specifications] (PFS) for radar sensors.
The PFS for radar sensors is the [Combined CEOS-ARD for Synthetic Aperture Radar, version 1.0]
and includes the specifications for:

- Normalised Radar Backscatter (`NRB`)
- Polarimetric Radar (`POL`)
- Ocean Radar Backscatter (`ORB`)
- Geocoded Single-Look Complex (`GSLC`)

> \[!NOTE]  
> THis document is a draft and not finalized yet.
> We welcome feedback from early adopters.

**Additional resources:**

- Examples (ToDo)
- JSON Schema (ToDo)

**Table of Contents:**

- [Document Structure](#document-structure)
- [STAC Extensions](#stac-extensions)
- [STAC Collections](#stac-collections)
- [STAC Items](#stac-items)
  - [Common Metadata](#common-metadata)
  - [Projection](#projection)
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
| [CEOS-ARD]        | `https://stac-extensions.github.io/ceos-ard/v0.2.0/optical/schema.json` | ✓        |
| [Processing]      | `https://stac-extensions.github.io/processing/v1.1.0/schema.json`       | ✗        |
| [Projection]      | `https://stac-extensions.github.io/projection/v1.0.0/schema.json`       | ✓        |
| [Raster]          | `https://stac-extensions.github.io/raster/v2.0.0/schema.json`           | ✗        |
| [SAR]             | `https://stac-extensions.github.io/sar/v1.0.0/schema.json`              | ✓        |

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
| stac_extensions | *n/a* | **REQUIRED.** Must contain [all STAC extensions](#stac-extensions) implemented. |
| geometry        | 1.4   | **REQUIRED.** The geometry of the acquisition. |
| bbox            | 1.7.8 | **REQUIRED.** The bounding box of the acquisition in WGS84. |

Generally, CEOS-ARD requests to provide DOIs for various resources.
For STAC, DOIs (e.g. `10.1109/5.771073`) must be converted to URLs (e.g. `https://doi.org/10.1109/5.771073`).
See [Resolve a DOI name](https://dx.doi.org/) for details.

### Common Metadata

...

### Projection

The metadata is **required** to specify the coordinate reference system (1.5) and the map projection (1.6) through
either `proj:epsg` or one of the alternatives. The map projection is not required for ST.

**Deprecation Notice:** In a future version of the version of the projection extension `proj:epsg` will be replaced by `proj:code`.
It is recommended to provide both for now.

| Field Name                            | Req.      | Description                                                  |
| ------------------------------------- | --------- | ------------------------------------------------------------ |
| proj:epsg / proj:wkt2 / proj:projjson | 1.7.11    | **REQUIRED**. One of the fields is required to be provided, recommended is `proj:epsg`. If there's no suitable EPSG code, set `proj:epsg` to `null` and add either `proj:wkt2` or `proj:projjson`. |
| proj:bbox                             | 1.7.7     | **REQUIRED**. The bounding box of the acquisition in the projection defined in proj:epsg / proj:wkt2 / proj:projjson. |

## Links

...

## Notes

...
[CEOS-ARD product family specifications]: <http://ceos.org/ard/>
[Combined CEOS-ARD for Synthetic Aperture Radar, version 1.0]: <https://ceos.org/ard/files/PFS/SAR/v1.0/CEOS-ARD_PFS_Synthetic_Aperture_Radar_v1.0.pdf>

[CEOS Missions Database]: <https://database.eohandbook.com/database/missiontable.aspx>
[CEOS Instruments Database]: <https://database.eohandbook.com/database/instrumenttable.aspx>
[CEOS Measurements Database]: <https://database.eohandbook.com/measurements/overview.aspx>

[STAC]: <https://github.com/radiantearth/stac-spec>
[Maturity Classification]: <https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity>
[Common metadata]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/item-spec/common-metadata.md>
[Media Type]: <https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#asset-media-type>
[Collection Summaries]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#collection-fields>
[Collection Assets]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#assets>

[Accuracy]: <https://github.com/stac-extensions/accuracy>
[Authentication]: <https://github.com/stac-extensions/authentication>
[Classification]: <https://github.com/stac-extensions/classification>
[CEOS-ARD]: <https://github.com/stac-extensions/ceos-ard>
[Processing]: <https://github.com/stac-extensions/processing>
[Projection]: <https://github.com/stac-extensions/projection>
[Raster]: <https://github.com/stac-extensions/raster>
[SAR]: <https://github.com/stac-extensions/sar>

[UDUNITS-2]: <https://ncics.org/portfolio/other-resources/udunits2/>
