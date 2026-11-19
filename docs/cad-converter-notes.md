# CAD converter documentation and notes

Workshop reference · reviewed 19 November 2026

## Official documentation

- [Converter overview and getting started](https://github.com/NVIDIA-Omniverse/usd-convert-cad)
- [CAD-to-USD data mapping](https://github.com/NVIDIA-Omniverse/usd-convert-cad/blob/main/docs/concept_mapping.md) — what is exported and how it is represented.
- [Known limitations](https://github.com/NVIDIA-Omniverse/usd-convert-cad/blob/main/LIMITATIONS.md)

The mapping documentation already explains one-way CAD/IFC → USD conversion, the absence of a general round trip, and the supported IFC relationships. The observations below supplement those references.

## Additional findings to discuss

**Specific geometry discrepancies.** Two shell-based prisms with different source lengths imported as the same shape. An Archicad-exported Esplanades slab retained its identity but had no mesh. These specific cases are not listed in the reviewed public *Known limitations* page.

Retained native diagnostics located both discrepancies in the geometry exposed by HOOPS after import, before NVIDIA USD authoring. They are candidates for Tech Soft 3D investigation; changing the USD writer alone would not recover the missing source geometry.

**Relationship mapping: implementation note.** The mapping documentation already states that links require matching exported GlobalIds. A focused test found that, with metadata enabled, the wall and door exported, but the opening did not; the wall’s `voidsElement` link had no target. Including hidden objects did not resolve it.

HOOPS exposes a separate opening-import setting, which the reviewed NVIDIA interface does not expose. If SDK scripting or configuration supplies the opening and resolves the links, this is integration work rather than a missing SDK capability. That route has not yet been verified for this case, so the observation is not classified here as an inherent relationship-support limitation.

**Implementation versus limitation.** Where the existing SDK exposes the required data and scripting can map it into USD, describe the work as an implementation option. A missing result from one converter configuration does not establish that the underlying technology lacks the capability.

**Other geometry observations already have vendor documentation.** Large-coordinate precision loss and coarse generated IFC tessellation are described in the [HOOPS IFC reader documentation](https://docs.techsoft3d.com/hoops/exchange/start/format/ifc_reader.html). They should be treated as documented reader limitations, rather than presented as newly discovered general defects.

## Evidence scope

Geometry findings refer to retained tests, including native converter 11.13 / HOOPS Exchange 2026.8 diagnostics; slab omission was also observed with public packages 0.2.0 and 0.3.0. The opening test used stock package 0.3.0 / native 11.9.0; interface review used source revision `4cac815a` (11.13). These are version-bound findings, not claims about every IFC file or confirmation that newer releases remain affected. This reference note summarises observations; it does not bundle the diagnostic source or test datasets.

These findings identify specific investigation points. They do not, by themselves, establish a need to replace the converter.
