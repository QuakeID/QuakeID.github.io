---
title: Exporting Data
linktitle: Exporting Data
toc: true
type: docs
date: "2021-10-27T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Geophysical Model Generator
    weight: 5

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5
---

## 1. Exporting Data
Now that you know how to import data and how to extract cross sections from it, you may want to export it to a format that can be imported into e.g. Paraview. This has been done already at several instances during this tutorial, but we state it here again for completeness. Exporting data to *vts* format (which can be imported to Paraview) can be done using the function {{<hl>}} Write_Paraview{{</hl>}}.

Based on the previous part of the tutorial, you may want to export the whole block of the imported tomography. This is simply done using this command:
```julia-repl
julia> Write_Paraview(Data_Paffrath, "DataPaffrathAlps")    
````
If, on the other hand, you would like to export the vertical cross section you just created, it can be exported using:
```julia-repl
julia> Write_Paraview(Data_Paffrath_A, "DataPaffrath_ProfileA")    
````
whereas the horizontal slice can be exported with:
```julia-repl
julia> Write_Paraview(Data_Paffrath_d100, "DataPaffrath_Slice100")    
````

Where the first argument is the data structure to be exported and the second one denotes the vts filename.
That's it!

{{< spoiler text="**Exercises**" >}}
1. Export all volumes/cross sections that you created.
{{< /spoiler >}}

{{< cta cta_text="Previous: Creating Cross Sections "  cta_link="../gmg_cross" >}} 
{{< cta cta_text="Next: Visualization" cta_link="../gmg_vis" >}} 
