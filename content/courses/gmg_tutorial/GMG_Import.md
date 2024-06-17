---
title: Importing Data
linktitle: Importing Data
toc: true
type: docs
date: "2021-10-27T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Geophysical Model Generator
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## 1. Data sources
Before you can use GMG, you obviously need data. Given the different kinds of data available, there is a multitude of sources. Here, we list some of them that may be useful in the future:

- [MB4D Data Repository](https://dataservices.gfz-potsdam.de/4dmb/): Repository for data obtained within SPP 4DMB
- [ETOPO1](https://ngdc.noaa.gov/mgg/global/global.html): Global relief data with a resolution of 1 arc-minute
- [IRIS Earth Model Collaboration](http://ds.iris.edu/ds/products/emc-earthmodels/): Different Earth Models, in particular a large collection of seismic tomographies
- [our own database](https://seafile.rlp.net/d/22b0fb85550240758552/): a collection of different data which we have already processed with GMG
- and many more...

A lot of data is also not available via dedicated repositories, but via more general repositories such as Zenodo or even hosted on private websites. For the purpose of this tutorial, we will now focus on data from seismic tomographies. For the European Alps, there are several datasets which are freely available. Here, we will use the dataset by *Paffrath et al.* which uses AlpArray data. The data is not yet available for download in a public repository, but will soon be. Here we use a simple text file with the data that was provided to us directly by Marcel Paffrath. 

Paffrath, M., Friederich, W., and the AlpArray and AlpArray-Swath D working group: *Imaging structure and geometry of slabs in the greater Alpine area – A P-wave traveltime tomography using AlpArray Seismic Network data*, Solid Earth Discuss. [preprint], https://doi.org/10.5194/se-2021-58, in review, 2021.

## 2. Importing tomographic data given as ASCII file
To start importing data, we start the `Julia REPL` as described in the [first part](../introjulia) of the tutorial. We then load the DelimitedFiles and the GeophysicalModelGenerator packages:
```julia-repl
julia> using DelimitedFiles, GeophysicalModelGenerator, GeoStats
```
To import the Paffrath dataset, read it using the function *readdlm*:

```julia-repl
julia> data = readdlm("./vgrid_it12_w_res.txt",skipstart=1)
```
This results in a large matrix where the data is stored. As a next step, we will organize this data a bit more:
```julia-repl
julia> lon    =  data[:,1];
julia> lat    =  data[:,2];
julia> depth  = -data[:,3];
julia> Vp     =  data[:,4];
julia> dVp    =  data[:,5];
```
To process this data, we still have to transfer it to a matrix format. To do so, we first generate a regular 3D grid:
```julia-repl
Depth_vec       =   unique(depth)
Lon,Lat,Depth   =   LonLatDepthGrid(0:0.5:21,37:0.25:51,Depth_vec);
```
Here, we generate a grid that has a longitude range from 0° to 20°, a latitude range from 37° to 21° and a depth range determined by the data. 

To transfer the data in matrix format, we then employ the *GeoStats.jl* package, which basically interpolates the data from the given data points to the grid:

```julia-repl
julia> dLon = Lon[2,1,1]-Lon[1,1,1]
julia> dLat = Lat[1,2,1]-Lat[1,1,1]
julia> Cgrid = CartesianGrid((size(Lon,1),size(Lon,2)),(minimum(Lon),minimum(Lat)),(dLon,dLat))
julia> Vp_3D = zeros(size(Depth));
julia> dVp_3D = zeros(size(Depth));

julia> for iz=1:size(Depth,3)
julia>     println("Depth = $(Depth[1,1,iz])")
julia>     ind   = findall(x -> x==Depth[1,1,iz], depth)
julia>     coord = PointSet([lon[ind]'; lat[ind]'])
julia>     Geo   = georef((Vp=Vp[ind],), coord)
julia>     P     = EstimationProblem(Geo, Cgrid, :Vp)
julia>     S     = IDW(:Vp => (distance=Euclidean(),neighbors=2)); 
julia>     sol   = solve(P, S)
julia>     sol_Vp= values(sol).Vp
julia>     Vp_2D = reshape(sol_Vp, size(domain(sol)))
julia>     Vp_3D[:,:,iz] = Vp_2D;

julia>     # dVp
julia>     Geo   = georef((dVp=dVp[ind],), coord)
julia>     P     = EstimationProblem(Geo, Cgrid, :dVp)
julia>     S     = IDW(:dVp => (distance=Euclidean(),neighbors=2)); 
julia>     sol   = solve(P, S)
julia>     sol_dVp= values(sol).dVp
julia>     dVp_2D = reshape(sol_dVp, size(domain(sol)))
julia>     dVp_3D[:,:,iz] = dVp_2D;
julia> end
```
We have now interpolated both Vp as well as dVp to the 3D grid. What is now left is to put the data in the GMG format:

```julia-repl
julia> Data_Paffrath    =   GeophysicalModelGenerator.GeoData(Lon,Lat,Depth,(Vp_km_s=Vp_3D,dVp_perc=dVp_3D)) 
```

Now store the imported and interpolated data in GMG format so that you can work with it later. This is done using the [JLD2.jl](https://github.com/JuliaIO/JLD2.jl) package:

```julia
julia> using JLD2
julia> jldsave("Paffrath_Europe.jld2"; Data_Paffrath)
```

If you, at a later stage, want to load this file again do it as follows:
```julia
julia> using JLD2, GeophysicalModelGenerator
julia> Data_Paffrath_ = load_object("Paffrath_Europe.jld2")
```


You can also find the above workflow in this file: {{% staticref "uploads/gmg_tutorial/ReadDataPaffrath.jl" "newtab" %}}ReadDataPaffrath.jl{{% /staticref %}}. To run it, do the following:
```julia-repl
julia> include("ReadDataPaffrath.jl")
```
Make sure to put the data file vgrid_it12_w_res.txt in the same directory.

## 3. Importing tomographic data given as netCDF
As mentioned above, *netCDF* is a standardized file dormat that is widely used in geoscientifc applications. As an example, we will here import the velocity model of the *Collaborative Seismic Earth Model* (CSEM, *Fichtner et al., 2018*), which has been constructed using multi-scale full seismic waveform inversion. This model is available via the IRIS EMC Database and can be downloaded [here](http://ds.iris.edu/files/products/emc/emc-files/csem-europe-2019.12.01.nc). The original work behind this dataset is:

- Blom, N. and Gokhberg, A. and Fichtner, A., 202. *Seismic waveform tomography of the central and eastern Mediterranean upper mantle*. Solid Earth; Gottingen Vol. 11, Iss. 2, (2020): 669-690. https://doi.org/10.5194/se-11-669-2020
- Cubuk-Sabuncu, Y., Taymaz, T., Fichtner, A., 2017. *3-D crustal velocity structure of western Turkey: constraints from full-waveform tomography*, Physics of the Earth and Planetary Interiors 270, 90-112. https://doi.org/10.1016/j.pepi.2017.06.014
- Fichtner, A., Trampert, J., Cupillard, P., Saygin, E., Taymaz, T., Capdeville, Y., Villasenor, A., 2013. *Multi-scale full waveform inversion*. Geophysical Journal International 194, 534-556. https://doi.org/10.1093/gji/ggt118
- Fichtner, A., van Herwaarden, D.-P., Afanasiev, M., Simute, S., Krischer, L., Cubuk-Sabuncu, Y., Taymaz, T., Colli, L., Saygin, E., Villasenor, A., Trampert, J., Cupillard, P., Bunge, H.-P., Igel, H., 2018. *The Collaborative Seismic Earth Model: Generation I*. Geophysical Research Letters 45, https://doi.org/10.1029/2018GL077338.


To import this data, we will use the [https://github.com/JuliaGeo/NetCDF.jl](NetCDF.jl) package. 

```julia-repl
julia> using NetCDF, GeophysicalModelGenerator
```

 ```julia-repl
julia> using Pkg
julia> Pkg.add("NetCDF")
```
First, you can have a look at the contents of this file (assuming that you are in the same directory where the file is located):
```julia-repl
julia> using NetCDF
julia> ncinfo("csem-europe-2019.12.01.nc")
```
The output of this command is quite large and lists all variables stored in this file. The important part here is the part where the variables are listed:

```julia-repl
##### Variables #####

Name                                            Type                    Dimensions                     
-------------------------------------------------------------------------------------------------------------
depth                                           FLOAT                   depth                              
latitude                                        FLOAT                   latitude                           
longitude                                       FLOAT                   longitude                          
Vs                                              FLOAT                   longitude latitude depth           
```
Here we can see that there are four variables in this file, three of them (depth,latitude, longitude) having a single dimension and the fourth one (Vs) having dimensions of the three previous variables. The three one-dimensional vectors therefore define a regualr grid of coordinates defining the locations where Vs is stored.  
To load this data, we can now simply use the commmand *ncread*:  
```julia-repl
julia> lat = ncread("csem-europe-2019.12.01.nc","latitude")
julia> lon = ncread("csem-europe-2019.12.01.nc","longitude")
julia> depth = ncread("csem-europe-2019.12.01.nc","depth")
julia> Vsh_3D = ncread("csem-europe-2019.12.01.nc","vsh")
julia> Vsv_3D = ncread("csem-europe-2019.12.01.nc","vsv")
julia> depth = -1 .* depth 
```
Note that we multiplied depth with -1. This is necessary to make depth to be negative, as that is what `GeophysicalModelGenerator.jl` expects. 

#### 3. Reformat the coordinate data
In the netCDF file, coordinates are given as 1D vectors denoting the location of nodes in a regular grid. However, `GeophysicalModelGenerator.jl` expects true 3D data, where each data point is assigned a latitude,longitude, depth and the respective property (here: Vs). To generate this full regular 3D grid, do the following:
```julia-repl
Lon3D,Lat3D,Depth3D = LonLatDepthGrid(lon, lat, depth);
```
#### 4. Generate Paraview file
Once the 3D coordinate matrix has been generated, producing a Paraview file is done with the following command 
```julia-repl
julia> DataCSEM_Europe    =   GeoData(Lon3D,Lat3D,Depth3D,(Vsh_km_s=Vsh_3D,Vsv_km_s=Vsv_3D))   
```
Finally, export the data again to a Paraview compatible format and 
 ```julia 
julia> Write_Paraview(DataCSEM_Europe, "CSEM_Europe")
```
Besides exporting the data to Paraview, you may want to store the imported data in GMG format so that you can work with it later. This is done using the [JLD2.jl](https://github.com/JuliaIO/JLD2.jl) package:

```julia-repl
julia> using JLD2
julia> jldsave("CSEM_Europe.jld2"; DataCSEM_Europe)
```



{{< spoiler text="**Exercise**" >}}
1. Follow the instructions above and import the CSEM velocity model. Save it to both vts and jld2 format.
{{< /spoiler >}}


{{< cta cta_text="Previous: Package Description "  cta_link="../gmg_desc" >}} 
{{< cta cta_text="Next: Creating Cross Sections "  cta_link="../gmg_cross" >}} 
