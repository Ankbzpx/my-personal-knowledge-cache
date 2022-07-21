
## Example

Mixing two cubemap texture and write result to another cubemap texture

### Shader

> BlendKernel.metal

```
#include <metal_stdlib>
using namespace metal;

kernel void
blendKernel(texturecube<half, access::read>  refTexture  [[texture(0)]],
            texturecube<half, access::read>  curTexture  [[texture(1)]],
            texturecube<half, access::write> outTexture [[texture(2)]],
            constant float& alpha [[buffer(0)]],
            uint2 gid [[thread_position_in_grid]])
{
    outTexture.write(mix(refTexture.read(gid, 0), curTexture.read(gid, 0), alpha), gid, 0);
    outTexture.write(mix(refTexture.read(gid, 1), curTexture.read(gid, 1), alpha), gid, 1);
    outTexture.write(mix(refTexture.read(gid, 2), curTexture.read(gid, 2), alpha), gid, 2);
    outTexture.write(mix(refTexture.read(gid, 3), curTexture.read(gid, 3), alpha), gid, 3);
    outTexture.write(mix(refTexture.read(gid, 4), curTexture.read(gid, 4), alpha), gid, 4);
    outTexture.write(mix(refTexture.read(gid, 5), curTexture.read(gid, 5), alpha), gid, 5);
}

```

### Usage

```
guard let commandQueue = device.makeCommandQueue() else { fatalError("Failed to create command queue") }
let defaultLibrary = device.makeDefaultLibrary()
let kernelFunction = defaultLibrary?.makeFunction(name: "blendKernel")
do {
    try computePipelineState =
    device.makeComputePipelineState(function: kernelFunction!)} catch {
        fatalError("Failed to create compute kernel pipeline state")
}

let envTextureDescriptor = MTLTextureDescriptor()
envTextureDescriptor.textureType = .typeCube
envTextureDescriptor.pixelFormat = .rgba16Float
envTextureDescriptor.width = width
envTextureDescriptor.height = height
envTextureDescriptor.depth = 1
envTextureDescriptor.arrayLength = 1
envTextureDescriptor.mipmapLevelCount = 9
envTextureDescriptor.sampleCount = 1
envTextureDescriptor.usage = [.shaderRead, .shaderWrite]

let refEnvTexture = self.device.makeTexture(descriptor: envTextureDescriptor)!
let curEnvTexture = self.device.makeTexture(descriptor: envTextureDescriptor)!
let outEnvTexture = self.device.makeTexture(descriptor: envTextureDescriptor)!


// Set the compute kernel's threadgroup size to 16 x 16.
threadgroupSize = MTLSize(width: 16, height: 16, depth: 1)
threadgroupCount = MTLSize()
// Calculate the number of rows and columns of threadgroups given the size of the
// input image. Ensure that the grid covers the entire image (or more).
threadgroupCount.width  = (envTextureDescriptor.width + threadgroupSize.width - 1)
                            / threadgroupSize.width
threadgroupCount.height = (envTextureDescriptor.height + threadgroupSize.height - 1)
                            / threadgroupSize.height
// The image data is 2D, so set depth to 1.
threadgroupCount.depth = envTextureDescriptor.depth


let commandBuffer = commandQueue.makeCommandBuffer()
let computeEncoder = commandBuffer?.makeComputeCommandEncoder()
computeEncoder?.setComputePipelineState(computePipelineState)
computeEncoder?.setTexture(refEnvTexture, index: 0)
computeEncoder?.setTexture(curEnvTexture, index: 1)
computeEncoder?.setTexture(outEnvTexture, index: 2)
computeEncoder?.setBytes(&alpha, length: MemoryLayout.size(ofValue: alpha), index: 0)
computeEncoder?.dispatchThreadgroups(threadgroupCount, threadsPerThreadgroup: threadgroupSize)
computeEncoder?.endEncoding()
let blitEncoder = commandBuffer?.makeBlitCommandEncoder()
blitEncoder?.generateMipmaps(for: outEnvTexture)
blitEncoder?.endEncoding()
commandBuffer?.commit()
```
