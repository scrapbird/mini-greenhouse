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
├── plans/              # FreeCAD .FCStd files
├── drawings/           # Exported PDFs / cut lists
└── notes/              # Decisions, sketches, photos
```

---

---

## Materials

All prices AUD, rough Bunnings/Mitre10 ballparks — verify in-store. Budget cap: **$500**.

### Timber

H3 = above-ground outdoor treated. H4 = ground-contact treated. Treated pine is the right call here: cheap, available everywhere, rated for outdoor weather, takes stain well.

| Use | Recommended | Why |
|---|---|---|
| 4× corner posts (1.8m) | **70×45 H3 treated pine** | Carries shelf loads + door weight. Extra width gives screw bite for hinges. |
| Frame rails, roof rafters, door frames | **70×35 H3 treated pine** | Plenty for a 0.7m-deep cabinet. Cheaper + lighter than 70×45. |
| Floor bearers (sit on pavers) | **90×45 H4 treated pine** | Ground-contact — this is the bit that'll rot first if you skimp. |
| Floor slats + shelf slats | **90×19 H3 treated pine decking** | Already milled with a smooth top, designed for wet duty. Gaps between slats drain. |

Rough qty + cost (2.4m lengths, ~$8–12 each):

- 4× 70×45 H3 @ 2.4m → ~$48
- 8× 70×35 H3 @ 2.4m → ~$72
- 2× 90×45 H4 @ 2.4m (bearers, cut down) → ~$24
- 4× 90×19 H3 decking @ 2.4m (floor + shelves) → ~$40

**Timber subtotal: ~$184**

### Polycarbonate

| Type | Pros | Cons | Best use |
|---|---|---|---|
| **Corrugated (Suntuf/Sunlite)** | Cheap (~$30–40/sheet), purpose-built for roofing, sheds water naturally, easy to screw with cap screws, every Bunnings stocks it | Profile only suits roofs/walls with same profile; needs foam profile fillers under ridges | **Roof** ✓ |
| **Twinwall (multiwall, 4–6mm)** | Insulating air gap = warmer greenhouse, light-diffusing (good for plants), externally flat, cheaper than solid, lighter | Channels can fungus up if ends aren't taped/closed; not crystal-clear | **Sides + back + doors** ✓ (best $/m²) |
| **Solid clear sheet (2–3mm)** | Crystal-clear view of plants, premium look, easy to cut with fine blade | ~2× the price of twinwall, no insulation, scratches show | Sides if budget allows + you want shop-window look |
| **Solid tinted/opal** | Cuts glare + heat | Less light = slower growth | Skip for this build |

Recommendation: **corrugated roof + twinwall everywhere else.** Twinwall externally has the flat finish you want; the ribs are internal. Costs roughly half what solid sheet does.

- 1× Suntuf corrugated clear ~2.4×0.86m → ~$40
- 4× twinwall 6mm ~2.1×0.7m → ~$140–180 (depends on thickness/brand)
- Aluminium U-channel edging for cut twinwall ends + roof profile foam fillers → ~$25

**Polycarbonate subtotal: ~$210**

### Foundation

**Recommendation: 6× 400×400mm concrete pavers, levelled on a sand bed, with H4 bearers spanning across them.**

- Cheap (~$5 each = $30), no concreting, fully removable
- Lifts timber clear of wet ground (timber-on-dirt is what kills these things)
- Forgiving of slight ground unevenness — just rake sand flat
- Footprint 2.0×0.7m → two rows of three pavers spaced under bearers

Skip: concrete piers (overkill, permanent), galvanised post stirrups (overkill, $$$), direct-on-ground (rot bomb).

**Foundation subtotal: ~$30** (pavers + a bag of paving sand)

### Door hardware

You said: hinges, basic latch, deadbolts at top of each door.

| Item | Pick | Price |
|---|---|---|
| Hinges | 2× pairs zinc-plated tee-hinges (150–200mm) — gate-style, look right on this build, mount on face so no rebating needed | ~$8–12/pair → **~$20** |
| Latch (active door to inactive) | Galvanised gate latch or simple hook-and-eye | ~$8–10 |
| Top bolts (one per door) | 2× 100mm barrel/tower bolts, galvanised — bolt up into the roof frame | ~$6 each → **~$12** |
| Bottom bolt (inactive leaf) | 1× 100mm barrel bolt down into floor frame | ~$6 |
| Handles | 2× cabinet pulls or just drilled finger-holes (free) | $0–15 |

**Door hardware subtotal: ~$50–60**

Pattern: inactive door is bolted top+bottom (acts as a fixed jamb when shut). Active door latches against it. Top bolt on the active door is optional belt-and-braces.

### Roof hatch prop

Manual prop options:

| Option | Price | Notes |
|---|---|---|
| **Casement window stay** (zinc, multi-position notches) | ~$10–15 | Recommended — adjustable to multiple opening heights, one-handed |
| Wooden prop stick + cup hook | ~$2 (or scrap) | Crude but works. Fixed positions only. |
| Folding cabinet door stay | ~$8–12 | Holds at one angle, no choice of vent amount |
| Friction window stay | ~$15–25 | Smooth, holds at any angle, fiddly to install |

**Pick: casement window stay (~$12).** Pair with two small butt hinges for the hatch itself (~$8).

**Hatch hardware subtotal: ~$20**

### Fasteners

- 1× box 8g 65mm exterior screws (galv or stainless) — main frame → ~$18
- 1× box 8g 50mm exterior screws — slats, trims → ~$15
- ~30× polycarbonate cap screws with neoprene washers (purpose-made, won't crack the sheet) → ~$15
- 4–6× 90° galvanised corner braces for stress points → ~$15
- Roof profile foam fillers (suit Suntuf profile) → ~$12

**Fasteners subtotal: ~$75**

### Finish

**Cabot's Deck & Exterior Stain**, water-based, tinted **Jarrah** or **Walnut** for dark — ~$45 for 2L (plenty for this size).

Apply two coats after assembly, before fitting polycarbonate. Water-based cleans up with water (no turps), low-smell, recoatable in hours.

- Stain → $45
- Cheap synthetic brush (75mm) + small brush for corners → $15

**Finish subtotal: ~$60**

### Shelves

**Timber cleats** screwed into the inner faces of the side walls — one continuous batten per height per side, shelf sits on top. 8 height positions, first at 300mm from frame bottom, 175mm spacing (last at 1525mm). 3 shelves slotted into any 3 of the 8 positions; lift out for cleaning / rearranging tall plants.

Cleats: 20×15mm offcuts of 70×35 stock, full depth (610mm). 16 cleats total → uses ~10m of offcuts (free from frame stock).

Shelves: 90×19 decking, full inside width less cleat clearance, full inside depth.

**Shelf cost: ~$0** (offcuts only)

### Total estimate

| | $ |
|---|---|
| Timber | 184 |
| Polycarbonate | 210 |
| Foundation | 30 |
| Door hardware | 55 |
| Hatch hardware | 20 |
| Fasteners | 75 |
| Finish | 60 |
| Shelf pegs | 5 |
| **Subtotal** | **~$639** |

Over budget by ~$140. Levers to pull:

- Drop twinwall thickness 6mm → 4mm: saves ~$40
- Use thinner Suntuf or one less sheet via tighter cuts: saves ~$10
- Skip aluminium edging; use timber trim with sealant: saves ~$25
- Single-coat finish, smaller can: saves ~$20
- Reuse/scrounge offcut timber, free pavers off Marketplace: saves ~$40+

Realistically achievable: **~$500** with a bit of scrounging. Tight but doable.

---

## Tools

You have none → borrow these. Hand-saw substitution is fine but slow; circular saw is the one tool that would make the biggest difference if borrowable.

**Must have (borrow or buy cheap):**

- Tape measure
- Pencil + combination square
- Hand saw (or circular saw)
- Cordless drill/driver + drill bits (3mm pilot, 5mm shelf pin, countersink)
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
