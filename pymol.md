# PyMOL

## Save current view as a PNG

```
png filename, [width=<DIM>], [height=<DIM>], [dpi=,INT>], [ray=(0|1)], [quiet=(0|1)]

# DIM
# integer (pixels)
# string: specify units ("in"|"cm")
```

### Transparent background

```
set ray_opaque_background, 0
png filename, width=4800, dpi=600, ray=1
```

### Flat

```
bg_color white
set ambient, 1.0
set ray_trace_mode, 1 # black outline with color
set ray_trace_color, black
set specular, off     # no reflections
```
