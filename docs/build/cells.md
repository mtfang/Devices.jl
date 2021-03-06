
Cells are used to logically group polygons or references to other cells into a single entity.

<a id='Devices.Cells.Cell' href='#Devices.Cells.Cell'>#</a>
**`Devices.Cells.Cell`** &mdash; *Type*.



```
type Cell{T<:Real}
    name::ASCIIString
    elements::Array{AbstractPolygon{T},1}
    refs::Array{CellRef,1}
    create::DateTime
    function even(str)
        if mod(length(str),2) == 1
            str*" "
        else
            str
        end
    end
    Cell(x,y,z) = new(even(x), y, z, now())
    Cell(x,y) = new(even(x), y, CellReference[], now())
    Cell(x) = new(even(x), AbstractPolygon{T}[], CellReference[], now())
end
```

A cell has a name and contains polygons and references to `CellArray` or `CellReference` objects. It also records the time of its own creation. As currently implemented it mirrors the notion of cells in GDS-II files.

In the future, it may make sense to generalize the idea and permit `Path` objects within a Cell.

To add elements, push them to `elements` field (or use `render!`); to add references, push them to `refs` field.

<a id='Devices.Cells.Cell-Tuple{AbstractString}' href='#Devices.Cells.Cell-Tuple{AbstractString}'>#</a>
**`Devices.Cells.Cell`** &mdash; *Method*.



```
Cell(name::AbstractString)
```

Convenience constructor for `Cell{Float64}`.

<a id='Devices.Cells.Cell-Tuple{AbstractString,AbstractArray{Devices.AbstractPolygon{T<:Real},1}}' href='#Devices.Cells.Cell-Tuple{AbstractString,AbstractArray{Devices.AbstractPolygon{T<:Real},1}}'>#</a>
**`Devices.Cells.Cell`** &mdash; *Method*.



```
Cell{T<:Real}(name::AbstractString, elements::AbstractArray{AbstractPolygon{T},1})
```

Convenience constructor for `Cell{T}`.

<a id='Devices.Cells.Cell-Tuple{AbstractString,AbstractArray{Devices.AbstractPolygon{T<:Real},1},AbstractArray{Devices.Cells.CellReference{S,T},1}}' href='#Devices.Cells.Cell-Tuple{AbstractString,AbstractArray{Devices.AbstractPolygon{T<:Real},1},AbstractArray{Devices.Cells.CellReference{S,T},1}}'>#</a>
**`Devices.Cells.Cell`** &mdash; *Method*.



```
Cell{T<:Real}(name::AbstractString, elements::AbstractArray{AbstractPolygon{T},1},
    refs::AbstractArray{CellReference,1})
```

Convenience constructor for `Cell{T}`.

<a id='Devices.bounds-Tuple{Devices.Cells.Cell{T<:Real}}' href='#Devices.bounds-Tuple{Devices.Cells.Cell{T<:Real}}'>#</a>
**`Devices.bounds`** &mdash; *Method*.



```
bounds(cell::Cell; kwargs...)
```

Returns a `Rectangle` bounding box with no properties around all objects in `cell`.

<a id='Devices.center-Tuple{Devices.Cells.Cell{T<:Real}}' href='#Devices.center-Tuple{Devices.Cells.Cell{T<:Real}}'>#</a>
**`Devices.center`** &mdash; *Method*.



```
center(cell::Cell)
```

Convenience method, equivalent to `center(bounds(cell))`. Returns the center of the bounding box of the cell.


<a id='Referenced-and-arrayed-cells-1'></a>

## Referenced and arrayed cells


Cells can be arrayed or referenced within other cells for efficiency or to reduce display complexity.

<a id='Devices.Cells.CellArray' href='#Devices.Cells.CellArray'>#</a>
**`Devices.Cells.CellArray`** &mdash; *Type*.



```
type CellArray{S,T} <: CellRef{S,T}
    cell::S
    origin::Point{2,T}
    deltacol::Point{2,T}
    deltarow::Point{2,T}
    col::Int
    row::Int
    xrefl::Bool
    mag::Float64
    rot::Float64
end
```

Array of `cell` starting at `origin` with `row` rows and `col` columns, spanned by vectors `deltacol` and `deltarow`. Optional x-reflection `xrefl`, magnification factor `mag`, and rotation angle `rot` in degrees are for the array as a whole.

The type variable `S` is to avoid circular definitions with `Cell`.

<a id='Devices.Cells.CellArray-Tuple{Devices.Cells.Cell{T<:Real},FixedSizeArrays.Point{2,T<:Real},FixedSizeArrays.Point{2,T<:Real},FixedSizeArrays.Point{2,T<:Real},Integer,Integer}' href='#Devices.Cells.CellArray-Tuple{Devices.Cells.Cell{T<:Real},FixedSizeArrays.Point{2,T<:Real},FixedSizeArrays.Point{2,T<:Real},FixedSizeArrays.Point{2,T<:Real},Integer,Integer}'>#</a>
**`Devices.Cells.CellArray`** &mdash; *Method*.



```
CellArray{T<:Real}(x::Cell, origin::Point{2,T}, dc::Point{2,T}, dr::Point{2,T},
    c::Integer, r::Integer; xrefl=false, mag=1.0, rot=0.0)
```

Construct a `CellArray{typeof(x),T}` object, with `xrefl`, `mag`, and `rot` as keyword arguments (x-reflection, magnification factor, rotation in degrees).

<a id='Devices.Cells.CellArray-Tuple{Devices.Cells.Cell{T<:Real},Range{T<:Real},Range{T<:Real}}' href='#Devices.Cells.CellArray-Tuple{Devices.Cells.Cell{T<:Real},Range{T<:Real},Range{T<:Real}}'>#</a>
**`Devices.Cells.CellArray`** &mdash; *Method*.



```
CellArray{T<:Real}(x::Cell, c::Range{T}, r::Range{T};
    xrefl=false, mag=1.0, rot=0.0)
```

Construct a `CellArray{typeof(x), T}` based on ranges (probably `LinSpace` or `FloatRange`). `c` specifies column coordinates and `r` for the rows. Pairs from `c` and `r` specify the origins of the repeated cells. The extrema of the ranges therefore do not specify the extrema of the resulting `CellArray`'s bounding box; some care is required.

`xrefl`, `mag`, and `rot` are keyword arguments (x-reflection, magnification factor, rotation in degrees).

<a id='Devices.Cells.CellReference' href='#Devices.Cells.CellReference'>#</a>
**`Devices.Cells.CellReference`** &mdash; *Type*.



```
type CellReference{S,T} <: CellRef{S,T}
    cell::S
    origin::Point{2,T}
    xrefl::Bool
    mag::Float64
    rot::Float64
end
```

Reference to a `cell` positioned at `origin`, with optional x-reflection `xrefl`, magnification factor `mag`, and rotation angle `rot` in degrees.

The type variable `S` is to avoid circular definitions with `Cell`.

<a id='Devices.Cells.CellReference-Tuple{Devices.Cells.Cell{T<:Real},FixedSizeArrays.Point{2,T<:Real}}' href='#Devices.Cells.CellReference-Tuple{Devices.Cells.Cell{T<:Real},FixedSizeArrays.Point{2,T<:Real}}'>#</a>
**`Devices.Cells.CellReference`** &mdash; *Method*.



```
CellReference{T<:Real}(x::Cell, y::Point{2,T}=Point(0.,0.);
    xrefl=false, mag=1.0, rot=0.0)
```

Convenience constructor for `CellReference{typeof(x), T}`.

<a id='Devices.bounds-Tuple{Devices.Cells.CellArray{Devices.Cells.Cell{S<:Real},T<:Real}}' href='#Devices.bounds-Tuple{Devices.Cells.CellArray{Devices.Cells.Cell{S<:Real},T<:Real}}'>#</a>
**`Devices.bounds`** &mdash; *Method*.



```
bounds(ref::CellArray; kwargs...)
```

Returns a `Rectangle` bounding box with properties specified by `kwargs...` around all objects in `ref`. The bounding box respects reflection, rotation, and magnification specified by `ref`.

Please do rewrite this method when feeling motivated... it is very inefficient.

<a id='Devices.bounds-Tuple{Devices.Cells.CellReference{S,T}}' href='#Devices.bounds-Tuple{Devices.Cells.CellReference{S,T}}'>#</a>
**`Devices.bounds`** &mdash; *Method*.



```
bounds(ref::CellReference; kwargs...)
```

Returns a `Rectangle` bounding box with properties specified by `kwargs...` around all objects in `ref`. The bounding box respects reflection, rotation, and magnification specified by `ref`.


<a id='Resolving-references-1'></a>

## Resolving references


When saving cells to disk, there will be a tree of interdependencies and logically one would prefer to write the leaf nodes of the tree before any dependent cells. These functions are used to traverse the tree and then find the optimal ordering.

<a id='Devices.Cells.traverse!' href='#Devices.Cells.traverse!'>#</a>
**`Devices.Cells.traverse!`** &mdash; *Function*.



```
traverse!(a::AbstractArray, c::Cell, level=1)
```

Given a cell, recursively traverse its references for other cells and add to array `a` some tuples: `(level, c)`. `level` corresponds to how deep the cell was found, and `c` is the found cell.

<a id='Devices.Cells.order!' href='#Devices.Cells.order!'>#</a>
**`Devices.Cells.order!`** &mdash; *Function*.



```
order!(a::AbstractArray)
```

Given an array of tuples like that coming out of [`traverse!`](cells.md#Devices.Cells.traverse!), we sort by the `level`, strip the level out, and then retain unique entries. The aim of this function is to determine an optimal writing order when saving pattern data (although the GDS-II spec does not require cells to be in a particular order, there may be performance ramifications).

For performance reasons, this function modifies `a` but what you want is the returned result array.

