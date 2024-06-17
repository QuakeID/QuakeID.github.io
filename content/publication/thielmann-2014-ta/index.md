---
title: "Discretization Errors in the Hybrid Finite Element Particle-in-cell Method"
date: 2014-01-01
publishDate: 2021-10-28T21:57:09.083113Z
authors: ["Marcel Thielmann", "Dave A May", "Boris J P Kaus"]
publication_types: ["2"]
abstract: "In computational geodynamics, the Finite Element (FE) method is frequently used. The method is attractive as it easily allows employment of body-fitted deformable meshes and a true free surface boundary condition. However, when a Lagrangian mesh is used, remeshing becomes necessary at large strains to avoid numerical inaccuracies (or even wrong results) due to severely distorted elements. For this reason, the FE method is oftentimes combined with the particle-in-cell (PIC) method, where particles are introduced which track history variables and store constitutive information. This implies that the respective material properties have to be interpolated from the particles to the inte- gration points of the finite elements. In numerical geodynamics, material parameters (in particular the viscosity) usually vary over a large range. This may be due to strongly temperature-dependent rheologies (which result in large but smooth viscosity variations) or material interfaces (which result in viscosity jumps). Here, we analyze the accuracy and convergence properties of velocity and pressure of the hybrid FE-PIC method in the presence of large viscosity variations. Standard interpolation schemes (arithmetic and harmonic) are compared to a more sophisticated interpolation scheme which is based on linear least squares interpolation for two types of elements (Q1 P0 and Q2 P_1 ). In the case of a smooth viscosity field, the accuracy and convergence is significantly improved by the new interpolation scheme. In the presence of viscosity jumps, the order of accuracy is strongly decreased."
featured: false
publication: "*Pure and Applied Geophysics*"
url_pdf: "http://link.springer.com/article/10.1007/s00024-014-0808-9"
---

