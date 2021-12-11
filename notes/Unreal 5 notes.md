---
attachments: [Karis_Nanite_SIGGRAPH_Advances_2021_final.pdf]
tags: [Rendering]
title: Unreal 5 notes
created: '2021-12-11T08:48:26.247Z'
modified: '2021-12-11T08:52:34.312Z'
---

# Unreal 5 notes

## Possible mesh alternatives
- voxelization is a form of uniform resampling and uniform resampling means loss
- subdivision by definition is amplification only
- displacement can’t increase the genus of a surface
- projecting to normal or displacement maps is a form of uniform resampling
- points require hole filling, but it's impossible to know the difference between a small gap that should be there and a hole that should be filled for certain without extra connectivity data

## Nanite mesh virtualization
- group triangles into clusters and build a bounding box for each cluster then cull the clusters based on their bounds
- "What was visible last frame is very likely to be what is visible this frame." 1. Draw what was visible in the previous frame 2. Build hierarchical Z-buffer (HZB) from that 3. Test the HZB to determine what is visible now but wasn’t in the last frame and draw anything that’s new
- decouple visibility from materials, use pixel level visibility buffer (deferred renderer)
- In general the cost of rendering geometry should scale with screen resolution, not scene complexity. That means constant time in terms of scene complexity and constant time means 
level of detail
- do level of detail with clusters, find a cut of the tree that matches the desired LOD, in a view dependent way based on the screen space projected error of the cluster
- group clusters and force them to make the same LOD decision for a level, request data based on demand
- lock the shared boundary edges between clusters during simplification, alternate group boundaries from level to level by grouping different clusters
- edge collapsing decimation, optimize for minimal error in new positions and attributes, projected on screen to a number of pixels of error to determine which LOD to select so is foundational to both quality and efficiency

