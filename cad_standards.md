# CAD Project Standards

This document describes standards that the entire project should follow in order
to keep everything organized and consistent. Most standards are provided with a
justification - standards should not be arbitrarily created without considering
_why_.

## Table of Contents <!-- omit in toc -->

- [Flat Folder Structure](#flat-folder-structure)
  - [Standard](#standard)
  - [Justification](#justification)
- [Official Vendor Part Files](#official-vendor-part-files)
  - [Standard](#standard-1)
  - [Justification](#justification-1)
- [Unofficial Vendor Part Files](#unofficial-vendor-part-files)
  - [Standard](#standard-2)
  - [Justification](#justification-2)
- [File Names](#file-names)
  - [Standard](#standard-3)
  - [Justification](#justification-3)
- [Versioning](#versioning)
  - [Standard](#standard-4)
  - [Justification](#justification-4)
- [Configurations](#configurations)
  - [Standard](#standard-5)
  - [Justification](#justification-5)
- [Materials](#materials)
  - [Standard](#standard-6)
  - [Justification](#justification-6)
- [Appearances](#appearances)
  - [Standard](#standard-7)
  - [Justification](#justification-7)
- [Variables](#variables)
  - [Standard](#standard-8)
  - [Justification](#justification-8)
- [Software Versions](#software-versions)
  - [Standard](#standard-9)
  - [Justification](#justification-9)
- [Documentation](#documentation)
  - [Standard](#standard-10)
  - [Justification](#justification-10)
- [Commits](#commits)
  - [Standard](#standard-11)
  - [Justification](#justification-11)
- [Pull Requests](#pull-requests)
  - [Standard](#standard-12)
  - [Justification](#justification-12)
- [Sketches](#sketches)
- [Units](#units)
- [Assembly References](#assembly-references)
  - [Standard](#standard-13)
  - [Justification](#justification-13)

## Flat Folder Structure

### Standard

All _custom_ parts, drawings, and assembly files are stored directly in the
`cad` folder and not in any subfolders.

### Justification

A consistent folder structure is crucial for rapid development, ensuring all
members know where all parts are located.

While this may seem counterintuitive at first, storing all files in the same
folder provides a number of benefits. Most importantly, it allows the simple
reuse of subassemblies across the entire project. If, for example, you are
desigining a rocket and have a `payload` subassembly and you make a reusable
fastener assembly for that payload, you have no logical way to reuse that
fastener assembly in, say, the `engine` subassembly without duplicating the
files.

## Official Vendor Part Files

### Standard

All parts and assemblies downloaded from a vendor are stored in the
`cad/vendors_official/<VENDOR_NAME>` folder. Files downloaded directly from a
vendor are never modified (this includes file names). No files are stored
directly in the `cad/vendors_official` folder.

Derivatives of vendor parts are created as
[derived parts](https://help.solidworks.com/2016/English/SolidWorks/sldworks/c_Derived_Parts_Overview_Folder.htm)
and saved in the `cad` folder as
`Part Name (derived from <ORIGINAL_PART_FILE_NAME>)`.

_Exception_: Always add the material to downloaded vendor parts if one is not
set.

### Justification

If this rule is followed, the `vendor` folder can serve as a reliable source of
truth. It is always known that every file in this directory is exactly as the
vendor provides it. If there is an issue, it should be raised with the vendor
and not with SOAR. If the same vendor part is used in multiple places, it will
always be referenced as the same file and therefore increase consistency and
improve performance.

Saving derivative parts as derived parts ensures that the link to the original
file is preserved. This improves BOM generation and any updates to the original
part cascade through the entire project effectively.

## Unofficial Vendor Part Files

### Standard

When the vendor does not provide CAD files to use, models must be created of the
parts. These should be saved in the `cad/vendors_unofficial/<VENDOR_NAME>`
folder. When modifying these parts, the same rules apply as for official files.

### Justification

The same justification applies here as for official parts. Storing unofficial
parts in a seperate folder ensures that all members understand that these parts
are not official and therefore the exact dimensions are not guarunteed by the
vendor.

## File Names

### Standard

Files should be saved with a descriptive name, using spaces and capital letters
(ie `Descriptive File Name.sldprt`). File names should not duplicate information
available in the file, ie materials or dimensions, unless necessary to
distinguish a part from another. Similar files should have similar names,
preferrably starting with the same characters so that they appear together when
sorted alphabetically. If a file name is changed, all assemblies in the model
should be checked for broken file references. File names should describe the
part, not the use case: `Nose Cone Bolt` is a bad name.

### Justification

Formatting file names with capital letters and spaces ensures they appear
cleanly in assembly trees. Avoiding information available in the file prevents
the file name from needing to be changed if the file changes. Avoiding
describing the use case for the part ensures that the part can be reused if
necessary.

## Versioning

### Standard

No more than one version of a file can exist in the project at any time.

### Justification

Version control is performed through the use of Git. Versioning with file
duplicates invites conflicts and inconsistency.

## Configurations

### Standard

Configurations should be used whenever feasible. If two similar parts are used
in the project, combining the two into one part with two configurations should
be considered. Avoid excessively complicated configurations - changes should
always be predictable when switching configurations.

### Justification

The use of configurations ensures that multiple versions of the same part are
tied to the same bases, and can both be changed easily. This also vastly
improves large assembly performance.

## Materials

### Standard

All parts must have an accurate material set. If the exact material does not
exist, it should be created and saved in the `cad/materials` folder. If the
exact material is not known, it should be estimated as closely as possible.

### Justification

Providing every part with a material makes distinguishing different parts in
assemblies much easier, and is a prerequisite for mass estimates.

## Appearances

### Standard

If a part has a different appearance (ie, different color) than its material's
default, the appearance should be set appropropriately on the part. If a part
has a different color on one feature (ie, is only painted in one location), this
should be reflected in the part's appearances.

### Justification

Setting appearances on parts makes distinguishing parts in complex assemblies
significantly easier.

## Variables

### Standard

If a value is used more than once in a part, it should be stored in a global
variable and referenced in dimensions and features. Variables should have
descriptive all-lowercase names and when applicable, include units.

If a value is used in more than one part, it should be included in a
`variables.txt` file in the `cad` folder and the file should be imported into
the part.

### Justification

Using variables allows for rapid changes to parts or families of parts.

## Software Versions

### Standard

All files will be created using the most recent version of the CAD software
available at the time of creation.

### Justification

Using recent software ensures reliability and access to new features.

## Documentation

### Standard

If included, documentation will be provided in the form of Markdown files. The
overall project will be described in `readme.md` in the project root. All other
documentation will be given for specific parts as `Part File Name.md` in the
folder containing the part.

### Justification

Documentation files will appear next to the parts they document when sorted
alphabetically, ensuring that discovering and finding them is simple.

## Commits

### Standard

Every batch of changes to files will be associated with a Git commit. The commit
message will describe the changes made. The first line of the commit message
will be less than 50 characters and start with a capitalized present-tense verb.
For example, `"Add bolt holes to fin can"`. If more elaboration is necessary,
further information can be provided on subsequent lines, kept to less than 75
characters each. If a commit affects an issue, the issue will be referenced by
ID (ie, `Closes #34 by removing excess material`).

### Justification

This is the standard form for commits used in industry, including on GitHub.
GitHub reccomends, for best display and support, that these rules are followed.

## Pull Requests

### Standard

No commits shall be made directly to the `master` branch of a repository.
Instead, sets of changes will be submitted in the form of pull requests, which
may consist of one or more commits. Each pull request will be reviewed by at
least one member knowleadgable with the files being modified or the area being
updated.

### Justification

Enforcing and using pull requests ensures that all files are reviewed to keep in
line with standards and that no files conflict or interact negatively with other
areas. This also ensures that every change to the project has two members'
knowledge as input.

## Sketches

All sketches will be fully defined. No components of a sketch will be 'fixed'.
As much as possible, sketch relationships will be used to define sketch
components rather than dimensions. Preferrably, all dimensions should have
descriptive names.

## Units

All custom parts will use inches as the primary units. If possible, inches will
be defined fractionally, up to 1/256".

## Assembly References

### Standard

Unless abslutely necessary, parts should not reference other parts in
assemblies. To avoid this, only edit parts with "Isolate" enabled or by opening
the part file separately.

### Justification

If a part references another part in the same assembly, unexpected issues will
occur when either part is modified or the assembly is changed.
