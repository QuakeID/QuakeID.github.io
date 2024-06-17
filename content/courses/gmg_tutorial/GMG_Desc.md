---
title: Package Description
linktitle: Package Description
toc: true
type: docs
date: "2021-10-27T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Geophysical Model Generator
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## 1. What is the Geophysical Model Generator?
The idea for the Geophysical Model Generator was born out of the need to view different geophysical datasets (e.g. tomographies, gravity measurements, receiver functions and others) in a single joint manner without having to resort to specifically programmed visualization routines for each case. The Geophysical Model Generator is NOT a single program, but can rather be seen as a set of tools (written in julia) that allow to import and modify different datasets and transfer them to a common data structure. This common data structure then allows to process and visualize the different datasets in a coherent manner, thus facilitating comparisons and interpretations. 
In addition, the GMG provides tools to assist numerical modellers in creating input models from these datasets.

For this, the julia module GeophysicalModelGenerator.jl provides the following functionality:

- A consistent GeoData structure, that holds the data along with lon/lat/depth information.
- Routines to generate VTK files from the GeoData structure in order to visualize results in Paraview.
- The ability to deal with points, 2D profiles and 3D volumes, for both scalar and vector values.
- Rapidly import screenshots of published papers compare them with other data sets in 3D using paraview.
 - Create geodynamic input models (for LaMEM)

## 2. Installation
To install GeophysicalModelGenerator.jl, start julia and go to the package manager:
```julia
julia> ]
(@v1.6) pkg> add GeophysicalModelGenerator
```
This will automatically install various other packages it relies on (using the correct version).

If you want, you can test if it works on your machine by running the test suite in the package manager:
```julia
julia> ]
(@v1.6) pkg> test GeophysicalModelGenerator
```
Note that we run these tests automatically on Windows, Linux and Mac every time we add a new feature to GeophysicalModelGenerator (using different julia versions). This Continuous Integration (CI) ensures that new features do not break others in the package. The results can be seen [here](https://github.com/JuliaGeodynamics/GeophysicalModelGenerator.jl/actions).

The installation of `GMG` only needs to be done once, and will precompile the package and all other dependencies.

If you, at a later stage, want to upgrade to the latest version of `GMG`, you can type:
```julia
julia> ]
(@v1.6) pkg> update GeophysicalModelGenerator
```

You can load GeophysicalModelGenerator, for example to create cross-sections, with:
```julia
julia> using GeophysicalModelGenerator
```
## 3. Dependencies
We rely on a number of additional packages. All of them are automatically installed, except `GeoParams.jl`, which you currenty have to add yourself, again using the package manager.
- [GeoParams.jl](https://github.com/JuliaGeodynamics/GeoParams.jl) Defines dimensional units, and makes it easy to convert for km/s to m/s, etc.
- [WriteVTK.jl](https://github.com/jipolanco/WriteVTK.jl) writes VTK files (to be opened with Paraview).
- [ImageIO.jl](https://github.com/JuliaIO/ImageIO.jl), [FileIO.jl](https://github.com/JuliaIO/FileIO.jl), [Colors.jl](https://github.com/JuliaGraphics/Colors.jl) to import screenshots from papers.
- [Interpolations.jl](https://github.com/JuliaMath/Interpolations.jl) for interpolations (for example related to importing screenshots).

{{< cta cta_text="Previous: Intro" cta_link="../introjulia" >}} 
{{< cta cta_text="Next: Data Import" cta_link="../gmg_import" >}} 