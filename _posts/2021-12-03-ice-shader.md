---
title: Volumetric Ice
layout: post
post-image: "/assets/images/shaders/Volumetric_ice/volumetric_ice.gif"
subtitle: ""
description: Material simulating volumetric ice
link: ""
link-name: ""
tags:
- Volumetric
- Unreal
- Shader Graph
label:
- shader
---


This shader simulates volumetric ice. Two different parameters can be changed to achieve the desired result:
* The Surface Texture is a normal map that recreates the texture in the surface of the material.
* The depth factor allows you to increase the depth of the ice texture related to the surface.

<br/>
<img src="/assets/images/shaders/Volumetric_ice/shader_graph.png" alt="Shader Graph" width="1200"/>
<br/>

The following video is a demo with different values for each of the parameters described:
<br/>

<video width="1080" height="720" controls>
  <source src="/assets/images/shaders/Volumetric_ice/volumetric_ice.mp4" type="video/mp4">
</video>

