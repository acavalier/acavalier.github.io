---
title: "Shadertoy: LRPN"
header: 
    overlay_image: /assets/images/posts/shadertoy-lrpn/LRPN.jpg
    show_overlay_excerpt: false
    actions: 
      - label: "SHADERTOY LINK"
        url: https://www.shadertoy.com/view/wlf3RH
classes: wide
# link: https://www.shadertoy.com/view/wlf3RH
# tags: [PRNG,LRPN,Noise,Shadertoy]
# categories: [Links]
---

I posted a simple implementation of the [Local Random Phase Noise](https://www.unilim.fr/pages_perso/guillaume.gilet/publications/pdf/ProcTextures.pdf) model on [shadertoy](https://www.shadertoy.com/view/wlf3RH).

<div align="center"><iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/wlf3RH?gui=true&t=10&paused=false&muted=false" allowfullscreen></iframe></div>

This noise model consists in a weighted sum of cosines :

$$ \text{LRPN}(\mathbf{x}) = \sum_{i}^{N} w(\mathbf{x}-\mathbf{x}_i) \sum_{j}^{M} A_{ij} cos(2\pi \mathbf{F_{ij}} \cdot \mathbf{x} + \phi_{ij}) $$

with : 

$$ F = \begin{pmatrix} F_0 cos(w_0) \\ F_0 sin(w_0) \end{pmatrix} $$