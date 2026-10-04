# How the Beanstory model works

Beanstory turns a bag of coffee into a small set of **relative properties** (0–100), and then into a **V60 recipe**. This page explains each step and how confident each rule is.

Confidence labels used below:

- **Established:** consistently reported in coffee science literature.
- **Supported:** widely accepted, with some evidence; magnitudes are approximate.
- **Heuristic:** a reasonable generalisation or rule of thumb; treat as a tendency.

All code referenced here is in `index.html` (`model()`, `composition()`, `recipe()`).

---

## 1. Inputs

| Input | Range | Notes |
|---|---|---|
| Origin | 27 countries | Used for latitude only |
| Altitude | 600–2,400 m | |
| Processing | 14 methods | Each has fruit contact, fermentation intensity, wall weakening, sugar uptake and acid shifts |
| Roast | Agtron 25–95 | SCA colour tiles: 95 very light · 75 moderately light · 65 light-medium · 55 medium · 45 medium-dark · 35 dark · 25 very dark |
| Days since roast | 2–60 | Drives CO₂ |
| Varietals | 31, multi-select | Bean shape follows the first pick; flavour traits average across picks |

## 2. Derived properties

### Effective altitude: *supported*

Altitude is a proxy for growing temperature, and temperature also depends on latitude. The model shifts altitude by **~30 m per degree** of latitude away from ~10°, capped at ±300 m. For example, 1,000 m in Brazil (~20°S) behaves like ~1,300 m; 1,900 m in Kenya (~0.5°) like ~1,600 m. This is a rough rule of thumb.

### Density: *supported*

Slower ripening at cooler sites gives harder, denser beans ("strictly hard bean" grading). Density rises with effective altitude. Processing adds small adjustments (monsooned, wet-hulled and barrel-aged beans are softer, since they take up moisture after processing) and fermentation weakening lowers it slightly.

### Porosity, brittleness/fines, solubility: *established for roast, supported for the rest*

Roasting drives out water, expands the bean and builds internal pressure. Darker roasts are markedly more porous and brittle, shatter into more fines, and extract faster. Roast degree carries the most weight; low density and processing are secondary.

### Acids: *mixed*

| Acid | Behaviour in the model | Confidence |
|---|---|---|
| Chlorogenic | ~6–8 % of green arabica (more in robusta), breaks down steadily with roasting | Established |
| Quinic | Formed from chlorogenic acid; rises with roast; perceived as bitter-astringent | Established |
| Citric, malic | Higher at altitude; fade with roast | Supported |
| Acetic | Formed from sugars during roasting, peaking around light-medium; also from fermentation | Supported |
| Phosphoric | Mineral acid, heat-stable; varietal-linked (e.g. SL28, SL9) | Heuristic |
| Lactic | From lactic fermentation (cultured, anaerobic) | Supported |

### CO₂ and freshness: *established*

CO₂ forms during roasting (more in darker roasts) and is mostly lost over the first 2–3 weeks. It drives bloom size and length.

### Oil on the surface: *established*

Oils migrate to the surface once roasting passes second crack (roughly Agtron < 45).

### Varietal traits: *heuristic*

Each varietal has rough tendencies (acidity, sweetness, body, lipids, phosphoric acid, bean size and shape, cherry colour). These are generalisations; origin, farm practice and processing usually matter more.

### Barrel-aged / infused: *heuristic*

Barrel aging happens after normal processing: green beans rest for weeks in an emptied whiskey, rum or wine barrel, take up some moisture and wood or spirit aromatics, and are dried again. The model treats it as a milder version of monsooning: slightly lower density and acidity, more aroma (esters), a touch more body, and a slightly cooler, coarser brew. The base process (washed, honey…) is not tracked separately.

## 3. Single-cell composition

Shares of roasted-bean dry mass, built from published ranges for roasted arabica (Clarke & Vitzthum; Illy & Viani; Farah 2012):

| Component | Range used | Driven by |
|---|---|---|
| Cell walls (polysaccharides) | ~40–50 % | Density (up), roast and weakening (down) |
| Lipids | ~13–17 % (robusta ~10 %) | Varietal |
| Melanoidins | ~10–30 % | Roast (up) |
| Sugars (mainly sucrose) | 6–9 % green, falling to < 1 % dark | Roast (down, exponentially), fruit contact (up) |
| Chlorogenic acids | ~6 % green, falling to ~1 % dark | Roast (down) |
| Other acids | ~2–4 % | Altitude, process, varietal |
| CO₂ | ~0.5–2 % | Roast, freshness |
| Caffeine | ~1.2 % (robusta ~2.2 %) | Species |
| Proteins and other | remainder | |

These are illustrative trends, not measurements of a specific bag.

## 4. Recipe rules

| Parameter | Rule | Basis |
|---|---|---|
| Temperature | Base curve on Agtron: light 95–97 °C, light-medium ~93 °C, medium ~90 °C, dark 85–88 °C. Then ±1–2 °C for effective altitude and processing (fermented coffees slightly cooler) | Mainstream specialty guidance |
| Grind | Grind index from roast (darker = coarser), density (denser = finer), processing (fragile = coarser) and age (older = finer). Converted per grinder | Established direction; magnitudes heuristic |
| Ratio | 1:14.5–1:17: more water for light, dense coffees, less for dark, heavy ones | SCA Golden Cup ~1:16–1:18 |
| Bloom | 2–3.5× dose; 25–50 s, longer with more CO₂ | Common practice |
| Pours and agitation | 1–5 pours. Hard, light beans get more pours and more agitation; brittle, fines-prone beds get fewer, calmer, centre pours | Heuristic based on fines behaviour |
| Total time | Targets about 2:00 (very dark) to 3:30 (dense, light) | Typical V60-02 range for 12–20 g |

### Grinder conversions: *heuristic*

Each grinder has a typical medium V60 setting and a step size. These are community starting points and vary between units and burr sets.

## 5. Not modelled (yet)

- Water hardness and alkalinity
- Filter paper type and flow rate
- Grind distribution (bimodality) per grinder
- Rest time after roasting beyond CO₂ (aroma staling)
