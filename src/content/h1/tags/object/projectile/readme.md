---
title: projectile
stub: true
about: 'tag:h1/projectile'
img: proj.jpg
caption: >-
  Projectiles can be given a render model and a variety of effects and
  attachments, like [contrails](~contrail) and [lights](~light).
thanks:
  gbMichelle: Movement
  Mimickal: Explaining bounce timer
  Kavawuvi: Invader tag definitions
  MosesOfEgypt: Tag structure research
---
**Projectiles** are special moving objects shot from weapons and thrown as grenades.

# Movement
Projectile movement is simulated during each game tick (smallest unit of simulated time). According to the projectile's velocity and gravity scale, its next position is calculated by the engine and a "trace" line between the two points is tested for collisions.

The trace collision test takes advantage of [objects'](~object) bounding radii and the collision BSP structures found in [model_collision_geometry](~) and [scenario_structure_bsp](~). If any collision is detected, it is handled accordingly (e.g. applying [effects](~effect) or playing [sounds](~sound)).

If no collision is detected, the projectile is moved to its next position at the end of the trace line. The process continues, tick by tick, until the projectile collides, detonates at its _maximum range_, or is removed at its maximum _air_ or _water damage range_.

A sufficiently high velocity projectile is essentially [hitscan][] if it can cross a playable space within a single tick, with the game being simulated at 30 ticks per second. Otherwise, ballistic leading will be required to hit a moving target.

[hitscan]: https://en.wikipedia.org/wiki/Hitscan

# Structure and fields

{% tagStruct "h1/projectile" /%}
