# CEOS ARD Extension Specification

- **Title:** CEOS-ARD
- **Identifier:** <https://stac-extensions.github.io/ceos-ard/v0.2.0/schema.json>
- **Field Name Prefix:** ceos_ard
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the CEOS-ARD Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It specifies how to create STAC Items and Collections that comply to the various CEOS ARD product family specifications.
It is planned that this extension supersedes the existing [CARD4L extension](https://github.com/stac-extensions/card4l),
which itself is planned to be deprecated.

**WORK IN PROGRESS**

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
| ceos_ard:specification         | string    | **REQUIRED.** The CARD4L product family specification implemented. |
| ceos_ard:specification_version | string    | **REQUIRED.** The CEOS-ARD product family specification version. |

### Additional Field Information

#### ceos_ard:specification

The following values are supported:

**Type 'Optical':**
- `optical-ar`: Aquatic Reflectance
- `optical-lidar-tcth`: LiDAR Terrain and Canopy Top Height
- `optical-sr`: Surface Reflectance
- `optical-st`: Surface Temperature

**Type 'Radar':**
- `radar-gslc`: Geocoded Single-Look Complex (GSLC)
- `radar-insar`: Interferometric Radar (INSAR)
- `radar-nrb`: Normalised Radar Backscatter
- `radar-orb`: Ocean Radar Backscatter
- `radar-pr`: Polarimetric Radar

This field was named `card4l:specification` in the STAC CARD4L extension.
The values translate as follows:
- `SR` -> `optical-sr`
- `ST` -> `optical-st`
- `NRB` -> `radar-nrb`
- `POL` -> `radar-pr`

#### ceos_ard:specification_version

At the time of writing this document the following versions are the latest versions.
You can use older or newer versions, but the metadata mapping might not be complete.

**Type 'Optical':**
- `optical-ar`: 1.0
- `optical-lidar-tcth`: Unreleased, reserved for future use
- `optical-sr`: 5.0
- `optical-st`: 5.0

**Type 'Radar':**
- `radar-gslc`: Unreleased, reserved for future use
- `radar-insar`: Unreleased, reserved for future use
- `radar-nrb`: 5.5
- `radar-orb`: 1.0
- `radar-pr`: 3.5

This field was named `card4l:specification_version` in the STAC CARD4L extension.
The field values can be copied as is.

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type            | Description |
| --------------- | ----------- |
| card4l-document | **REQUIRED.** Provides at least one link to the CEOS-ARD product family specification document. Word (media type: `application/vnd.openxmlformats-officedocument.wordprocessingml.document`) and/or PDF (media type: `application/pdf`). |

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
