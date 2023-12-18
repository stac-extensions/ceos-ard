# CEOS-ARD Extension Specification

- **Title:** CEOS-ARD
- **Identifier:**
  - <https://stac-extensions.github.io/ceos-ard/v0.2.0/schema.json>
  - See section [Profiles](#profiles) for more

- **Field Name Prefix:** ceos_ard
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the CEOS-ARD Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It specifies how to create STAC Items and Collections that comply to the various CEOS-ARD product family specifications.

It is planned that this extension supersedes the existing [CARD4L extension](https://github.com/stac-extensions/card4l),
which itself is planned to be deprecated. **WORK IN PROGRESS**

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item (ToDo)
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection (ToDo)
- [JSON Schema](json-schema/schema.json) (ToDo)
- [Changelog](./CHANGELOG.md) (ToDo)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [ ] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name                     | Data Type | Description |
| ------------------------------ | --------- | ----------- |
| ceos_ard:type                  | string    | **REQUIRED.** The CEOS-ARD (sensor) type implemented, one of `optical` or `radar`. |
| ceos_ard:specification         | string    | **REQUIRED.** The CEOS-ARD product family specification implemented. |
| ceos_ard:specification_version | string    | **REQUIRED.** The CEOS-ARD product family specification version implemented. |

The following combinations of values are supported for the fields above:

| PFS                                               | `ceos_ard:type` | `ceos_ard:specification` | `ceos_ard:specification_version` | STAC Status |
| ------------------------------------------------- | --------------- | ------------------------ | -------------------------------- | ----------- |
| Surface Reflectance                               | `optical`       | `SR`                     | `5.0`                            | Stable      |
| Surface Temperature                               | `optical`       | `ST`                     | `5.0`                            | Stable      |
| Aquatic Reflectance                               | `optical`       | `AR`                     | `1.0`                            | WIP         |
| Nighttime Lights Surface Radiance                 | `optical`       | `NLSR`                   | `1.0`                            | WIP         |
| LiDAR Terrain and Canopy Top Height               | `optical`       | `LIDAR-TCTH`             | *Unreleased*                     | Reserved    |
| Combined SAR: Normalised Radar Backscatter (NRB)  | `radar`         | `NRB`                    | `1.0`                            | WIP         |
| Combined SAR: Polarimetric Radar (POL)            | `radar`         | `POL`                    | `1.0`                            | WIP         |
| Combined SAR: Ocean Radar Backscatter (ORB)       | `radar`         | `ORB`                    | `1.0`                            | WIP         |
| Combined SAR: Geocoded Single-Look Complex (GSLC) | `radar`         | `GSLC`                   | `1.0`                            | WIP         |
| Combined SAR: Interferometric Radar (INSAR)       | `radar`         | `INSAR`                  | *Unreleased*                     | Reserved    |

## Profiles

Additional requirements for CEOS-ARD are specified in the corresponding STAC profiles:

- [Optical](optical.md)
- [Radar](radar.md)

One of the profiles above must be implemented.

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type                   | Description |
| ---------------------- | ----------- |
| ceos-ard-specification | *n/a* | **REQUIRED.** Provides at least one link to the CEOS-ARD specification document. Word (media type: `application/vnd.openxmlformats-officedocument.wordprocessingml.document`) and/or PDF (media type: `application/pdf`). |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
