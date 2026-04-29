# Möbius ∞ City

An infinite city wrapped around a 3D figure-8 Möbius strip. Drive around it forever — the road never ends, and after two passes you're back where you started.

**Live demo:** https://jakobzhao.github.io/mobious/

## What it is

The world is laid out on a non-orientable ribbon shaped like a stretched figure-8. The ribbon has three half-twists, so it's a real Möbius strip topologically — both faces are actually the same surface, and walking the centerline once flips your orientation; twice restores it.

Both faces of the ribbon carry a continuous road, sidewalks, buildings on both curbs, trees, parked cars, lamp posts, mailboxes, fire hydrants, benches and shop awnings. The city density varies along the loop with a smooth `cityness(u)` field — denser near the figure-8 crossings, fading to park-like countryside at the lobe apexes.

Built with [Three.js](https://threejs.org/) + `OrbitControls`. Single static HTML file, no build step.

## Controls

| Key / Input | Action |
|---|---|
| Drag | Orbit the camera |
| Scroll | Zoom |
| `R` | Pause / resume auto-rotation |
| `F` | Toggle slow fly-through along the road |

## How the math works

The centerline is a 3D Lemniscate of Gerono with vertical lift, so the figure-8 doesn't self-intersect:

```
c(u) = (A·sin u,  H·cos u,  B·sin u·cos u)
```

A perpendicular frame `(B1, B2)` is built at each `u` by projecting the world up-axis through the tangent. The strip's cross-section direction rotates with the twist:

```
A(u) = cos(K·u/2)·B1 + sin(K·u/2)·B2
p(u, v) = c(u) + v·A(u)
```

For odd `K` the strip satisfies `p(u + 2π, v) = p(u, -v)` — the Möbius identification. The mesh closes the seam by wrapping `i = segU − 1` back to `i = 0` with `j` flipped, which keeps top/bottom faces, road markings, sidewalks and buildings continuous across the seam.

The ribbon has thickness, so its underside is genuinely opaque — looking from "the other side" you see the bottom face's road-and-cityscape, not a peek-through to the top.

## Run locally

The page uses ES module imports, so open it through a server, not `file://`:

```bash
python3 -m http.server 8765
# then open http://localhost:8765/
```

## Files

- [`index.html`](index.html) — everything: scene, geometry, parameterization, vertex-coloured road markings, procedural buildings/trees/furniture/cars, OrbitControls and fly-through.
