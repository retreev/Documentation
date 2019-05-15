# Reference

## Commands

### `Source`

**Texture Source**

```text
Source = [Explode.jpg, 32, 32
```

#### Arguments:

* File Name
* Sprite Width
* Sprite Height
* _\(optional\)_ Billboard On/Off

**Puppet Source**

```text
Source = *pet gp_blackhole-purple02.pet, petlight
```

#### Arguments:

* File Name \(prefixed with `*pet`\)
* Lighting _\(petlight\)_

### `Blend`

```text
Blend = Add, Add
```

#### Arguments:

* Blend Mode, one of:
  * `Normal`
  * `Add`
  * `Multiply`
  * `InvMultiply`
* Tail Blend Mode, one of:
  * `Normal`
  * `Add`
  * `Multiply`
  * `InvMultiply`

### `Lighting`

```text
Lighting = Off
```

#### Arguments:

* Lighting Mode, one of or none:
  * `Off`
  * `Diffuse`
  * `Ambient1`
  * `Ambient2`

### `GenPos`

```text
GenPos = (0, 0, 0), (0, 0, 0)
```

#### Arguments:

* Minimum AABB \(Axis-aligned bounding box\)
  * `x`
  * `y`
  * `z`
* Maximum AABB
  * `x`
  * `y`
  * `z`

### `Init_Size`

```text
Init_Size = (1.45, 1.45), 1
```

#### Arguments:

* Multiples \(between 0 and 1\)
  * Minimal Size
  * Maximum Size
  * Ratio

### `Add_Size`

```text
Add_Size = (0 %, 90 %), (100 %, 150 %)
```

#### Arguments:

* Percentiles
  * Start Time
  * Start Size
* Percentiles
  * End Time
  * End Size

### `Init_Frame`

```text
Init_Frame = (0, 2)
```

#### Arguments:

* Integer values
  * Minimal Frame
  * Maximum Frame

### `Add_Frame`

```text
Add_Frame = (0%, 100%), (0, 15, 1f)
```

#### Arguments:

* Percentiles
  * Start Time
  * End Time
* Integer values
  * Start Frame
  * End Frame
  * Transition Time \(ms/frames\)

### `Add_Generation`

```text
Add_Generation = (0, 100), 30, 30d
```

#### Arguments:

* Integer values
  * Start Frame
  * End Frame
* Number of items to generate
* Angle of Cone \(deg/rad\)

### `LifeTime`

```text
LifeTime = (40f, 60f)
```

#### Arguments:

* Float values
  * Minimum lifetime \(ms/frames\)
  * Maximum lifetime \(ms/frames\)

### `Add_Fade`

```text
Add_Fade = (0 %, (255, 255, 100, 100)), (100 %, (255, 255, 100, 100))
```

#### Arguments:

* Start values
  * Start Time \(Percentile\)
  * Start Color \(ARGB\_255\)
* End values
  * End Time \(Percentile\)
  * End Color \(ARGB\_255\)

### `Init_Velocity`

```text
Init_Velocity = (0, 0.2, 0), (0, 0.5, 0)
```

#### Arguments:

* Minimum Velocity Direction
  * `x`
  * `y`
  * `z`
* Maximum Velocity Direction
  * `x`
  * `y`
  * `z`

### `Add_Velocity`

```text
Add_Velocity = (0, 100 %), (5000, 0 %)
```

#### Arguments:

* Start values
  * Start Time \(ms/frames\)
  * Start Velocity
* End values
  * End Time \(ms/frames\)
  * End Velocity

### `Tail`

```text
Tail = 10, 3f, (255, 0, 0, 0), ~ball_straight.jpg, 90, monolith, cameraside
```

#### Arguments:

* Length
* Generation Interval \(ms/frames\)
* End Color \(ARGB\_255\)
* Texture File Name
* Width
* Status \(_monolith_\)
* Facing Camera \(_cameraside_\)

### `Init_Angle`

```text
Init_Angle = (0d, 0d, 0d), (360d, 360d, 360d)
```

#### Arguments:

* Minimum Angles \(deg/rad\)
  * `x`
  * `y`
  * `z`
* Maximum Angles \(deg/rad\)
  * `x`
  * `y`
  * `z`

### `Init_Rotation`

```text
Init_Rotation = (-10d, -10d, -10d), (10d, 10d, 10d)
```

#### Arguments:

* Minimum Angular Velocity \(deg/rad\)
  * `x`
  * `y`
  * `z`
* Maximum Angular Velocity \(deg/rad\)
  * `x`
  * `y`
  * `z`

### `Add_Rotation`

```text
Add_Rotation = (0%, 100%), (100%, 100%)
```

#### Arguments:

* Start values
  * Start Time \(Percentile\)
  * Start Angular Velocity \(Percentile\)
* End values
  * End Time \(Percentile\)
  * End Angular Velocity \(Percentile\)

### `Friction`

```text
Friction = 0.003
```

#### Arguments:

* Friction \(Float\)

### `GroundFriction`

```text
GroundFriction = 0.1
```

#### Arguments:

* Friction \(Float\)

### `Gravity`

```text
Gravity = (0, -0.01, 0)
```

#### Arguments:

* Gravity Direction
  * `x`
  * `y`
  * `z`

### `Add_GravityPoint`

```text
Add_GravityPoint = (0, 0, 0), 0, 0, 0.03
```

#### Arguments:

* Center Position
  * `x`
  * `y`
  * `z`
* Intensity
* Barrier Range
* Out-of-Range Intensity

### `Ground`

```text
Ground = 0, 1%, 0
```

#### Arguments:

* Height
* Elasticity \(Percentile\)
* Collision Radius

### `Add_Quake`

```text
Add_Quake = (0, 0, 0), (1000, (0, 0, 0)), (2000, (0, 0, 0))
```

#### Arguments:

* Center Position
  * `x`
  * `y`
  * `z`
* Start values
  * Start Time \(ms/frames\)
  * Start Intensity
    * `x`
    * `y`
    * `z`
* End values
  * End Time \(ms/frames\)
  * End Intensity
    * `x`
    * `y`
    * `z`
* Time Interval \(ms/frames\)

### `Add_Flash`

```text
Add_Flash = 0, 5, 10
```

#### Arguments:

* Start Time \(ms/frames\)
* Maximum Time \(ms/frames\)
* End Time \(ms/frames\)

### `CamOffset`

```text
CamOffset = 0.1
```

#### Arguments:

* Offset to Camera \(Float\)

### `Vol_Core`

```text
Vol_Core = (0.85, 0.8)
```

#### Arguments:

* Positional values
  * Horizontal Scale
  * Vertical Scale

### `Vol_Max_Opacity`

```text
Vol_Max_Opacity = 0.5
```

#### Arguments:

* Opacity \(Float\)

### `Dist_Limit`

```text
Dist_Limit = 300, 400
```

#### Arguments:

* Positional values
  * `x`
  * `y`

### `Add_Bubble`

```text
Add_Bubble = (0, 0), (0.003, 2.2), (0.003, 2)
```

#### Arguments:

* Bubble motion values
  * Front/Back Amplitude
  * Frequency/second
* Bubble motion values
  * Bottom Amplitude
  * Frequency/second
* Bubble motion values
  * Left/Right Amplitude
  * Frequency/second

### `Ext_Orientation`

```text
Ext_Orientation = 1, 1, 1
```

#### Arguments:

* `x` \(0 or 1\)
* `y` \(0 or 1\)
* `z` \(0 or 1\)

### `Add_Wind`

```text
Add_Wind = (0.03, 0.05)
```

#### Arguments:

* Float values
  * Minimum Rate
  * Maximum Rate

### `Ext_Wind`

```text
Ext_Wind = (1, 0, 0)
```

#### Arguments:

* Positional values
  * `x`
  * `y`
  * `z`

### `IsSphere`

```text
IsSphere = 1
```

#### Arguments:

* Integer \(0: Box, 1: Sphere\)

### `RegenAtSamePos`

```text
RegenAtSamePos = 1
```

#### Arguments:

* Integer boolean representation

