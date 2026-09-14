# Sisyphus Timer

Sisyphus Timer maps a stopwatch onto a continuous landscape. As time advances, the camera follows a boulder across the terrain; pausing the clock freezes the world with it.

[kivilcimlab.org/sisyphustimer](https://kivilcimlab.org/sisyphustimer)

## Time as distance

The horizontal world position is `elapsedSeconds × 130`. Terrain height comes from several low amplitude sine waves and a repeating twenty second profile. The second half of that profile descends through a cosine eased valley, holds briefly, then climbs back out.

The boulder does not use a fixed angular speed. Each frame measures the distance between its previous and current terrain positions and divides that arc length by the radius of 55 pixels. Its rotation therefore follows the surface even as the slope changes.

Reset returns elapsed time, boulder angle, and the camera to their initial state. The terrain itself is deterministic and does not regenerate.

## Rendering the landscape

The ground is built in three passes: the terrain profile, a second line offset along the surface normal, and sparse texture dots. Dot placement is derived from the world x coordinate, which keeps the texture fixed while the camera moves.

The entire scene is rotated by 18 degrees. Clouds are Bézier shapes with their own drift, bobbing, and parallax rates. The ALT dial is a stylised reading derived from the transformed world position rather than a physical altitude model.

Canvas dimensions account for device pixel ratio, while the world scale contracts on small screens. Space starts or pauses the timer, and R resets it.

## Implementation

The project uses JavaScript and Canvas 2D. `PhysicsEngine.js` defines terrain and altitude calculations. `Renderer.js` owns terrain texture, clouds, the boulder texture, and the dial. `main.js` manages time, input, camera movement, and the render loop.
