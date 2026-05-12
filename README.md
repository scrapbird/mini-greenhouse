# mini-greenhouse

Greenhouse cabinet for the yard. 2m W × 1.8m H × 0.7m D. Timber frame, polycarbonate cladding, slatted raised floor, double doors, vent hatch in roof.

Plans drafted in **FreeCAD**, with Claude Code assisting via the [`freecad-mcp`](https://github.com/neka-nat/freecad-mcp) MCP server.

See `CLAUDE.md` for locked specs and conventions.

---

## Setup

Target: Ubuntu 25.10. Adapt paths if on another distro.

### 1. Install FreeCAD

Pick one. AppImage is recommended — it's the cleanest way to get current FreeCAD 1.x without snap quirks that complicate the MCP addon path.

**FreeCAD 1.1.1 AppImage** — installed at `~/Software/FreeCAD-1.1.1.AppImage`.

How it was installed:

```bash
mkdir -p ~/Software
wget -O ~/Software/FreeCAD-1.1.1.AppImage \
  https://github.com/FreeCAD/FreeCAD/releases/download/1.1.1/FreeCAD_1.1.1-Linux-x86_64-py311.AppImage
chmod +x ~/Software/FreeCAD-1.1.1.AppImage
```

Run it:

```bash
~/Software/FreeCAD-1.1.1.AppImage
```

Optional — make it launchable from the app menu:

```bash
~/Software/FreeCAD-1.1.1.AppImage --appimage-extract-and-run --install
```

Alternatives (skip — AppImage already done):
- `sudo apt install freecad` — older version, fine if AppImage breaks
- `sudo snap install freecad` — works but uses `~/snap/freecad/common/Mod/` for addons

### 2. Install the `freecad-mcp` addon

```bash
git clone https://github.com/neka-nat/freecad-mcp.git ~/src/freecad-mcp
```

Copy the addon into FreeCAD's `Mod/` directory. Path depends on how you installed FreeCAD:

For our setup (FreeCAD 1.1 AppImage):

```bash
mkdir -p ~/.local/share/FreeCAD/v1-1/Mod/
cp -r ~/src/freecad-mcp/addon/FreeCADMCP ~/.local/share/FreeCAD/v1-1/Mod/
```

Other install methods would use different Mod paths: `~/.FreeCAD/Mod/` for older Ubuntu builds, `~/snap/freecad/common/Mod/` for snap.

Restart FreeCAD. From the Workbench dropdown pick **MCP Addon**, then **Start RPC Server** from the FreeCAD MCP toolbar. Tick **Auto-Start Server** if you want it on every launch.

### 3. `uvx` (Python tool runner)

Already installed on this machine (`/home/user/.local/bin/uvx`). If you ever need it elsewhere:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 4. Wire the MCP server into Claude Code

From the project directory:

```bash
cd ~/Development/mini-greenhouse
claude mcp add freecad -- uvx freecad-mcp
```

This writes a `.mcp.json` in the project so the server is available whenever Claude Code runs here. Verify with:

```bash
claude mcp list
```

FreeCAD must be running with the RPC server started before Claude can talk to it.

### 5. Smoke test

In Claude Code, with FreeCAD open and the RPC server started, ask: *"Create a new FreeCAD document called `greenhouse` and add a 2000×700×1800 box as a placeholder envelope."* If the box appears in FreeCAD, the loop is working.

---

## Repo layout (will fill in as we go)

```
mini-greenhouse/
├── CLAUDE.md           # Specs + conventions for Claude
├── README.md           # This file
├── Greenhouse.FCStd    # FreeCAD model (canonical = closed state)
├── macros/             # FCMacros — open/close toggles
├── notes/              # Cut list + decisions
└── drawings/           # Exported PDFs (later)
```

## Open / close toggle

Two macros in `macros/` toggle the doors + roof hatch between closed and open states. They read all geometry from the `Dims` spreadsheet, so they stay in sync if you change posts/depth/etc.

- `gh_open.FCMacro` — swings doors out 90°, lifts hatch 60°, deploys the prop
- `gh_close.FCMacro` — reverses all of the above

To make them appear in FreeCAD's **Macro → Macros…** menu, symlink them into FreeCAD's macro dir:

```bash
ln -s ~/Development/mini-greenhouse/macros/gh_open.FCMacro ~/.local/share/FreeCAD/Macro/
ln -s ~/Development/mini-greenhouse/macros/gh_close.FCMacro ~/.local/share/FreeCAD/Macro/
```

Then in FreeCAD: **Macro → Macros…**, pick one, Execute. Or run via the FreeCAD Python console:

```python
exec(open("/home/user/Development/mini-greenhouse/macros/gh_open.FCMacro").read())
```

Macros are idempotent — running `gh_open` twice is a no-op (it detects the `Hatch_Prop` object).

---

---

## Materials

Prices NZD, rough Mitre 10 NZ ballparks — Mitre 10 NZ doesn't publish online pricing; confirm in-store. Original budget cap was NZ$550; current realistic estimate ~NZ$1,200 (driven by pallet-style shelves needing many more pieces than first sketched).

Detailed cut list with per-product links: see [notes/cut-list.md](notes/cut-list.md).

### Timber

H3 = above-ground outdoor treated. H4 = ground-contact treated. Treated radiata pine: cheap, everywhere, rated for outdoor weather, takes stain well.

| Use | Stock | Why |
|---|---|---|
| 4× corner posts | **70×45 H3 SG8 KD pine** | Carries shelf loads + door weight. 70mm depth gives hinge screws bite. |
| Frame rails, rafters, door frames, shelf battens, back centre beam | **70×35 H3 SG8 pine** | Cheaper + lighter than 70×45. Plenty for this span. |
| Floor bearers (under cabinet, on pavers) | **90×45 H4 wet SG8 pine** | Ground-contact — first thing to rot if you skimp. |
| Floor slats + shelf slats | **90×21 H3 radiata decking** | Pre-milled smooth top, wet-duty rated. (Mitre 10 NZ stock is 90×21, not 90×19.) |

Qty + cost (2.4m lengths):

- 4× 70×45 H3 → ~$64
- 17× 70×35 H3 → ~$187
- 2× 90×45 H4 → ~$40
- 12× 90×21 decking → ~$216

**Timber subtotal: ~$507**

Why higher than first sketch: pallet-style shelves use 4 long 70×35 battens + 28 short 90×21 slats. That alone is ~9 extra lengths of timber.

### Polycarbonate

| Type | Pros | Cons | Best use |
|---|---|---|---|
| **Corrugated (Sunclear / Suntuf)** | Cheap, purpose-built for roofing, sheds water naturally, easy to screw with cap screws | Profile only suits roofs/walls with same profile; needs foam closure under ridges | **Roof** ✓ |
| **Twinwall (Ampelite, 6mm)** | Insulating air gap = warmer greenhouse, light-diffusing, externally flat, cheaper than solid | Channels need taping/closing at cut ends; not crystal-clear | **Sides + back + doors** ✓ |
| **Solid clear sheet (2–3mm)** | Crystal-clear, premium look | ~2× price of twinwall, no insulation, scratches show | Skip for this build |

Recommendation: **corrugated roof + twinwall everywhere else.**

- 1× Sunclear corrugated 2.4m clear → ~$40
- 2× Ampelite twinwall 1050×2500mm clear (back + side + door panels cut from these) → ~$140
- Aluminium U-channel edging (3 × 1m) for cut twinwall ends → ~$25
- Foam closure strip for Suntuf profile → ~$20

**Polycarbonate subtotal: ~$225**

### Foundation

**6× 400×400×40mm concrete pavers, levelled on a sand bed, with H4 bearers spanning across them.**

- Cheap, no concreting, fully removable
- Lifts timber clear of wet ground
- Forgiving of slight ground unevenness — just rake sand flat
- Footprint: pavers overhang cabinet front and back by ~178mm each side (front overhang doubles as a doorstep)

Skip: concrete piers (overkill, permanent), galvanised post stirrups (overkill, $$$), direct-on-ground (rot bomb).

- 6× 400×400 pavers @ ~$8 → ~$48
- 1× paving sand 20kg → ~$8

**Foundation subtotal: ~$56**

### Door hardware

Pattern: **inactive door (left) bolted top+bottom**, acts as a fixed jamb when shut. **Active door (right) latches** against the inactive leaf's astragal. Astragal strip on inactive leaf seals the meeting edge.

| Item | Count | Price |
|---|---|---|
| Tee-hinges, 150mm galvanised (face-mount, no rebating) | 3 per leaf × 2 = 6 | ~$108 |
| 100mm barrel bolts (inactive leaf top + bottom into roof frame and floor) | 2 | ~$30 |
| D-type gate latch (active leaf) | 1 | ~$18 |
| Handles | 2 | ~$10 (or $0 if drilled finger-pulls) |

**Door hardware subtotal: ~$166**

### Hatch hardware

Hatch hinges along the back ridge, lifts at front. Two hook-and-eye latches at front-left and front-right pin it shut against wind lift. A telescopic window stay holds it open at multiple angles.

| Item | Count | Price |
|---|---|---|
| Butt hinges 50mm zinc (2-pack) | 1 pair | ~$10 |
| Hook & eye latches 100mm (2-pack) | 1 pack | ~$10 |
| Telescopic window stay (multi-position) | 1 | ~$20 |

**Hatch hardware subtotal: ~$40**

### Fasteners

- Otter Treated Pine Screws 10g × 65mm, pack of 500 (frame joints — must be treated-pine rated for H3/H4 stock) → ~$45
- Otter Heavy Duty Timber Screws 14g × 50mm, pack of 100 (slats, trims) → ~$22
- Otter Polycarbonate Roof Screws 12g × 50mm, pack of 50 → ~$25
- 4–6× Zenith corner brace brackets, zinc plated → ~$25
- Aluminium U-channel 20×20×1.5mm × 2.4m (twinwall edges) → ~$25
- Ampelite corrugated eave filler 760mm, 2-pack → ~$20

**Fasteners subtotal: ~$162**

### Finish

Water-based decking stain (DECKMAX or Wattyl — Cabot's range is limited at Mitre 10 NZ). Apply two coats after assembly, before fitting polycarbonate.

- 10L water-based decking stain — Honey Oak or similar dark tone → ~$100 (overkill for this build; if 5L sold in-store, halve)
- Brushes (75mm + small) → ~$15

**Finish subtotal: ~$115**

### Shelves

**Timber cleats** screwed into the inner faces of the side walls — one continuous batten per height per side, shelf sits on top. 8 height positions, first at 300mm from frame bottom, 175mm spacing (last at 1525mm). 2 pallet-style shelves fitted at positions 4 and 7; can be moved or a third added.

Cleats: 20×15mm offcuts of 70×35 stock, full depth (560mm). 16 cleats total → free from frame offcuts.

Each shelf: 2× 70×35 cross-battens (1910mm long) + 14× 90×21 decking slats (560mm each) screwed on top with ~50mm gaps. Drains like the floor.

**Shelf cost: ~$0** beyond timber already counted.

### Total estimate

| | NZD |
|---|---|
| Timber | 507 |
| Polycarbonate | 225 |
| Foundation | 56 |
| Door hardware | 166 |
| Hatch hardware | 40 |
| Fasteners | 162 |
| Finish | 115 |
| **Subtotal** | **~$1,271** |

Over original NZ$550 target by ~$690. Levers to claw back:

- Drop to 1 shelf instead of 2 → save ~$90 (decking + battens)
- Use 5L stain instead of 10L → save ~$50
- Untreated pine for shelf slats (out of weather under the roof) → save ~$60
- Substitute corrugated for the back panel (no need for clear flat there) → save ~$50
- Mix Marketplace / Trade Me offcuts for posts and bearers → save ~$80+
- Skip aluminium U-channel; tape twinwall ends + timber trim → save ~$25

Realistic floor with corners cut: **~$900 NZD**.

---

## Tools

You have none → borrow these. Hand-saw substitution is fine but slow; circular saw is the one tool that would make the biggest difference if borrowable.

**Must have (borrow or buy cheap):**

- Tape measure
- Pencil + combination square
- Hand saw (or circular saw)
- Cordless drill/driver + drill bits (3mm pilot, countersink)
- Spirit level (600mm)
- 2× bar clamps or quick-clamps
- Tin snips OR fine-tooth blade (32+ tpi) for cutting polycarbonate
- Safety glasses, dust mask, ear protection

**Nice to have:**

- Mitre box (for square hand-cuts) — ~$15 if borrowing not possible
- Pocket-hole jig (Kreg or generic) — speeds joinery massively, ~$40 cheap end
- Orbital sander — ~$60 if buying; sandpaper + block works

---

## Open questions

Still need a call on:

- Buy a basic toolkit vs. commit to borrowing? (affects budget headroom)
- Want me to pre-stain timber before assembly, or after? (before = no missed spots; after = easier touchup)

Resolved:

- ~~Site orientation~~ → doors face afternoon sun (west)
