# PET

The PET \(Puppet\) File Format is Pangya's 3D model related file format. These files contain data required to display models in Pangya, including texture information, animation, etc.

## File Types

Puppet files exist in many different variations, which usually contain the same set of structures, the variations can easily be denoted by their file extensions:

* `pet`: Regular Puppet File
* `apet`: Animation Puppet File
* `bpet`: Bone Puppet File
* `mpet`: Motion Puppet File

## File Structure

### Sections

The general Puppet file structure is relatively simple. It's made up of different sections, for each key part of the 3D model. These sections are presented with a FourCC and an unsigned integer representing the length of the section.

```csharp
struct PETSectionHeader 
{
  string Name;
  uint Length;
}
```

Puppet files contain following sections:

* `VERS`: Version
* `TEXT`: Textures
* `SMTL`: Specular Material
* `BONE`: Bones
* `ANIM`: Animation
* `MESH`: Mesh
* `FANM`: Face Animation
* `FRAM`: Frames
* `MOTI`: Motions
* `COLL`: Collision Boxes

### `VERS` - Version

The `VERS` structure denotes the version of the file.

```csharp
struct Version
{
  byte Minor;
  byte Major;
}
```

Following version of Puppet files are known of:

* 1.0: Every Puppet without a `VERS` section.
* 1.1
* 1.2
* 1.3

Different versions of Puppet files include small changes across some of the sections which need to be respected for proper parsing.

### `TEXT` - Textures

The `TEXT` section denotes the used textures by the model. An unsigned integer at the beginning of this section tells us how many textures are used.

```csharp
struct Texture
{
  string TextureName;
}
```

There is a lot of data behind the file name of the texture, which currently does not appear to make much sense in our eyes. To properly parse multiple textures, read in 44 bytes per texture and split at the first null-byte to get the texture name!

### `SMTL` - Specular Material

This section is highly speculative, as we only have the name, which we assume to be specular material, as the game uses those quite frequently.

The structure is mostly empty, so it could be that the mere existance inside a model is a reference to a specular material being used, like a boolean flag.

### `BONE` - Bones

This section contains informations about the bones of the 3D model. The first byte after the section size determines the amount of bones present. Then following structure can be read repeatedly:

```csharp
struct Bone
{
  string Name; // null-byte delimited string
  byte Parent;
  float[12] Matrix;
}
```

The 12-float Matrix is based on the perspective-projection matrix, technically.

A speciality about this model is that in version 1.3, there is an unknown float value to be read after the matrix.

### `ANIM` - Animation

This section contains data for the animation of the model. It contains several different structures.

```csharp
struct Animation
{
  byte BoneID;
  PositionData[] PositionData;
  RotationData[] RotationData;
  ScalingData[] ScalingData;
  AnimationFlag[] AnimationFlag;
}
```

The first byte of the animation structure is the ID of the Bone we are currently animating. If the Bone ID read is `255` you should stop reading animations.

**Positional Data**

A unsigned integer determines the amount of positional data the bone has, for that amount, you then read following structure:

```csharp
struct PositionData
{
  float Time;
  float X;
  float Y;
  float Z;
}
```

Positional data tells us the coordinates a bone moves to at `Time`.

**Rotational Data**

A unsigned integer determines the amount of rotational data the bone has, for that amount you then read following structure:

```csharp
struct RotationData
{
  float X;
  float Y;
  float Z;
  float W;
  float Time;
}
```

Rotational data is a Quaternion telling us about the rotation of the bone at `Time`.

The reading order changes with versions. The above displayed structure is the reading order at version 1.0. In version 1.2 and above, the reading order is `Time`, `X`, `Y`, `Z`, `W` instead.

In version 1.0, after the amount of rotational data, there is another unsigned integer to be read. This is not present in all following versions.

**Scaling Data \(only version 1.2 and above\)**

A unsigned integer determines the amount of scaling data the bone has, for that amount you then read  following structure:

```csharp
struct ScalingData
{
  float Time;
  float X;
  float Y;
  float Z;
}
```

Scaling data tells us the scale of the axis of a bone at `Time`.

**Animation Flag \(only version 1.3\)**

A unsigned integer determines the amount of animation flags the bone has, for that amount you then read following structure:

```csharp
struct AnimationFlag
{
  float Time;
  float Value;
}
```

This structure is only present in the latest models, and the `Value` tied to `Time` is `-1` in every single model, so it's not clear what the exact purpose of this is yet.

### `MESH` - Mesh

This section contains data for the mesh of the model, it contains several different structures.

```csharp
struct Mesh
{
  Vertex[] Vertices;
  Polygon[] Polygons;
}
```

**Vertices**

A unsigned integer determines the amount of vertices the mesh has, for this amount you read following structure:

```csharp
struct Vertex
{
  float X;
  float Y;
  float Z;
  BoneInformation[] BoneInformation;
}
```

**BoneInformation**

`BoneInformation` is part of `Vertex` and determines the attachment to a specific bone and weight to it.

Following structure is read until the total weight of all bones equals `255`:

```csharp
struct BoneInformation
{
  byte Weight;
  byte BoneID;
}
```

**Polygons**

A unsigned integer determines the amount of polygons the mesh has, for this amount you read the following structure:

```csharp
struct Polygon
{
  PolygonIndex[3] PolygonIndices;
  byte TextureIndex;
}
```

**Polygon Indices**

Each `Polygon` has 3 polygon indices determining the corner positions of the polygon. Read following structure 3 times:

```csharp
struct PolygonIndex
{
  uint Index;
  float X;
  float Y;
  float Z;
  UVMapping[] UVMapping;
}
```

**UV Mapping**

Each `PolygonIndex` has U/V-information for texture projection on it. It has the following structure:

```csharp
struct UVMapping
{
  float U;
  float V;
}
```

In version 1.0 you can read this one single time right after the `Z` from `PolygonIndex`. In version 1.2 and above there is an additional byte which tells the amount of `UVMapping` structures to be read.

**Texture Index**

After all `Polygon`s and their `PolygonIndex` structures have been read in, there is the texture index. For the amount of `Polygon` structures you have, you can read a single byte which determines the `Texture` that should be used for that specific Polygon.

### `FANM` - Face Animations

This section contains information about face animations. It has not been parsed yet.

### `FRAM` - Frames

This section contains information for frame based animation sequences of models. A unsigned integer determines the amount of frames a model has, for that amount you then read following structure:

```csharp
struct Frame
{
  uint Index;
  string Script;
  float X;
  float Y;
  float Z;
}
```

The `Script` string has a `uint` ahead of itself which determines the length of the `Script` string that needs to be read.

### MOTI - Motions

This section contains information for groups of frame based animations called Motions. A unsigned integer determines the amount of motions a model has, for that amount you then read following structure:

```csharp
struct Motion
{
  string Name;
  uint FrameStart;
  uint FrameEnd;
  string TargetName;
  string TypeName;
  string BoneName;
}
```

Every `*Name` property has a unsigned integer ahead of itself which determines the length that needs to be read.

### `COLL` - Collision Boxes

This section contains information about the collision boxes a model has. A unsigned integer determines the amount of collision boxes a model has, for that amount you then read following structure:

```csharp
struct CollisionBox
{
  uint Unknown1;
  uint Unknown2;
  string Name;
  string BoneName;
  uint Unknown3;
  uint Unknown4;
  float MinX;
  float MinY;
  float MinZ;
  float MaxX;
  float MaxY;
  float MaxZ;
}
```

In version 1.0 there is an additional byte after `BoneName` that needs to be read.

