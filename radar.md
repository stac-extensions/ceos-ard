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

## STAC Collections

CEOS-ARD lists a lot of requirements (and fields) that have common values across all generated STAC Items and assets.
Thus, it is **recommended** to provide a STAC Collection for the Items and
put common fields (in the STAC Item `properties`) into [Collection Summaries].
While the STAC Item fields still need to be in the Item, too,
you can de-duplicate links and assets by putting common links once into the STAC Collection links.
Also, common assets can be just put once into the STAC Collection using [Collection Assets].
All this is still CEOS-ARD compliant as it doesn't require all information to be in a single file.

## STAC Items

### Common Metadata

...

## Links

...

## Notes

...

[CEOS-ARD product family specifications]: <http://ceos.org/ard/>
[Combined CEOS-ARD for Synthetic Aperture Radar, version 1.0]: <https://ceos.org/ard/files/PFS/SAR/v1.0/CEOS-ARD_PFS_Synthetic_Aperture_Radar_v1.0.pdf>

[STAC]: <https://github.com/radiantearth/stac-spec>
[Maturity Classification]: <https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity>
[Collection Summaries]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#collection-fields>
[Collection Assets]: <https://github.com/radiantearth/stac-spec/tree/v1.0.0/collection-spec/collection-spec.md#assets>
