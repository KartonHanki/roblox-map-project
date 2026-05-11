# Liminal Mall — Style Guide PoC

**Cel dokumentu:** referencja stylistyczna dla AI przy budowie. Każdy prompt PoC odwołuje się do tych wartości.

---

## Paleta kolorów (RGB 0–255)

| Rola | RGB | Zastosowanie |
|---|---|---|
| **Mall Pink (neon główny)** | `255, 87, 200` | Główne neony, akcenty świetlne, logo |
| **Mall Cyan (neon wtórny)** | `87, 220, 255` | Neony pomocnicze, ekrany, fountain |
| **Floor Beige** | `212, 196, 170` | Podłoga lobby (stare lastryko) |
| **Wall Cream** | `230, 220, 200` | Ściany lobby (matowy beż) |
| **Ceiling White** | `245, 245, 240` | Sufit (płytki sufitowe) |
| **Trim Dark** | `45, 35, 50` | Listwy, kontury, słupy |
| **Fountain Tile** | `120, 200, 210` | Płytki fontanny (turkus retro) |
| **Wood Warm** | `155, 110, 75` | Info desk, ławki (drewno) |
| **Glass Tint** | `200, 230, 240` | Witryny sklepowe, drzwi szklane |
| **Door Metal** | `170, 175, 180` | Drzwi metalowe, futryny |

## Materiały Roblox

| Obiekt | Material | Powód |
|---|---|---|
| Podłoga | `Granite` | Imituje stare lastryko z plamami |
| Ściany | `Plaster` | Matowy gładki tynk |
| Sufit | `SmoothPlastic` | Płytki sufitowe |
| Neony (rury) | `Neon` | Świecące, główny vibe liminal |
| Fontanna (basen) | `Marble` | Połysk retro lat 90 |
| Info desk (blat) | `Wood` | Ciepło wśród zimnego mall'u |
| Słupy nośne | `Concrete` | Kontrast z gładkością ścian |
| Drzwi szklane | `Glass` | Transparency 0.3 |
| Drzwi metalowe | `Metal` | Reflectance 0.2 |

## Oświetlenie globalne

```lua
Lighting.Brightness = 1.5
Lighting.Ambient = Color3.fromRGB(180, 165, 200)  -- lekko fioletowy ambient
Lighting.OutdoorAmbient = Color3.fromRGB(100, 90, 120)
Lighting.ColorShift_Top = Color3.fromRGB(255, 200, 220)  -- różowawe światło z góry
Lighting.ColorShift_Bottom = Color3.fromRGB(150, 200, 255)  -- chłodne refleksy z dołu
Lighting.GlobalShadows = true
Lighting.ShadowSoftness = 0.3
Lighting.ClockTime = 14  -- "popołudnie" (nieważne, mall jest zamknięty)
```

## Atmosphere (efekt mgły/zamglenia)

```lua
Atmosphere.Density = 0.3
Atmosphere.Offset = 0.15
Atmosphere.Color = Color3.fromRGB(220, 200, 230)
Atmosphere.Decay = Color3.fromRGB(180, 160, 200)
Atmosphere.Glare = 0.2
Atmosphere.Haze = 1.5
```

**Dlaczego:** liminal aesthetic = lekko mglista, "podwodna" atmosfera. Bez tego mall wygląda jak sklepowy showroom.

## ColorCorrection (post-processing)

```lua
ColorCorrection.Brightness = -0.05
ColorCorrection.Contrast = 0.15
ColorCorrection.Saturation = -0.2  -- desaturacja, vintage feel
ColorCorrection.TintColor = Color3.fromRGB(255, 240, 230)  -- ciepły tint
```

## Bloom (efekt świecenia neonów)

```lua
Bloom.Intensity = 0.6
Bloom.Size = 24
Bloom.Threshold = 0.95
```

## Wymiary referencyjne (studs)

- **Lobby box (cały pokój):** 80 × 14 × 60 (X szerokość, Y wysokość, Z głębokość)
- **Grubość ścian:** 1
- **Wysokość sufitu:** 14 (typowy mall ma wysoki sufit)
- **Drzwi wejściowe:** 8 × 10 × 0.5
- **Fontanna (środek lobby):** Ø 16 (6 wysokość)
- **Info desk:** 12 × 4 × 4
- **Słup nośny:** 2 × 14 × 2 (4 sztuki, w narożnikach lub w środku)

## Anti-patterns (czego NIE robić)

- ❌ Saturated neon colors (np. RGB 255,0,0) — za bardzo "gaming", nie liminal
- ❌ Material `Plastic` na ścianach — wygląda zabawkowo
- ❌ ClockTime nocna (0–6) — gracz nie zobaczy szczegółów, atmosphere się gubi
- ❌ Brak Atmosphere — mall wygląda jak showroom IKEA
- ❌ Skybox standardowy — wymienić na neutralny grey lub fioletowy

## Skybox

```lua
-- Usuń domyślny Sky, dodaj nowy:
Sky.SkyboxBk = "rbxasset://textures/sky/skybox_neg_z.png"
Sky.SunAngularSize = 0  -- ukrywa słońce
Sky.MoonAngularSize = 0
-- Lub: jednolity szary kolor przez Sky.SkyboxXX = nil dla wszystkich
```

**Jeśli problem ze skyboxem** — zostaw domyślny. Atmosphere przykryje.

## Weppy PRO restrictions (Basic mode)

Zablokowane w naszej licencji (omijane przez wolniejsze API):
- mass_create → używaj create_with_props pojedynczo
- create_tree → twórz hierarchię przez Parent w create_with_props
- batch_execute → wykonuj akcje sekwencyjnie
- manage_lighting → używaj set_multiple zamiast tego
- execute_luau → zamiast tego twórz ServerScript z logiką w Source

Co działa w Basic mode (potwierdzone):
- create_with_props
- manage_properties.get_all / set_multiple
- query_instances
- delete_instances
