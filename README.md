# LVM.jl

Simple tool for reading `.lvm` files or other tabular data with columns that are stacked. In traditional text files, a single column represents a distinct set of data of a single unit (intensity, wavelength, etc.). For example, the first column is `x` and the second column is `y` &mdash; the `x` and `y` columns are represented by only one unit and the header (if there is one) appears at the top of the column. For some reason, I have a program that spits out data where there are columns `a`, `b`, `c`, `d` at the top, but then below these data are additional data `e` and `f`, with different units and with their own headers. The number of columns in each stack is also different, throwing up errors when trying to read these files. These `.lvm` files cannot simply be read by a program like `CSV.jl` and must be pre-processed.

This program takes these text files and returns a Julia dictionary with each real column of data as a separate key-value pair. This dictionary can easily be read by into a DataFrame using `DataFrames.jl` or similar package.

Example of the offending text file type:

```
A   B   C
0.1 0.1 0.1
0.2 0.2 0.2
0.3 0.3 0.3
0.2 0.2 0.2
0.1 0.2 0.1
D   E
1   1
2   2
3   3
4   4
5   5
```

This becomes

```
Dict(:A => [0.1, 0.2, 0.3, 0.2, 0.1], :B => [...], :C => [...], :D => [...], :E => [...])
```

## Example of how to use `LVM.read`

If anyone besides me is using this, then they should pass `getall=true` in the arguments.
Otherwise, it will look for keywords in the headers specific to my experiment and change the
corresponding dictionary keys to be more readable.

```
julia> LVM.read(filename, getall=true)

Dict{Any, Any}:
    "CH0" => [...]
    "CH1" => [...]
    ...
```