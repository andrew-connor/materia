# Changelog

All notable changes to this project will be documented in this file.

The format is loosely based on [Keep a Changelog](https://keepachangelog.com/).

## Initial release

First public version of Materia.

- Two synchronized 3D views: **Form** (object colored by real material) and **Composition** (materials as volume-scaled stacked cubes).
- 26 objects across four categories: Vehicles & Craft, Buildings & Monuments, Living Things, and Household.
- **Enclosed Volume** readout computed from object geometry via the divergence theorem.
- **Total Material Volume** and a per-material breakdown with volumes and percentages.
- Real material densities, so volumes map to physically meaningful masses.
- Camera-lock, unit (m³ / ft³), and dimension-line toggles.
- Single self-contained HTML file; Three.js loaded from a CDN with no build step.
