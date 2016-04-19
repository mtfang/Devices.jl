
<a id='Index-1'></a>

## Index

- [`Devices.Paths.CPW`](api.md#Devices.Paths.CPW)
- [`Devices.Paths.Straight`](api.md#Devices.Paths.Straight)
- [`Devices.Paths.Style`](api.md#Devices.Paths.Style)
- [`Devices.Paths.Trace`](api.md#Devices.Paths.Trace)
- [`Devices.Paths.Turn`](api.md#Devices.Paths.Turn)
- [`Devices.Paths.distance`](api.md#Devices.Paths.distance)
- [`Devices.Paths.extent`](api.md#Devices.Paths.extent)
- [`Devices.Paths.firstangle`](api.md#Devices.Paths.firstangle)
- [`Devices.Paths.firstpoint`](api.md#Devices.Paths.firstpoint)
- [`Devices.Paths.firststyle`](api.md#Devices.Paths.firststyle)
- [`Devices.Paths.lastangle`](api.md#Devices.Paths.lastangle)
- [`Devices.Paths.lastpoint`](api.md#Devices.Paths.lastpoint)
- [`Devices.Paths.laststyle`](api.md#Devices.Paths.laststyle)
- [`Devices.Paths.launch!`](api.md#Devices.Paths.launch!)
- [`Devices.Paths.paths`](api.md#Devices.Paths.paths)
- [`Devices.Paths.preview`](api.md#Devices.Paths.preview)
- [`Devices.Paths.straight!`](api.md#Devices.Paths.straight!)
- [`Devices.Paths.turn!`](api.md#Devices.Paths.turn!)
- [`Devices.Paths.width`](api.md#Devices.Paths.width)
- [`Devices.Points.Point`](api.md#Devices.Points.Point)
- [`Devices.Points.getx`](api.md#Devices.Points.getx)
- [`Devices.Points.gety`](api.md#Devices.Points.gety)
- [`Devices.view`](api.md#Devices.view)

<a id='Points-1'></a>

## Points


Points are implemented using the abstract type `FixedVectorNoTuple` from [FixedSizeArrays.jl](https://github.com/SimonDanisch/FixedSizeArrays.jl). This permits a fast, efficient representation of coordinates in the plane, which would not be true using ordinary `Array` objects, which can have variable length. Additionally, unlike `Tuple` objects, we can add points together, simplifying many function definitions.


To interface with gdspy, we simply convert the `Point` object to a `Tuple` and let [PyCall.jl](https://github.com/stevengj/PyCall.jl) figure out what to do.

<a id='Devices.Points.Point' href='#Devices.Points.Point'>#</a>
**`Devices.Points.Point`** &mdash; *Type*.

---


`immutable Point{T<:Real} <: FixedVectorNoTuple{2,T}`

A point in the plane (Cartesian coordinates).

<a id='Devices.Points.getx' href='#Devices.Points.getx'>#</a>
**`Devices.Points.getx`** &mdash; *Function*.

---


`getx(p::Point)`

Get the x-coordinate of a point.

<a id='Devices.Points.gety' href='#Devices.Points.gety'>#</a>
**`Devices.Points.gety`** &mdash; *Function*.

---


`gety(p::Point)`

Get the y-coordinate of a point.


<a id='Paths-1'></a>

## Paths


<a id='Segments-1'></a>

### Segments

<a id='Devices.Paths.Straight' href='#Devices.Paths.Straight'>#</a>
**`Devices.Paths.Straight`** &mdash; *Type*.

---


`type Straight{T<:Real} <: Segment{T}`

A straight line segment is parameterized by its length. It begins at a point `origin` with initial angle `α0`.

The parametric function over `t ∈ [0,1]` describing the line segment is given by:

`t -> origin + Point(t*l*cos(α),t*l*sin(α))`

<a id='Devices.Paths.Turn' href='#Devices.Paths.Turn'>#</a>
**`Devices.Paths.Turn`** &mdash; *Type*.

---


`type Turn{T<:Real} <: Segment{T}`

A circular turn is parameterized by the turn angle `α` and turning radius `r`. It begins at a point `origin` with initial angle `α0`.

The center of the circle is given by:

`cen = origin + Point(r*cos(α0+sign(α)*π/2), r*sin(α0+sign(α)*π/2))`

The parametric function over `t ∈ [0,1]` describing the turn is given by:

`t -> cen + Point(r*cos(α0-π/2+α*t), r*sin(α0-π/2+α*t))`


<a id='Styles-1'></a>

### Styles

<a id='Devices.Paths.Style' href='#Devices.Paths.Style'>#</a>
**`Devices.Paths.Style`** &mdash; *Type*.

---


`abstract Style`

How to render a given path segment.

<a id='Devices.Paths.Trace' href='#Devices.Paths.Trace'>#</a>
**`Devices.Paths.Trace`** &mdash; *Type*.

---


`type Trace <: Style`

Simple, single trace.

  * `width::Function`: trace width.
  * `divs::Int`: number of segments to render. Increase if you see artifacts.

<a id='Devices.Paths.CPW' href='#Devices.Paths.CPW'>#</a>
**`Devices.Paths.CPW`** &mdash; *Type*.

---


`type CPW <: Style`

Two adjacent traces can form a coplanar waveguide.

  * `trace::Function`: center conductor width.
  * `gap::Function`: distance between center conductor edges and ground plane
  * `divs::Int`: number of segments to render. Increase if you see artifacts.

May need to be inverted with respect to a ground plane, depending on how the pattern is written.


<a id='Path-interrogation-1'></a>

### Path interrogation

<a id='Devices.Paths.firstpoint' href='#Devices.Paths.firstpoint'>#</a>
**`Devices.Paths.firstpoint`** &mdash; *Function*.

---


`firstpoint(p::Path)`

First point of a path.

`firstpoint{T}(s::Segment{T})`

Return the first point in a segment.

<a id='Devices.Paths.lastpoint' href='#Devices.Paths.lastpoint'>#</a>
**`Devices.Paths.lastpoint`** &mdash; *Function*.

---


`lastpoint(p::Path)`

Last point of a path.

`lastpoint{T}(s::Segment{T})`

Return the last point in a segment.

<a id='Devices.Paths.firstangle' href='#Devices.Paths.firstangle'>#</a>
**`Devices.Paths.firstangle`** &mdash; *Function*.

---


`firstangle(p::Path)`

First angle of a path.

<a id='Devices.Paths.lastangle' href='#Devices.Paths.lastangle'>#</a>
**`Devices.Paths.lastangle`** &mdash; *Function*.

---


`lastangle(p::Path)`

Last angle of a path.

<a id='Devices.Paths.firststyle' href='#Devices.Paths.firststyle'>#</a>
**`Devices.Paths.firststyle`** &mdash; *Function*.

---


`firststyle(p::Path)`

Style of the first segment of a path.

<a id='Devices.Paths.laststyle' href='#Devices.Paths.laststyle'>#</a>
**`Devices.Paths.laststyle`** &mdash; *Function*.

---


`laststyle(p::Path)`

Style of the last segment of a path.


<a id='Path-building-1'></a>

### Path building

<a id='Devices.Paths.launch!' href='#Devices.Paths.launch!'>#</a>
**`Devices.Paths.launch!`** &mdash; *Function*.

---


`launch!(p::Path; extround=5, trace0=300, trace1=5,         gap0=150, gap1=2.5, flatlen=250, taperlen=250)`

Add a launcher to the path. Somewhat intelligent in that the launcher will reverse it's orientation depending on if it is at the start or the end of a path.

There are numerous keyword arguments to control the behavior:

  * `extround`: Rounding radius of the outermost corners; should be less than `gap0`.
  * `trace0`: Bond pad width.
  * `trace1`: Center trace width of next CPW segment.
  * `gap0`: Gap width adjacent to bond pad.
  * `gap1`: Gap width of next CPW segment.
  * `flatlen`: Bond pad length.
  * `taperlen`: Length of taper region between bond pad and next CPW segment.

Returns a `Style` object suitable for continuity with the next segment. Ignore the returned style if you are terminating a path.

<a id='Devices.Paths.straight!' href='#Devices.Paths.straight!'>#</a>
**`Devices.Paths.straight!`** &mdash; *Function*.

---


`straight!(p::Path, l::Real)`

Extend a path `p` straight by length `l` in the current direction.

<a id='Devices.Paths.turn!' href='#Devices.Paths.turn!'>#</a>
**`Devices.Paths.turn!`** &mdash; *Function*.

---


`turn!(p::Path, α::Real, r::Real, sty::Style=laststyle(p))`

Turn a path `p` by angle `α` with a turning radius `r` at unit velocity in the path direction. Positive angle turns left.


<a id='Rendering-1'></a>

### Rendering

<a id='Devices.Paths.preview' href='#Devices.Paths.preview'>#</a>
**`Devices.Paths.preview`** &mdash; *Function*.

---


`preview(p::Path, pts::Integer=100; kw...)`

Plot the path using `Plots.jl`, enforcing square aspect ratio of the x and y limits. If using the UnicodePlots backend, pass keyword argument `size=(60,30)` or a similar ratio for display with proper aspect ratio.

No styling of the path is shown, only the abstract path in the plane. A launcher will look no different than a straight line, for instance.

We reserve `xlims` and `ylims` keyword arguments but all other valid Plots.jl keyword arguments are passed along to the plotting function.

<a id='Devices.view' href='#Devices.view'>#</a>
**`Devices.view`** &mdash; *Function*.

---


Launch a LayoutViewer window.


<a id='Interfacing-with-gdspy-1'></a>

## Interfacing with gdspy

<a id='Devices.Paths.distance' href='#Devices.Paths.distance'>#</a>
**`Devices.Paths.distance`** &mdash; *Function*.

---


For a style `s` and parameteric argument `t`, returns the distance between the centers of parallel paths rendered by gdspy.

<a id='Devices.Paths.extent' href='#Devices.Paths.extent'>#</a>
**`Devices.Paths.extent`** &mdash; *Function*.

---


For a style `s` and parameteric argument `t`, returns a distance tangential to the path specifying the lateral extent of the polygons rendered by gdspy.

<a id='Devices.Paths.paths' href='#Devices.Paths.paths'>#</a>
**`Devices.Paths.paths`** &mdash; *Function*.

---


For a style `s` and parameteric argument `t`, returns the number of parallel paths rendered by gdspy.

<a id='Devices.Paths.width' href='#Devices.Paths.width'>#</a>
**`Devices.Paths.width`** &mdash; *Function*.

---


For a style `s` and parameteric argument `t`, returns the width of paths rendered by gdspy.
