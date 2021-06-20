---
tags: [Rendering]
title: Android GLES draw bitmap
created: '2021-06-20T07:40:06.543Z'
modified: '2021-06-20T09:36:38.924Z'
---

# Android GLES draw bitmap

```
// Vertex Shader
val vertexShaderCode =
        "attribute vec4 vPosition;" +
            "attribute vec4 vInputTextureCoordinate;" +
            "varying vec2 vTextureCoordinate;" +
            "void main() {" +
            "    gl_Position = vPosition;" +
            "    vTextureCoordinate = vInputTextureCoordinate.xy;" +
            "}"

// Fragment Shader
val fragmentShaderCode =
        "varying highp vec2 vTextureCoordinate;" +
            "uniform sampler2D logoFrame;" +
            "void main() {" +
            "    gl_FragColor = texture2D(logoFrame, vTextureCoordinate);" +
            "}"

fun loadShader(type: Int, shaderCode: String): Int {
        return GLES20.glCreateShader(type).also { shader ->
            GLES20.glShaderSource(shader, shaderCode)
            GLES20.glCompileShader(shader)
        }
    }

// const
private const val COORDS_PER_VERTEX = 3
private const val COORDS_PER_TEXTURE = 2
private const val DRAW_COUNT = 6
private const val BYTES_PER_FLOAT = 4
private const val BYTES_PER_INT = 4
private const val BYTES_PER_COORDS = 2

// bitmap to draw
val Bitmap: Bitmap

// texture for bitmap
val textureHandle: IntBuffer.allocate(1)

// OpenGL texture coordinate
//
// (0, 1) ---- (1, 1)
//    |          |
//    |          |
//    |          |
// (0, 0) ---- (1, 0)

val squareCoords = floatArrayOf(
        buttonLeftX, topRightY, 0.0f, // top left
        buttonLeftX, buttonLeftY, 0.0f, // bottom left
        topRightX, buttonLeftY, 0.0f, // bottom right
        topRightX, topRightY, 0.0f // top right
    )

val quadVertexHandle = ByteBuffer.allocateDirect(squareCoords.size * BYTES_PER_FLOAT).run {
        order(ByteOrder.nativeOrder())
        asFloatBuffer().apply {
            put(squareCoords)
            position(0)
        }
    }

val drawOrderHandle: ShortBuffer
    by lazy {
        val drawOrder = shortArrayOf(0, 1, 2, 0, 2, 3)
        ByteBuffer.allocateDirect(drawOrder.size * BYTES_PER_COORDS).run {
            order(ByteOrder.nativeOrder())
            asShortBuffer().apply {
                put(drawOrder)
                position(0)
            }
        }
    }


// create gl program
var program = GLES20.glCreateProgram().also {
        GLES20.glAttachShader(it, loadShader(GLES20.GL_VERTEX_SHADER, vertexShaderCode))
        GLES20.glAttachShader(it, loadShader(GLES20.GL_FRAGMENT_SHADER, fragmentShaderCode))
        GLES20.glLinkProgram(it)
    }

// load bitmap into texture
GLES20.glGenTextures(1, textureHandle)
GLES20.glBindTexture(GLES20.GL_TEXTURE_2D, textureHandle[0])
GLES20.glTexParameteri(GLES20.GL_TEXTURE_2D, GLES20.GL_TEXTURE_MIN_FILTER, GLES20.GL_LINEAR)
GLES20.glTexParameteri(GLES20.GL_TEXTURE_2D, GLES20.GL_TEXTURE_MAG_FILTER, GLES20.GL_LINEAR)
GLUtils.texImage2D(GLES20.GL_TEXTURE_2D, 0, GLES20.GL_RGBA, bitmap, 0)

// draw
GLES20.glUseProgram(program)
val vertexPosHandle = GLES20.glGetAttribLocation(program, "vPosition")
GLES20.glEnableVertexAttribArray(vertexPosHandle)
GLES20.glVertexAttribPointer(vertexPosHandle, COORDS_PER_VERTEX, GLES20.GL_FLOAT, false, COORDS_PER_VERTEX * 4, quadVertexHandle)

val texturePosHandle = GLES20.glGetAttribLocation(program, "vInputTextureCoordinate")
GLES20.glEnableVertexAttribArray(texturePosHandle)
GLES20.glVertexAttribPointer(texturePosHandle, COORDS_PER_TEXTURE, GLES20.GL_FLOAT, false, COORDS_PER_TEXTURE * 4, textureHandle)

GLES20.glEnable(GLES20.GL_BLEND)
GLES20.glBlendFunc(GLES20.GL_ONE, GLES20.GL_ONE_MINUS_SRC_ALPHA)

val logoHandle = GLES20.glGetUniformLocation(program, "logoFrame")
GLES20.glActiveTexture(GLES20.GL_TEXTURE0)
GLES20.glBindTexture(GLES20.GL_TEXTURE_2D, textureHandle[0])
GLES20.glUniform1i(logoHandle, 0)

GLES20.glDrawElements(GLES20.GL_TRIANGLES, DRAW_COUNT, GLES20.GL_UNSIGNED_SHORT, drawOrderHandle)

GLES20.glDisableVertexAttribArray(vertexPosHandle)
GLES20.glDisableVertexAttribArray(texturePosHandle)
GLES20.glDisablbe(GLES20.GL_BLEND)

// release
GLES20.glDeleteTextures(1, textureHandle)
GLES20.glDeleteProgram(program)
```
