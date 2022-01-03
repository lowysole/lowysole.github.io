---
title: Distortion Shader
layout: post
post-image: "/assets/images/shaders/Distortion/distortion_main.gif"
subtitle: 
description: A basic distortion shader created for Goya's Nightmare videogame
link: https://github.com/FrancPS/GoyaNightmare/blob/main/GoyaNightmare/Assets/Shaders/S_PostProcess.shader
link-name: Source Code
tags:
- Distortion
- Unity
- HLSL
label:
- shader
---


The objective of this shader is to distort the camera screen when an enemy is close to you or facing directly to your forward view direction, getting more distortion when the action time is increasing. 
With it, we simulate the remaining life points you have, getting a fully dark screen when you are dead.

It is designed in Unity, as an image effect shader. It's written in HLSL language, modifying the fragment function.

To develop this shader, we use different inputs:
* **Camera output screen**<br/>
  * We take the final render image from the camera and we apply the filter using the function:<br/>

  ```cpp  
  private void OnRenderImage(RenderTexture source, RenderTexture destination) { 
        Graphics.Blit(source, destination, material);
  }
  ```      
* **Noise texture**<br/>
  * A noisy texture to modify the UV coordinates from the camera image. We will use a random noise based on two different patterns to gain more randomness, using the RGB channels of the texture. 
<p align="center">
  <img src="/assets/images/shaders/Distortion/noise_texture.gif" alt="NoiseTexture" width="250"/>
  <img src="/assets/images/shaders/Distortion/noise_code.png" alt="NoiseCode" width="550"/>
</p>

* **Distortion and darkness**<br/>
  * A float to modify the factor of distortion and darkness of the shader.

<p align="center">
  <img src="/assets/images/shaders/Distortion/distortion_factor.gif" alt="DistortionFactor" width="600"/>
  <img src="/assets/images/shaders/Distortion/darkness_factor.gif" alt="DarknessFactor" width="600"/>
</p>

<br/>

You can find the code here: <a href="https://github.com/FrancPS/GoyaNightmare/blob/main/GoyaNightmare/Assets/Shaders/S_PostProcess.shader" target="_blank">Distortion Shader</a>
