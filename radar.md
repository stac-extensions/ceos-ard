# CEOS-ARD for Radar data <!-- omit in toc -->

- **Title:** CEOS-ARD for Radar
- **Identifier:** <https://stac-extensions.github.io/ceos-ard/v0.2.0/radar.json>
- **Field Name Prefix:** -
- **Scope:** Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This extension specifies how to create [STAC](https://github.com/radiantearth/stac-spec) Items that
comply to the [CEOS-ARD](http://ceos.org/ard/) product family specification for optical sensors:
[Combined CEOS-ARD for Synthetic Aperture Radar, 1.0](https://ceos.org/ard/files/PFS/SAR/v1.0/CEOS-ARD_PFS_Synthetic_Aperture_Radar_v1.0.pdf).
It includes the specifications for:
- Normalised Radar Backscatter (`NRB`)
- Polarimetric Radar (`POL`)
- Ocean Radar Backscatter (`ORB`)
- Geocoded Single-Look Complex (`GSLC`)

This extension doesn't define new STAC fields, it's just a profile that uses
[existing STAC extensions](#stac-extensions) to map and fulfill the CEOS-ARD requirements.

**WORK IN PROGRESS**

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
- [Notes](#notes)

## Document Structure

...

## STAC Extensions

...

## STAC Collections

...

## STAC Items

### CEOS-ARD

See the separate [CEOS-ARD extension README](README.md#fields) for details.

### Common Metadata

...

## Notes

...
