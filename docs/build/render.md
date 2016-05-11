
<a id='Rendering-1'></a>

## Rendering

<a id='Devices.render!' href='#Devices.render!'>#</a>
**`Devices.render!`** &mdash; *Function*.

---


```
render!(c::Cell, segment::Paths.Segment, s::Paths.Style; kwargs...)
```

Render a `segment` with style `s` to cell `c`.

```
render!(c::Cell, segment::Paths.Segment, s::Paths.DecoratedStyle; kwargs...)
```

Render a `segment` with decorated style `s` to cell `c`. This method draws the decorations before the path itself is drawn.

```
render!(c::Cell, p::Path; kwargs...)
```

Render a path `p` to a cell `c`.

```
render!(c::Cell, r::Polygon, s::Polygons.Style=Polygons.Plain(); kwargs...)
```

Render a polygon `r` to cell `c`, defaulting to plain styling.

```
render!(c::Cell, r::Rectangle, s::Rectangles.Style=Rectangles.Plain(); kwargs...)
```

Render a rectangle `r` to cell `c`, defaulting to plain styling.

```
render!(c::Cell, r::Rectangle, s::Rectangles.Rounded; kwargs...)
```

Render a rounded rectangle `r` to cell `c`. This is accomplished by rendering a path around the outside of a (smaller than requested) solid rectangle. The bounding box of `r` is preserved.

```
render!(c::Cell, r::Rectangle, ::Rectangles.Plain; kwargs...)
```

Render a rectangle `r` to cell `c` with plain styling.


<a id='Saving-patterns-1'></a>

## Saving patterns


To save a pattern, make sure you are `using FileIO`.

<a id='FileIO.save-Tuple{FileIO.File{FileIO.DataFormat{:GDS}},Devices.Cells.Cell{T<:Real},Vararg{Devices.Cells.Cell{T<:Real}}}' href='#FileIO.save-Tuple{FileIO.File{FileIO.DataFormat{:GDS}},Devices.Cells.Cell{T<:Real},Vararg{Devices.Cells.Cell{T<:Real}}}'>#</a>
**`FileIO.save`** &mdash; *Method*.

---


```
save(::Union{AbstractString,IO}, cell0::Cell, cell::Cell...)

save(f::File{format"GDS"}, cell0::Cell, cell::Cell...;
name="GDSIILIB", precision=1e-9, unit=1e-6, modify=now(), acc=now(),
verbose=false)`
```

This bottom method is implicitly called when you use the convenient syntax of the top method: `save("/path/to/my.gds", cells_i_want_to_save...)`

The `name` keyword argument is used for the internal library name of the GDS-II file and is probably inconsequential for modern workflows.

The `verbose` keyword argument allows you to monitor the output of [`traverse!`](cells.md#Devices.Cells.traverse!) and [`order!`](cells.md#Devices.Cells.order!) if something funny is happening while saving.
