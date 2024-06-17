---
widget: slider
weight: 44
active: true
headless: true

design:
  # Slide height is automatic unless you force a specific height (e.g. '400px')
  slide_height: '300px'
  is_fullscreen: false
  # Automatically transition through slides?
  loop: false
  # Duration of transition between slides (in ms)
  interval: 2000


content:
  slides:
    - title: "Geophysical Model Generator"
      content: "A toolkit in the julia programming language to import, process and visualize geophysical datasets."
      align: center
      link:
       icon: github
       icon_pack: fab
       text: Take a look!
       url: https://github.com/JuliaGeodynamics/GeophysicalModelGenerator.jl
      background:
        position: right
        color: '#666'
        brightness: 0.7
        media: GMG.png
    - title: "GeoParams"
      content: "A julia package to handle material parameters and nonlinear rheologies in geodynamic simulations."
      align: center
      link:
       icon: github
       icon_pack: fab
       text: Take a look!
       url: https://github.com/JuliaGeodynamics/GeoParams.jl
      background:
        position: right
        color: '#666'
        brightness: 0.8
        media: GeoParams.png
    - title: "MDOODZ7.0"
      content: "A 2D staggered grid MIC-FD code to model deformation in the Earth's crust and lithosphere. "
      align: center
      link:
       icon: github
       icon_pack: fab
       text: Take a look!
       url: https://github.com/tduretz/MDOODZ7.0
      background:
        position: right
        color: '#666'
        brightness: 0.7
        media: mdoodz.png
    - title: "LaMEM"
      content: LaMEM (Lithosphere and Mantle Evolution Model) is a software package to simulate 2D/3D geological and geomechanical processes.
      align: center
      link:
       icon: bitbucket
       icon_pack: fab
       text: Go to repository
       url: https://bitbucket.org/bkaus/lamem/src/master/
      background:
        position: right
        color: '#666'
        brightness: 0.7
        media: LaMEM_overview.png
    - title: "MVEP2"
      content: "A 2D MIC FE code to simulate mantle and lithosphere deformation."
      align: center
      link:
       icon: bitbucket
       icon_pack: fab
       text: Take a look!
       url: https://bitbucket.org/bkaus/mvep2/src/master/
      background:
        position: right
        color: '#666'
        brightness: 0.7
        media: strain_micro.png
    - title: "DEDLoc"
      content: "A hydro-thermo-mechanical pseudo-transient GPU code to model deep earthquake rupture (under development)."
      align: center
      link:
       icon: bitbucket
       icon_pack: fab
       text: Coming soon!
       url: ''
      background:
        position: right
        color: '#666'
        brightness: 0.7
        media: DucLoc.png
---
