---
title: "Shadertoy: LRPN"
header: 
    overlay_image: /assets/images/posts/shadertoy-lrpn/LRPN.jpg
    show_overlay_excerpt: false
    actions: 
      - label: "SHADERTOY LINK"
        url: https://www.shadertoy.com/view/wlf3RH
classes: wide
link: https://www.shadertoy.com/view/wlf3RH
# tags: [PRNG,LRPN,Noise,Shadertoy]
# categories: [Links]
---

I posted a simple implementation of the [Local Random Phase Noise](https://www.shadertoy.com/view/wlf3RH) model on shadertoy.

$$ \text{LRPN}(x,y) = \sum_{i}^{N} w_{i} \; cos(2\pi F_0 (xcos(w_0)+ ysin(w_0)) ) $$