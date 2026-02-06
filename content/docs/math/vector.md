---
title: "Vector"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---
# Vector

Vector is a value that contains X amount of numbers.

Vector2 contains 2 floating point numbers (x, y). Used for simulations in 2D space or projected simulations of 3D space.

Vector3 contains 3 floating point numbers (x, y, z). Used for simulations in 3D space.

Vector4 contains 4 floating point numbers (x, y, z, w). Used in shaders to contain RGBA values.

Vectors can represent:
- position in space (position)
- direction with length (velocity)
- property across different axes (scale, Euler's angles)

# Vector * Number

When you multiply a vector by a number, all of the vector's numbers get multiplied by it.

Depending on the multiplicative, the direction of the vector might change:
- Multiplicative > 0:
	- Resulting vector faces the same way
- Multiplicative = 0:
	- Resulting vector is a null vector and doesn't face anywhere.
- Multiplicative < 0:
	- Resulting vector faces the opposite plan.

Example of using vector multiplication:
#TODO go back here
{{< tabs >}}
{{% tab "GDScript" %}}
```gdscript
const forward_direction := Vector3.FORWARD # Moving towards -Z axis
const speed := 10.0 # 10 units per second.

velocity = forward_direction * speed
```
{{% /tab %}}
{{% tab "Unity" %}}
```c#
Vector3.Dot(vectorA, vectorB);
```
{{% /tab %}}
{{< /tabs >}}

# Normalized Vector

Normalized Vector has a length of 1. This means that vector expresses only the direction.

If you multiply normalized vector by a number, you add the length component back.

# Basis * Vector

Basis is a Matrix of X by X size. It can be seen as an X-sized array of VectorXs, every one of which express axis of an object's local space.

In Godot, Basis contians 3 Vector3 and represents rotation and scale of an object.

Multiplying Vector by Basis returns Vector, that was scale and rotated according to Basis.

{{< tabs >}}
{{% tab "GDScript" %}}
```gdscript
# Global basis - Combination of rotation and scale. Orthanormalization - removes scale factor.
# Multiplying rotated Basis by Vector3 returns Vector3 rotated according to the Basis.
var forward_direction : Vector3 = global_basis.orthonormalized() * Vector3.FORWARD
const speed = 10.0 # 10 units per second.

velocity = forward_direction * speed
```
{{% /tab %}}
{{% tab "Unity" %}}
```c#
const float Speed = 10.0 // 10 units per second

// Unity does not have a Basis type. You can use Transform methods in order to get same results
GetComponent<Rigidbody>().velocity = transform.forward * speed;
```
{{% /tab %}}
{{< /tabs >}}

# Transform

Transform is a Matrix of X by X+1 size.
It's a Basis matrix with an additional VectorX representing object's origin (position) in space.

In Godot, there's 2 Transform types:
- Transform2D: 2x3 matrix:
	- Vector2 x axis
	- Vector2 y axis
	- Vector2 origin
- Transform3D: 3x4 matrix:
	- Vector3 x axis
	- Vector3 y axis
	- Vector3 z axis
	- Vector3 origin

This class contains everything you need about position in 2D or 3D space.

It's Unity's counterpart does both 2D and 3D and is an interface for object's basis.

# Dot product

Result of a dot product of 2 vectors allows to compare angle between them.


{{< tabs >}}
{{% tab "Math" %}} 
```katex
\vec{a} \cdot \vec{b} = |\vec{a}| |\vec{b}| \cos \theta

```

\\( \vec{a} \cdot \vec{b} \\) - dot product of vector A and B

\\( |\vec{a}| \\) - length of vector A

\\( |\vec{b}| \\) - length of vector A

\\( \cos \theta \\) - cosine of an angle between A and B

{{% /tab %}}
{{% tab "GDScript" %}}
```gdscript
vector_a.dot(vector_b)
```
{{% /tab %}}
{{% tab "Unity" %}}
```c#
Vector3.Dot(vectorA, vectorB);
```
{{% /tab %}}
{{< /tabs >}}

You can compare the result of the dot product to see angle between 2 vectors:
- Dot product > 0:
	- Angle between vectors is < 90°
- Dot product = 0:
	- Angle between vectors is = 90°
- Dot product < 0:
	- Angle between vectors is > 90°

If both vectors are normalized, the value of dot product will range [-1; 1]. 


If we use normalized vectors, or divide the dot product by lengths of the vectors, we can get angle between them.

{{< tabs >}}
{{% tab "Math" %}} 
```katex
\theta = \frac{ \arccos ( \vec{a} \cdot \vec{b} ) } { |\vec{a}| |\vec{b}| }
```
{{% /tab %}}
{{% tab "GDScript" %}}
```gdscript
vector_a.angle_to(vector_b)
```
{{% /tab %}}
{{% tab "Unity" %}}
```c#
Vector3.Angle(vectorA, vectorB);
```
{{% /tab %}}
{{< /tabs >}}
