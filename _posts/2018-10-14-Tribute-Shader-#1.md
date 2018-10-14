---
title:  "Tribute Shader #1"
categories: [blog,devlog]
tags: [Tribute,Rendering,Gallery] 
gallery:
    - image_path: /assets/images/posts/tribute-shader-1/Resultat_1.PNG
      title: "Test 1"
    - image_path: /assets/images/posts/tribute-shader-1/Resultat_2.PNG
      title: "Test 2"
---

Today I publish some old screenshots of my first "true" raymarcher i code last year. This first blog post only contains screendshots and texts, i forgot where the source code is (lost on one of my HDDs).

The aim was to reproduce the "orange" atmosphere of Blade Runner 2049. I worked with a "goal frame" of the movie.  

{% include figure image_path="/assets/images/posts/tribute-shader-1/BladeRunnerSample.jpg" alt="Oups! Imaginez une silhouette arpentant une brume orangée" caption=" **Ohlala cette direction artistique** " %}


I first wrote a terrain marcher using a Fractional-Brownian-Motion (i point to you the marvelous Inigo Quilez's [blog post](https://www.iquilezles.org/www/articles/terrainmarching/terrainmarching.htm)). 
I added a distance-based fog using a color palette, trying to mimic the goal image. And then it gave :
{% include figure image_path="/assets/images/posts/tribute-shader-1/Terrain_plus_fog.PNG" alt="Oups!" caption="ColorPalette : Checked" %}

It felt a bit empty so I wanted to add a huge structure reminiscent of the Tyrell Corporation building.
{% include figure image_path="/assets/images/posts/tribute-shader-1/model_tyrell_building_behind_the_scene.jpg" alt="Oups!" caption="Behing the scene of Blade Runner, Tyrell Corporation building model" %}

I used simple distance primitives to draw it. Using Boxes, Pyramid and operations (Quilez's [Toolbox](https://www.iquilezles.org/www/articles/distfunctions/distfunctions.htm)).

I start with a "Inner" pyramid. I add the external structures by subtracting a box from a larger pyramid. I then create a cross shape with two boxes and erode the first pyramid. Finally I add a sphere at the center of the structure.

{% include figure image_path="/assets/images/posts/tribute-shader-1/strange_building.PNG" alt="Oups!" caption="CSV operations to the rescue" %} 

**Et voilà**
{% include gallery id="gallery" layout="half" %}

