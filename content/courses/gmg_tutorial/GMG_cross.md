---
title: Creating Cross Sections
linktitle: Creating Cross Sections
toc: true
type: docs
date: "2021-10-27T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Geophysical Model Generator
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---
In scientific publications that deal with seismic tomographies, the results are often shown in 2D horizontal and vertical cross sections (slices and profiles). The *Geophysical Model Generator* therefore also provides tools do exactly this. 

We will use the data that we have previously imported to show how to create cross sections. If you haven't closed julia, these datasets should still be in memory, otherwise you have to load the corresponding jld2 file:

```julia-repl
julia> load("Paffrath_Europe.jld2")
```
## 1. Creating horizontal cross sections (slices)
Once you have loaded the required data, creating a horizontal slice is quite easy using the GMG tool {{<hl>}}CrossSection{{</hl>}}. To extract a horizontal slice at a given depth, simply call the function as in the example below:

```julia-repl
julia> Data_Paffrath_d100  =   CrossSection(Data_Paffrath, Depth_level=-100km)  
```
where the first argument to the function is the GMG data structure and the second argument denotes the kind of cross section to be extracted. The key word *Depth_level* denotes that a horizontal slice is to be extracted, with its depth value being -100 km (note that depth is negative).
## 2. Creating vertical cross sections (profiles)
Creating a vertical cross section is similarly straightforward, the only difference being that in this case, the starting and ending point of the cross section has to be provided:
```julia-repl
julia> Data_Paffrath_A  =  CrossSection(Data_Paffrath, Start=(4.65,45.73), End=(17.23,43.80))
```
Where the start and end points have to be given as (longitude,latitude) pairs.
{{< spoiler text="**Exercises**" >}}
1. Create cross-sections for all three profiles given below. 
2. If you have time, have a look for horizontal slices in {{% staticref "uploads/gmg_tutorial/Lippitsch2003.pdf" "newtab" %}}Lippitsch et al. (2003){{% /staticref %}}.
3. If you have additional time, , do the same for the P-wave tomography by Koulakov et al. 

| Profile ID | lon start  | lat start  | lon end | lat  end | Comment | 
|------------|-------|-------|-----|-----|-----------|
A|   4.65|45.73|17.23| 43.80|   Lippitsch 2002 & 2003 (location corrected) |
B|   5.51|51.53|12.04|43.68|   Lippitsch 2002 & 2003  (location corrected) |
C|   17.78|   50.95|11.66|43.68|   Lippitsch 2002 & 2003 | (location corrected) |

{{< /spoiler >}}

{{< cta cta_text="Previous: Data Import" cta_link="../gmg_import" >}} 
{{< cta cta_text="Next: Exporting data" cta_link="../gmg_export" >}} 




