# SAI2 File Format Specification

SAI2 is a File format used by Systemax PaintTool SAI version 2 - painting graphics software. Authors did not release any public description of the format. This is the "unofficial" specification.



## Data types

The file is a sequence of bytes. Following data types are used in this document:
- `uint4` - four bytes, interpreted as a 32-bit unsigned integer in Little Endian
- `sint4` - four bytes, interpreted as a 32-bit   signed integer in Little Endian
- `ascii` - a sequence of bytes, represents an ASCII text
- `asci4` - four bytes, interpreted as a four-byte ASCII string


# File Structure

SAI2 file consists of three parts: a 64-Byte header, a Chunk List, and a sequence of chunks.

## Header
    Size  Type    Value
    16    ascii   "SAI-CANVAS-TYPE0"
    4     uint4   flags (usually 0x100 or 0x1000100)
    4     uint4   W - document width
    4     uint4   H - document height
    4     uint4   unknown
    
    4     uint4   N - number of chunks
    4     uint4   unknown
    16    bytes   zeros
    4     uint4   0xff808080 (grey color)
    4     asci4   "norm" or "ver1"

## Chunk List (repeats N times, N x 16 Bytes)

    Size  Type    Value
    4     asci4   Chunk Type ("hist", "thum", "intg", "layr", "lpix", ...)
    4     uint4   Object ID
    4     uint4   Chunk Offset
    4     uint4   zero

## Chunks: N chunks

## "layr" Chunk (specifies a layer)

    Size  Type    Value
    4     asci4   "layr"
    4     uint4   Layer ID (same as Object ID in the Chunk List)
    8     bytes   zeros
    4     asci4   Layer Type: "norm", "fold", "text", "shap", "liwk"
    16    sint4   unknown (four signed integers, each can be negative)
    4     uint4   unknown
    4     uint4   C - tile count
    4     asci4   Blend Mode ("norm", "scrn", "over", "ddge", ...)
    4     uint4   Opacity (0 .. 100)
    4     uint4   Flags
    ----- Layer parameters (repeats, ended with a zero)
    4     asci4   Param Name  ("name", "frng", ....)
    4     uint4   Plen - parameter length
    Plen  bytes   Value of the parameter
    -----
    4     uint4   zero

The pixel data is stored in tiles. A tile is an image of 256 x 256 pixels.

For each layer ("layr") there are layer pixels (a "lpix" chunk with the same Object ID as the "layr" chunk) with C tiles (C specified in "layr").

There is an "integrated" (merged) image of the document inside the "intg" chunk: C = ceil(W/256) x ceil(H/256) tiles.

## "intg" / "lpix" Chunk (pixel data in tiles)

    Size  Type    Value
    4     asci4   "dpcm"     
    X     uint4   Tile lengths (an integer for each tile) - L1, L2, L3, L4, ...
    X     bytes   A sequence of compressed tiles (L1 Bytes for the first tile, L2 Bytes for the second tile, ...)
    

    
    

