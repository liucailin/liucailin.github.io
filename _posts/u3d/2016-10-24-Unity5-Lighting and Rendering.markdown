---
layout: post
title: Unity5 Lighting and Rendering
---

Global illumination, or 'GI', is a term used to  describe a range of techniques and mathematical models which attempt to simulate the complex behaviour of light as it bounces and interacts with the world.

## CHOOSING A LGIHTING TECHNIQUE

`Two way of lighting -realtime or precomputed`

### REALTIME LIGHTING

Lights in unity - directional, spot and point are realtime by default, but light rays from realtime lights do not bounce.

### BAKED GI LIGHTING

'Lightmap' is static, can not change during gameplay, baked from static objects and the reusult are written to textures which are overlaid on the top of scene geometry.

The lightmap can include both direct light which strike a surface and 'indirect' light that bounces from other objects. This lighting texture can be used together whit surface information like color(albedo) and relief(normal) by the shader associated whit an object's material.

Realtime lights can be overlaid and used additively on top of a lightmapped scene.

### PRECOMUPTED REALTIME GI LIGHTING

Whilst backed lightmap are unable to react to changes in lighting, precomputed realtime GI does offer us a technique for updating lighting interactively.

A good example of this would be a time of day system.

To speed up the precompute further Unity dosn't directly work on lightmap texels, but instead create a low resoulution approximation of the static geometry in the world, called 'clusters'.

Unity use ray tracing to calculate the releationships between surface clusters beforehand - during the 'Light Transport' stage of the precomputed.

## THE PRECOMPUTED PROCESS

### ENABLEING BAKED GI OR PRECOMPUTED REALTIME GI

By default both are enabled. A good practise is to ensure onl one system is used at a time, by disabling the other globally, and any settings configured per-light will be overridden.

### PRE-LIGHT SETTINGS

The defalut baking mode for each light is 'Realtime'. This means that the selected light(s) will still contribute direct light to your scene, with indirect light handled by Unity's Precomputed Realtime GI system.

'Baked' light will contribute lighting solely to Unity's Baked GI system, both direct and indirect light will be baked into lightmaps ans cannot be changed during gameplay.

'Mixed' light still contribute direct realtime light to non-static GameObjects. Static GameObjects will include this light in their Baked GI lightmaps.

## SCENE SETUP

### CHOOSING A RENDERING PATH

 *|Forward Rendering | Deferred Rendering
  |----------------- | ------------------
advantage | fast        | good performance
bound by  | light count | pixels count

### CHOSSING A COLOR SPACE

(Edit>Project Settings>Player)

Linear or Gamma

## LIGHTING A SCENE

- __directional lights__
- __point lights__
- __spot lights__
- __area lights__
- __emissive materials__
- __light probes__  Only static objects are consider by Unity's Baked or Precomputed Realtime GI systems. In order for dynamic objects, we need to record this lighting information into a format which can be quickly read and used during gameplay.We do this by placing sample points in the world and then capturing light from all directions. The color information these points record is then encoded into a set of values (or ‘coefficients’) which can be quickly evaluated during gameplay. In Unity, we call these sample points, ‘Light Probes’.











