# Design Spec — Erfassen

Basis: `capture.html` (Stand nach zoom-Bereinigung). Alle Werte sind finale px-Werte ohne zoom-Faktor.

---

## Tokens

```css
--bg:        #1a1a1a      /* Seitenhintergrund */
--surface:   #262626      /* Card, Panel, Overlay */
--surface-2: #2f2f2f      /* Hover-Fläche, Input-BG */
--border:    #383838      /* Standard-Border */
--border-2:  #444444      /* Stärkere Border, Fokus-Vorstufe */
--text:      #ececec      /* Primärtext */
--text-2:    #a0a0a0      /* Sekundärtext, Labels */
--text-3:    #666666      /* Tertiärtext, Icons, Hints */
--accent:    #FD939F      /* Rosa — CTAs, aktive Zustände */
--accent-bg: rgba(253,147,159,.12)
--error:     #e57373
--good:      #81c784
--radius:    11px         /* Standard Card-Radius */
```

---

## Typografie

```
Font:        ui-sans-serif, sans-serif
Smoothing:   -webkit-font-smoothing: antialiased
Line-height: 1.5 (global)

Body:           15px
Header-Title:   16px / weight 500
Header-Date:    14px / color --text-2
Card-Head-Title: 12px / weight 600 / uppercase / letter-spacing .7px / --text-3
Counter-Label:  12px / weight 500 / uppercase / letter-spacing .4px / --text-3
Label (inline): 12px / weight 500 / uppercase / letter-spacing .4px / --text-3
Chip:           14px
Input (text):   14px
Input (large):  16px   (entry-area, search-input, c-input)
Mood-Value:     24px / weight 500
Status-Label:   12px / weight 600 / uppercase / letter-spacing .7px / --accent
Status-Text:    13px / italic / --text-2
Hint:           12px / --text-3
Metadata/Badge: 11px
```

---

## Header

```
Höhe:               ~46px (padding 11px oben/unten + Inhalt)
Hintergrund:        --bg
Border-Bottom:      1px solid --border
Position:           sticky top:0 / z-index 50
Padding:            11px 18px
Layout:             space-between, 3 Zonen (left / center / right)
```

### Nav-Button / Header-Button (bordiert)
```
Grösse:    31 × 31px
Radius:    7px
Border:    1px solid --border
Icon:      14 × 14px / stroke 2.5
Hover:     background --surface-2, color --text, border --border-2
```

### Header-Icon (borderlos)
```
Grösse:    35 × 35px
Icon:      18 × 18px / stroke 1.8
Hover:     color --text
```

### Folder-Button / Action-Button
```
Höhe:      37px
Padding:   0 13px
Radius:    9px
Border:    1px solid --border
Font:      14px / weight 500
Icon:      16 × 16px / stroke 1.8
Hover:     background --surface-2
Aktiv:     background --accent-bg, color --accent, border rgba(--accent, .4)
```

### Today-Button (klein, akzent)
```
Font:      12px / weight 500
Padding:   3px 9px
Radius:    7px
Border:    1px solid rgba(--accent, .25)
BG:        --accent-bg
Color:     --accent
```

---

## Cards

```
Background:    --surface
Border:        1px solid --border
Radius:        11px (--radius)
Overflow:      hidden
```

### Card-Head
```
Höhe:          40px (fix, alle Cards gleich)
Padding:       0 15px
Border-Bottom: 1px solid --border
Gap:           9px
Icon:          14 × 14px / stroke 2 / --text-3
Title:         12px / weight 600 / uppercase / letter-spacing .7px / --text-3
```

### Card-Body
```
Padding:       15px
```

#### Konsum & Stimmung (card-body)
```
Höhe:          154px (box-sizing: border-box)
Layout:        flex column / gap 14px
```

#### Eintrag (card-body)
```
Höhe:          154px (box-sizing: border-box)
Padding:       14px
```

---

## Counter (Äpfel / Bananen / Kaffee)

```
Layout:        grid 3 Spalten / gap 9px
Label:         12px / uppercase / --text-3

Counter-Ctrl:
  Höhe:        42px
  Radius:      9px
  Border:      1px solid --border
  BG:          --surface-2
  Fokus:       border-color --accent

Minus/Plus-Button:
  Breite:      31px / Höhe: 42px
  Font:        20px
  Hover:       color --accent, background --accent-bg

Zahlenfeld:
  Font:        16px / weight 500
  Align:       center
  BG:          transparent
```

---

## Mood-Slider

```
Mood-Value:    24px / weight 500 / min-width 57px
Track:         Höhe 3px / gradient rot → grau → grün
Thumb:         20 × 20px / radius 50% / background --accent / shadow 0 1px 4px rgba(0,0,0,.5)
Thumb hover:   scale(1.2)
Ticks:         11px / --text-3 / margin-top 3px
```

---

## Chips

```
Höhe:          31px
Padding:       0 11px
Radius:        15px
Border:        1px solid --border
Font:          14px / font-family: inherit
Color:         --text-2
Gap (intern):  6px
Transition:    all .15s

Hover:         border --border-2, color --text
Aktiv (.on):   background --accent-bg, border rgba(--accent, .4), color --accent

Chip-Dot:      6 × 6px / radius 50% / --accent (nur wenn .on)
Chip-X:        11px / opacity .6 (nur wenn .on)
```

### Chip-Input (Texteingabe als Chip)
```
Höhe:          31px
Padding:       0 11px
Radius:        15px
Border:        1px dashed --border-2
Min-Width:     121px
Font:          14px / font-family: inherit
Fokus:         border-style solid, border-color --accent
```

### Autocomplete-Dropdown
```
BG:            --surface-2
Border:        1px solid --border-2
Radius:        9px
Shadow:        0 9px 26px rgba(0,0,0,.4)
Max-Height:    198px
Item:          padding 9px 13px / 14px / --text-2
Item hover:    background --border, color --text
```

---

## Text-Inputs (inline, settings)

```
BG:            --surface-2
Border:        1px solid --border
Radius:        8px
Padding:       8px 11px
Font:          14px / font-family: inherit
Fokus:         border-color --accent
Placeholder:   --text-3
```

---

## Textarea (Eintrag)

```
Font:          16px / line-height 1.7
BG:            transparent
Border:        none
Resize:        vertical (im normalen Modus)
Min-Height:    88px
```

---

## Icon-Buttons (Header-Bereich, allgemein)

```
Bordiert:      37 × 37px / radius 9px / border 1px --border
Borderlos:     35 × 35px / kein border
Icon:          15–18px / stroke 1.8
Hover (bord.): background --surface-2, color --text, border --border-2
Hover (blos.): color --text
```

---

## Buttons (Action)

### Primär (akzent)
```
Höhe:          37px
Padding:       0 18px
Radius:        9px
Border:        1px solid rgba(--accent, .4)
BG:            --accent-bg
Font:          14px / weight 500 / --accent
Hover:         background rgba(--accent, .2)
```

### Sekundär (neutral)
```
Höhe:          37px
Padding:       0 15px
Radius:        9px
Border:        1px solid --border
BG:            transparent
Font:          14px / --text-3
Hover:         background --surface-2, color --text
```

### Klein (btn-small / today-btn)
```
Font:          12–13px / weight 500
Padding:       4px 9–11px
Radius:        7px
Border:        1px solid rgba(--accent, .25)
BG:            --accent-bg
Color:         --accent
```

---

## Overlays (Search, Export, Settings)

```
Backdrop:      rgba(0,0,0,.45)
Panel-BG:      --surface
Panel-Border:  1px solid --border-2
Panel-Radius:  11–15px
Shadow:        0 26px 66px rgba(0,0,0,.35)
Padding-Top:   88px (Abstand von oben)
Max-Width:     528–748px / 92vw
```

### Panel-Head
```
Padding:       12px 15px 11px
Border-Bottom: 1px solid --border
Icon:          14 × 14px / --text-3
Title:         12px / weight 600 / uppercase / letter-spacing .7px / --text-3
Close (✕):     15px / --text-3 → --text on hover / margin-left auto
```

### Search spezifisch
```
Panel-Radius:  15px
Min-Height:    57px (search-head)
Input:         16px / flex 1
Count:         12px / --text-3
Suggestion:    Höhe 31px / radius 15px / 14px
Result-Date:   12px / weight 600 / uppercase / letter-spacing .6px / --accent
Result-Text:   16px
Result-Meta:   14px / --text-3
```

---

## Bottom Sheet (Mobile)

```
Radius:        18px 18px 0 0 (oben)
Border-Top:    1px solid --border
Transition:    translateY .3s cubic-bezier(.32,.72,0,1)
Handle:        40 × 4px / radius 2px / --border-2 / margin 11px auto
Item:          padding 15px 22px / 16px / --text-2 / gap 13px
Divider:       1px solid --border / margin 0 22px
Conn-Dot:      8 × 8px / radius 50%
```

---

## Datepicker

```
Panel:         308px / padding 18px / radius 13px
Shadow:        0 13px 44px rgba(0,0,0,.6)
Titel:         15px / weight 600
Nav-Button:    31 × 31px / radius 7px (identisch nav-btn)
Tag:           aspect-ratio 1 / 14px / radius 8px
Tag heute:     color --accent / weight 600
Tag gewählt:   bg --accent / color #fff
Jahr-Grid:     3 Spalten / gap 7px
Jahr-Button:   padding 9px 4px / 14px / radius 8px
```

---

## Mic-Button

```
Grösse:        48 × 48px
Radius:        50%
Border:        1.6px solid rgba(--accent, .4)
BG:            --accent-bg
Icon:          22 × 22px / stroke 1.8
Aktiv:         border --error, color --error, animation pulse .9s
```

---

## Toast

```
BG:            --surface-2
Border:        1px solid --border-2
Radius:        9px
Padding:       10px 18px
Font:          14px
Position:      fixed bottom 79px, horizontal zentriert
Transition:    opacity + translateY .25s ease
OK:            border rgba(--good, .4), color --good
Error:         border rgba(--error, .4), color --error
```

---

## Spinner

```
Grösse:        31 × 31px
Border:        2.6px solid --border-2
Accent:        border-top-color --accent
Animation:     spin .7s linear infinite
Overlay-BG:    rgba(26,26,26,.8)
```

---

## Layout

```
Einspaltiger Container: max-width 638px / padding 22px 18px 110px / gap 13px
Zweispaltig ab 1056px:  max-width 1210px / grid 1fr 1fr / column-gap 22px / padding 22px 26px 110px
```

---

## Übergänge (Zusammenfassung)

```
Standard:      all .15s
Farbe/Opacity: color .15s / opacity .3s ease
Mic-Pulse:     transform .9s ease infinite
Bottom-Sheet:  .3s cubic-bezier(.32,.72,0,1)
Toast:         .25s ease
Slider-Thumb:  transform .1s
```
