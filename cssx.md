# cssx

A tiny utility framework: **reset → base → tokens → utilities.**

## reset
```css
* {
  margin: 0;
  padding: 0;
  border-style: solid;
  border-width: 0;
  box-sizing: border-box;
}
```

## base
```css
body { min-height: 100vh; }
ul, ol { list-style: none; }
a { color: inherit; text-decoration: none; }
button { cursor: pointer; }
```

---

# utilities

## color

### palette
9 families × 3 temperatures — each non-anchor family carries a `deep` mode, a
darker-hue variant listed under its parent (Tailwind red/orange/green/blue/
purple/fuchsia values):

| temperature | families |
|---|---|
| warm | stone · rose · rose-deep · amber · amber-deep |
| cold | slate · emerald · emerald-deep · sky · sky-deep |
| cool | zinc · violet · violet-deep · pink · pink-deep |

### shades
| family | shades |
|---|---|
| stone / slate / zinc | 50 · 100 · 200 · 800 · 950 |
| the other twelve (`*-deep` incl.) | 100 · 200 · 800 |

`50` = white, `950` = black (only the three temperature anchors carry them).

### roles
Three roles, one per surface. Each maps a family to a shade:

| role | shade |
|---|---|
| `bg` — background | `-100` |
| `b` — border | `-200` |
| `font` — text | `-800` |

Utilities: `.bg-*` `.b-*` `.font-*`

### black / white
Per temperature, `bg` and `font` only (no `b`):
`bg-warm-white` (stone-50) … `bg-warm-black` (stone-950), and same for `cold`/`cool`.

---

## typography

**font = data** (what the glyphs *are*) · **text = action** (what you *do* to a run of text).

### family
```
font-reading / font-ui / font-code   =  serif / sans / mono
```

### size
`font-1…6` reads the **scale** spine at `scale-3…8`:

| class | token | px |
|---|---|---|
| font-1 | scale-3 | 12 |
| font-2 | scale-4 | 16 |
| font-3 | scale-5 | 24 |
| font-4 | scale-6 | 32 |
| font-5 | scale-7 | 48 |
| font-6 | scale-8 | 64 |

The scale spine itself (rem, 6-based odd / 8-based even) is defined in **box → size** — it also feeds box size and spacing.

### style
```
font-light   (300) · font-bold (700)
font-italic
```

### alignment
```
text-start / text-end / text-center   (text-align)
```

---

## box

### size
`size` and `fixed-size`, each with all-axis and `-x` / `-y` variants, read the **scale** spine at `scale-5…14`:

```
size(-x/y)-1…10      width/height
fixed-size(-x/y)-1…10  min = max (locked size)
```

**scale spine** (rem) — 14 steps, interleaving two doubling sequences (6-based odd / 8-based even):

| lv | 6-based (odd) | 8-based (even) |
|---|---|---|
| 1 | 6 → `scale-1` | 8 → `scale-2` |
| 2 | 12 → `scale-3` | 16 → `scale-4` |
| 3 | 24 → `scale-5` | 32 → `scale-6` |
| 4 | 48 → `scale-7` | 64 → `scale-8` |
| 5 | 96 → `scale-9` | 128 → `scale-10` |
| 6 | 192 → `scale-11` | 256 → `scale-12` |
| 7 | 384 → `scale-13` | 512 → `scale-14` |

Three windows onto the spine:

| token | window | values (px) | feeds |
|---|---|---|---|
| `space-1…6` | scale-1…6 | 6 8 12 16 24 32 | margin / padding / gap |
| `font-1…6` | scale-3…8 | 12 16 24 32 48 64 | font-size |
| `size-1…10` | scale-5…14 | 24 32 48 64 96 128 192 256 384 512 | width / height |

### spacing
margin / padding, each with all-axis and x/y variants (`space-1…6`):

```
m(-x/y)-1…6
p(-x/y)-1…6
```

### edge
border-width · radius · shadow. Radius and shadow read the **grain** spine (px) and **proportion** tokens.

**grain spine** (px) — 6 steps, interleaving two doubling sequences (2-based odd / 3-based even):

| lv | 2-based (odd) | 3-based (even) |
|---|---|---|
| 1 | 2 → `grain-1` | 3 → `grain-2` |
| 2 | 4 → `grain-3` | 6 → `grain-4` |
| 3 | 8 → `grain-5` | 12 → `grain-6` |

| token | window | values (px) | feeds |
|---|---|---|---|
| `line-1…4` | grain-1…4 | 2 3 4 6 | border-width |
| `corner-1…6` | grain-1…6 | 2 3 4 6 8 12 | border-radius |
| `haze-1…4` | grain-1…4 | 2 3 4 6 | shadow blur |

```
b(-x/y)-1…4          border-width   (line)
r-1…6                border-radius  (corner)
r-circle  r-pill     radius 50% / 9999px  (proportion)
sh-1…4               box-shadow      (haze + alpha)
```

**proportion** — anchor + alpha:

```
none: 0          inf: 9999px
frac-half: 0.5   frac-full: 1
pct-half: 50%     pct-full: 100%
alpha-1…4: 0.04 · 0.08 · 0.16 · 0.32
```

`sh-1…4` = `haze` (blur) + `alpha` (opacity):
```css
.sh-1 { box-shadow: 0 0 var(--haze-1) rgb(0 0 0 / var(--alpha-1)); }
```

---

## layout

**position** = where it anchors (`vport`/`scrol`) · **engine** = how children arrange (`flexbox`, `gridbox`).

### position
`fixed` pins to the viewport, `sticky` pins to the scroller:

```
vport / scrol  (-x/-y)  (-start/-end/-center)   ← center: planned
```
- `-x` / `-y` bare = span the full axis (`left:0; right:0`)
- `-start` / `-end` = pin one edge
- `-center` = center on the axis *(not yet implemented)*

### flexbox
= **flex** engine + **box** alignment.

**container**
```
flexbox-row / flexbox-col           direction
single-line / multi-line            nowrap / wrap
```

**item**
```
flex-auto   = flex: 1 1 auto
flex-rigid  = flex: 0 0 auto
```

### gridbox
*(planned — reserved)*

### alignment
= **gap** + **flex alignment** + **grid alignment**. Gap reads `space-1…6`:

```
g(-x/y)-1…6                     gap / column-gap / row-gap

main-start/end/center/spaced    justify-content
cross-start/end/center/spaced   align-items (spaced = align-content)
self-start/end/center/stretch   align-self (per-item override of cross-*)
```
*(grid alignment — planned)*
