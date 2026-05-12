# Cut list

Generated from FreeCAD model. Bin-packed at 2.4m stock lengths, 3mm kerf per cut.

## Stock summary

| Stock | Pieces | 2.4m lengths | Avg offcut |
|---|---|---|---|
| 70×45 H3 treated pine (posts) | 4 | **4** | 685mm |
| 70×35 H3 treated pine (frame/doors/shelves) | 21 | **14** | 322mm |
| 90×45 H4 treated pine (floor bearers) | 2 | **2** | 400mm |
| 90×19 H3 decking (floor + shelf slats) | 33 | **12** | 284mm |

Cleats + astragal: rip from 70×35 offcuts (no extra stock).

## 70×45 H3 treated pine

| Piece | Length |
|---|---|
| Frame_PostBL | 1790mm |
| Frame_PostBR | 1790mm |
| Frame_PostFL | 1640mm |
| Frame_PostFR | 1640mm |

**4 pieces, 4 × 2.4m lengths** (1 post each).

## 70×35 H3 treated pine

| Piece | Length | Count |
|---|---|---|
| Frame_RailBotFront/Back, Frame_RailTopFront/Back | 1910mm | 4 |
| Shelf{2,3}_Batten{Front,Back} | 1910mm | 4 |
| Frame_BackCenter | 1720mm | 1 |
| Door{L,R}_Stile{Hinge,Latch} | 1497mm | 4 |
| Door{L,R}_Rail{Top,Bot} | 882mm | 4 |
| Frame_Rafter{Left,Right} | 715mm | 2 |
| Frame_RailBot{Left,Right} | 560mm | 2 |

**21 pieces, 14 × 2.4m lengths.**

Packing:
- 8 × (1910 + 490 offcut)
- 1 × (1720 + 560 + 117 offcut)
- 4 × (1497 + 882 + 18 offcut)
- 1 × (715 + 715 + 560 + 404 offcut)

## 90×45 H4 treated pine

| Piece | Length |
|---|---|
| Foundation_BearerFront | 2000mm |
| Foundation_BearerBack | 2000mm |

**2 pieces, 2 × 2.4m lengths.**

## 90×19 H3 decking

| Piece | Length | Count |
|---|---|---|
| Floor_Slat{1..5} | 1930mm | 5 |
| Shelf{2,3}_Slat{01..14} | 560mm | 28 |

**33 pieces, 12 × 2.4m lengths.**

Packing:
- 5 × (1930 + 470 offcut)
- 7 × (4 × 560 + 151 offcut)

## From offcuts

- **16 × shelf cleats** @ 560mm × 20×15mm — rip from 70×35 offcuts (~9m of cleat needed; one 2.4m of 70×35 ripped gives ~14m of 20×15 strips)
- **1 × astragal** @ 1497mm × 30×12mm — rip from a 1910 offcut

# Polycarbonate panels

| Panel | W × H (mm) | Type |
|---|---|---|
| Roof_Hatch | 2000 × 822 | Suntuf corrugated |
| Panel_BackL, Panel_BackR | 1650 × 930 | Twinwall 6mm |
| Panel_Left, Panel_Right | 1650 × 630 | Twinwall 6mm (5-sided, slope-cut top) |
| DoorL_Infill, DoorR_Infill | 1427 × 882 | Twinwall 6mm |

Side panels: cut as 1650×630 rectangle, then trim the upper edge to follow the roof slope (12° from horizontal). Front edge keeps 630mm height; back edge ends at full panel height.

Suggested twinwall sheet buy: depends on stock available locally. 2 × 2.1×1.2m sheets cover all flat panels with offcuts; or 4 × 1.05×0.7m smaller sheets.

Suntuf: 1 × 2.4m sheet (~860mm wide), cut to 822mm width × 2000mm length.

# Hardware

## Door hardware

- **6 × tee-hinges** (3 per door, ~150–200mm length) — face-mount on front
- **2 × 100mm barrel bolts** (top + bottom on inactive/left leaf — into roof frame and into bottom rail/floor)
- **1 × gate latch** (active/right leaf, latches against inactive leaf astragal)
- 2 × cabinet pulls or drilled finger-pulls (handles)

## Hatch hardware

- **2 × butt hinges** (~50–75mm, along back ridge into back top rail)
- **2 × hook-and-eye latches** (front-left and front-right corners, prevent lift)
- **1 × casement window stay** (holds hatch open at adjustable angle)

## Foundation

- **6 × 400×400×40mm concrete pavers**, bedded on sand
- ~50kg bag paving sand

## Fasteners (estimate)

- ~100 × 8g 65mm exterior screws (frame joints)
- ~50 × 8g 50mm exterior screws (slats, trims)
- ~40 × polycarbonate cap screws + neoprene washers
- 4–6 × galvanised 90° corner braces (front top corners + wherever rails feel weak under shake test)
- Roof profile foam fillers (1 strip per Suntuf ridge)
- Aluminium U-channel for twinwall cut edges (~6m)

# Joinery summary

All joints butt + screw. No mortice/tenon, no halving. Pocket screws optional for door frames if a Kreg jig is borrowable.

Critical joint locations:
- Posts ↔ bottom rails: 2 × 65mm screws toe-nailed through rail end into post end-grain. Add corner brace.
- Posts ↔ top rails: same.
- Top rails ↔ rafters: rafters sit on top of front + back top rails. 2 screws each end through rafter into rail.
- Door frame stiles ↔ rails: pocket screws (preferred) OR 2 × 65mm through stile face into rail end. Glue.
- Bearers ↔ bottom rails: 4–5 × 65mm screws up through bearer into bottom rail.
- Centre back beam ↔ top/bottom rails: butt + 2 × 65mm screws each end.
- Cleats ↔ side panels/posts: 3 × 50mm screws per cleat through side panel into post (front+back+middle).
- Tee-hinges: through hinge plate into door stile (front face) + post (front face).
