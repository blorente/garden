---
publish: true
created: 2026-01-16T15:52:09.788+00:00
modified: 2026-06-17T15:37:25.361+01:00
---

## Buffers

Arrays of data in OpenGL memory.

### Vertex Buffers

- [[#Buffers]] that contain vertex positon data.
- Usually `[]float`.
- Positon buffers: Just contain the positions, with each three elements marking the position of a single vertex.
  - So, `len(buffer) == 3 * #ofvertices`
- They can contain more data, like normals, uv coords....

### Index Buffers

- [[#Buffers]] to store the order of vertices that conform polygons.
- In some models, a vertex can be shared by many polys. Therefore, it's more efficient to store the index order once, instead of repeating position (and possibly more) data.
- Usually `[]int`, where each three elements represent a polygon rendered in counter clockwise order.
  - so `index_buffer[1, 2, 3, 2, 3, 4]` contains two polys:
    - `v1, v2, v3`,
    - `v2, v3, v4`

### Vertex Buffer Objects (VBO)

- A buffer with some structured data.
  - Can represent vertex coordinates, indices...
  - You need to create the buffer, load it with data, and then bind it to a VBA.

### Vertex Array Objects (VAO)

- A series of pointers to VBOs.
  - Represent an object in your world that you want to render.
  - When rendering, you need to tell OpenGL which VAO it's currently rendering (binding it)

## Shaders

### Shader

- A program that runs on the GPU

### Vertex Shader

- A shader that applies to each vertex (the [[#Vertex Array Objects (VAO)]] is the input), and returns a series of positions.
- Can take vertex data as inputs, like vertex positions, normal maps, texture coordinates...

### Fragment Shader

- A [[#Shader]] that applies to each pixel on the screen.
- Is automatically passed the output of the vertex shader.

### \[Shader] Program

- A duple of [[#Vertex Shader]] and [[#Fragment Shader]], used to represent a single rendering pipeline (?).

### Uniforms

- A way to pass CPU-computed inputs to the [[#Shader]] without recompiling it or modifying the [[#Vertex Buffers]] in memory.

## Textures

### Render textures

- Just images that can be mapped to a model.
- Each [[#Vertex]] can have its own texture coordinate.

### Texture Coordinates

- Go from 0 to 1.
- (0,0) is the bottom left.

### Sampling

- Getting the texture color from the [[#Texture Coordinates]].

### Filtering

- Figuring out which pixel to [[#Sampling]] from a low-res texture.
- What happens if the texture is low resolution and the model big?
- You can use GL\_NEAREST to just pick the nearest pixel, or
- GL\_LINEAR to interpolate between neighbours.

### Mipmap

- If you want to apply the same texture to many many objects, but they can have wildly different sizes.
- You may want different versions (resolutions) of the same texture.
- OpenGL can generate these maps for you with `glGenerateMipmap`.
