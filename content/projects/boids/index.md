---
title: "Boids"
date: 2025-11-17
draft: false
summary: "A flock simulation based on Reynolds 'Boids' algorithm."
tags: ["C", "raylib", "CMake"]
params:
    links: 
        - GitHub: "https://github.com/harryjduke/boids"
---

{{< alert >}}
**Note:** This project is a work in progress.
{{< /alert >}}

A real-time flocking simulation implementing [Craig Reynold's boids algorithm](https://www.red3d.com/cwr/boids/). The flocking behaviour is produced through three simple rules:

- Separation
- Alignment
- Cohesion

The project uses the [raylib library](https://www.raylib.com/) for graphics rendering and input handling.
