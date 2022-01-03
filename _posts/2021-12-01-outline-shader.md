---
title: Outline Shader
layout: post
post-image: "/assets/images/shaders/Outline/outline_shader.gif"
subtitle: 
description: A basic outline shader created for Goya's Nightmare videogame
link: https://github.com/FrancPS/GoyaNightmare/blob/main/GoyaNightmare/Assets/Shaders/S_Outline.shader
link-name: Source Code
tags:
- Utils
- Unity
- HLSL
label:
- shader
---


The objective of this shader is to highlight an object when the player is facing it, to show that it's a pickable object. 

It is designed in Unity, as an image effect shader and it's written in HLSL language.

For this shader, we create another Pass in the SubShader section that we will apply before the normal one. The inputs are:
* Outline Value
* Outline Color

In the vertex function, we have to scale the geometry of the object with the Outline Value:
```cpp
float4 Outline(float4 vertexPos, float outValue)
{
    float4x4 scale = float4x4
        (
            1 + outValue, 0, 0, 0,
            0, 1 + outValue, 0, 0,
            0, 0, 1 + outValue, 0,
            0, 0, 0, 1 + outValue
        );

    return mul(scale, vertexPos);
}

v2f vert (appdata v)
{
    v2f o;
    float4 vertexPos = Outline(v.vertex, _OutValue);
    o.vertex = UnityObjectToClipPos(vertexPos);
    o.uv = v.uv;
    return o;
}
```
<br/>

And for the fragment function, we will send the Outline Color instead of the color of the object texture:
```cpp
fixed4 frag (v2f i) : SV_Target
{
    fixed4 col = tex2D(_MainTex, i.uv);
    return float4(_OutColor.r, _OutColor.g, _OutColor.b, col.a);
}
```
<br/>

With this shader, we will draw the same object twice, but the first object rendered will have an increase of size and a mono-color as a texture. The overlap between the two Passes will create the result achieved.

Finally, it's important to set the Render Queue from the material to 2501 (AlphaTest+51). Otherwise, the first Pass will be always visible. 

<br/>

You can find the code here: <a href="https://github.com/FrancPS/GoyaNightmare/blob/main/GoyaNightmare/Assets/Shaders/S_Outline.shader" target="_blank">Outline Shader</a>