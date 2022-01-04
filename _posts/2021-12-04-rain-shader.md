---
title: Rain Drop Effect
layout: post
post-image: "/assets/images/shaders/Rain/rain_shader.gif"
subtitle: 
description: Complete rain shader with drops, drips, ripples and puddles
link: 
link-name: 
tags:
- Rain 
- Unreal
- Shader Graph
label:
- shader
---


This shader simulates the effect of rain impacting a surface. It includes drops, ripples, puddles, and drips. It's based on <a href="http://www.bencloward.com/" target="_blank">Ben Cloward's</a> work.

The main technique we use in this shader consists of adding or modifying normals to simulate all the effects of the rain. But, we also need to apply some effects to the base texture to simulate the moisture of the surface:

### Wetness
The key to this effect goes to saturation: wet materials go darker and more saturated and porous surfaces get more smooth and less specular, so we need to modify the roughness and specular values. 
To achieve this, we take the base texture and we lerp between the original texture and the one saturated. The custom values for rain could be 0.07 for roughness and 0.3 for specular (normally is 0.5). The factor for both lerps will be the combination of the % of wetness and porousness we want to achieve.

<p align="center">
  <img src="/assets/images/shaders/Rain/wetness.gif" alt="Wetness" width="620"/>
  <img src="/assets/images/shaders/Rain/wetness_graph.png" alt="Wetness Graph" width="800"/>
</p>

<br/>

### Drops


For the drops, we have created a texture with different information in each channel. The RG channels have the drop normals. The B channel contains a texture that controls when the drop will appear depending on the color it has. Finally, the A channel contains which drops are static (will appear always) and which drops are dynamic (will be affected by the timer texture from B channel): 

<p align="center">
  <img src="/assets/images/shaders/Rain/drops_texture.png" alt="Drops Texture" width="1000"/>
</p>

For the drop normal values (RG channels), we adapt the size and multiply the result with the output mask before sending it to the output channel. This output mask contains the combination of the static drops, the dynamic drops that will appear in that moment of time and a filter composed by the boolean indicating if we want rain and the Vertex Normal vector in World Space to discard all the surfaces we don't want to apply this effect:

<p align="center">
  <img src="/assets/images/shaders/Rain/drops.gif" alt="Drops" width="675"/>
  <img src="/assets/images/shaders/Rain/drops_graph.png" alt="Drops Graph" width="900"/>
</p>


### Wind Ripples

The wind ripples are the combination of two noisy textures (normals using RG channels) that are moved in two different directions. We can add a boolean to activate or deactivate the wind effect:
<p align="center">
  <img src="/assets/images/shaders/Rain/wind_ripples.gif" alt="Wind Ripples" width="475"/>
  <img src="/assets/images/shaders/Rain/wind_ripples_graph.png" alt="Wind Ripples Graph" width="1000"/>
</p>

### Water ripples

For the water ripples, we define three input parameters: weight, time, and input UVs. The texture sample is composed of a texture delimiting the zone of the ripple (R) and the normals (GB). Taking the normals with the input UVs, we can modify the result with the other two input parameters. The weight will control the intensity of the effect. The time, together with a sinusoid, will create the sense of movement of the ripple. Assigning different weights, timers, and UVs values to the single water ripple and combining them, we can achieve this effect:

<p align="center">
  <img src="/assets/images/shaders/Rain/water_ripples.gif" alt="Water Ripples" width="485"/>
  <img src="/assets/images/shaders/Rain/water_ripples_graph.png" alt="Water Ripples Graph" width="1200"/>
</p>


### Puddles

The puddles are zones of the material where the water is in movement and has more depth. These zones can be set with another texture. In our case, we make a noise texture and we apply different operations to adjust the size of those zones inside the material. But we can enrich this effect by combining the water and wind ripples explained before to get a more realistic effect: 

<p align="center">
  <img src="/assets/images/shaders/Rain/puddles.gif" alt="Puddles" width="610"/>
  <img src="/assets/images/shaders/Rain/puddles_graph.png" alt="Puddles Graph" width="1000"/>
</p>


<br/>
<br/>
<br/>

Finally, mixing in the same material all of the effects we have explained before, we get a cool rain effect for surfaces:

<p align="center">
  <img src="/assets/images/shaders/Rain/rain_shader.gif" alt="Wetness" width="1000"/>
</p>