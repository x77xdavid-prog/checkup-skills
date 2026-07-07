---
name: three-3d-web
description: Use when building 3D / WebGL / immersive web pages — Three.js, React Three Fiber (R3F), @react-three/drei, GLTF/GLB models, shaders (GLSL), Spline, product configurators, 3D hero sections — including load performance, mobile fallbacks, and reduced-motion. NOT for 2D scroll animation (that is GSAP).
---

# 3D Web (Three.js / R3F / WebGL)

## Overview
3D on the web is a **performance and fallback problem first, a visual problem second**. A gorgeous scene that drops to 8fps on a phone or blocks first paint for 6s is a failure. Budget the frame and the bytes before adding polish. Default stack: React Three Fiber + drei; raw Three.js only if not in React.

## When to Use
- 3D hero, product viewer/configurator, interactive 3D landing
- Loading `.glb`/`.gltf` models, environment maps, custom GLSL shaders
- Spline embeds, `@react-three/fiber`, `@react-three/drei`, `@react-three/postprocessing`

## Minimal R3F pattern
```jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, useGLTF, Environment } from '@react-three/drei'
import { Suspense, lazy } from 'react'

function Model() {
  const { scene } = useGLTF('/model-draco.glb') // draco-compressed
  return <primitive object={scene} />
}
export default function Scene() {
  return (
    <Canvas dpr={[1, 2]} camera={{ position: [0, 0, 5] }}>
      <Suspense fallback={null}>
        <Environment preset="city" />
        <Model />
        <OrbitControls enablePan={false} />
      </Suspense>
    </Canvas>
  )
}
```
Preload: `useGLTF.preload('/model-draco.glb')`.

## Performance budget
| Lever | Target |
|-------|--------|
| Draw calls | Keep low; merge geometry, use instancing for repeats |
| Poly count | Decimate models; web ≠ film. <100k tris for hero. |
| Textures | Compress (KTX2/basis), power-of-two, no 4K on mobile |
| GLB size | Draco/meshopt compress; lazy-load, never block LCP |
| `dpr` | Cap at `[1, 2]`; full retina DPR murders fill rate |
| Postprocessing | Expensive — measure; skip on low-end |

## Loading & code-split (critical for CWV)
Three.js is heavy (~150kb+). Dynamically import so it never blocks the initial page:
```js
const Scene = lazy(() => import('./Scene'))
```
Show a poster image / static render until the canvas is ready. Never let the 3D bundle be render-blocking for the hero.

## Fallbacks (required)
- **Reduced motion:** honor `prefers-reduced-motion` → static image or no auto-rotate.
- **No WebGL / low-end:** feature-detect, serve a rendered still.
- **Mobile:** lower dpr, smaller textures, fewer lights; consider static image entirely.

## Common Mistakes
- Loading a 40MB uncompressed GLB → compress with draco/meshopt, target <2–3MB.
- Animating in a `useFrame` that allocates objects every frame → allocate once, mutate.
- Full device pixel ratio on retina → fill-rate collapse; cap dpr.
- No `Suspense`/loading state → blank frozen canvas while model downloads.
- Shipping Three.js in the main bundle → dynamic import it.
- Ignoring `dispose()` on route change → GPU memory leak in SPAs.
- Using 3D where a video or CSS would do → 3D only when interaction/depth is the point.
