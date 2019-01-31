### Spline Continuity

Animation paths are more "demanding" than static shapes in terms of continuity, except in the case of zero tangent in/out where static spline can be kinky and animation reach zero velocity at the control point

static shape smooth means tangentIn and tangentOut in the same direction(special care on zero tangents).

---
Parametric Continuity. 

C_n continuity means its first n drivatives are continuous.

---

Geometric Continuity

This is defined in terms of geometry shapes. For 2-dimensional, only x-y relation is important, not x-t or y-t.

---

A polynomial curve of degree n can really achieve only C_n-1 continuity.

C_n continuity requires n+1 control points

Degree 1(linear) line can only achive C_0 continuity.
Degree 2(quadratic) polynomial can achieve C_1 continuity.
Degree 3(cubic curve) polynomial can achive C_2 continuity.
