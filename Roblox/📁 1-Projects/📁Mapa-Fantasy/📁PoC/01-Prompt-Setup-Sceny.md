# PoC Prompt 1 — Setup sceny + lobby box

**Cel:** Zbudować szkielet pokoju (ściany, podłoga, sufit) + skonfigurować oświetlenie i atmosphere zgodnie ze stylem Liminal Mall.

**Czas oczekiwany:** 5-10 minut Claude Code

**Kryteria sukcesu:**
- Lobby box 80×14×60 widoczny w Studio
- Wnętrze ma wymiary około 78×13×58 (ściany grubości 1)
- Globalne oświetlenie ma fioletowo-różowy ambient
- Atmosphere stworzone, mall ma "mglistą" estetykę
- Skybox/lighting nie pokazuje słońca/dnia

---

## Prompt do wklejenia w Claude Code

```
Buduj pierwszy etap PoC pokoju "Mall Lobby" zgodnie z dokumentem stylistycznym Liminal Mall (paleta, materiały, oświetlenie). Wykonaj WSZYSTKIE poniższe kroki w jednej sesji, batch'ując operacje przez create_with_props gdzie to możliwe.

KROK 1 — Cleanup
Usuń z Workspace wszystkie obiekty oprócz Baseplate i SpawnLocation. Jeśli istnieje WeppyTestCube — usuń go.

KROK 2 — Folder structure
W Workspace utwórz Model o nazwie "MallLobby" z dziećmi (Folder lub Model):
- "Walls" (Folder)
- "Floor" (Folder)
- "Ceiling" (Folder)
- "Pillars" (Folder)
- "Lighting" (Folder na PointLight'y)

KROK 3 — Floor (podłoga)
Stwórz Part w MallLobby/Floor:
- Name: "MainFloor"
- Position: Vector3.new(0, 0, 0)
- Size: Vector3.new(80, 1, 60)
- Material: Enum.Material.Granite
- Color: Color3.fromRGB(212, 196, 170)
- Anchored: true
- TopSurface i BottomSurface: Smooth (jeśli da się ustawić)

KROK 4 — Walls (ściany)
Cztery ściany dookoła podłogi. Wszystkie:
- Material: Enum.Material.Plaster
- Color: Color3.fromRGB(230, 220, 200)
- Anchored: true
- W folderze MallLobby/Walls

Wymiary i pozycje:
1. "WallNorth" — Position (0, 7.5, -30), Size (80, 14, 1)
2. "WallSouth" — Position (0, 7.5, 30), Size (80, 14, 1)
3. "WallEast" — Position (40, 7.5, 0), Size (1, 14, 60)
4. "WallWest" — Position (-40, 7.5, 0), Size (1, 14, 60)

KROK 5 — Ceiling (sufit)
Stwórz Part w MallLobby/Ceiling:
- Name: "MainCeiling"
- Position: Vector3.new(0, 14.5, 0)
- Size: Vector3.new(80, 1, 60)
- Material: Enum.Material.SmoothPlastic
- Color: Color3.fromRGB(245, 245, 240)
- Anchored: true

KROK 6 — Pillars (słupy nośne — 4 sztuki, betonowe)
Cztery słupy w narożnikach wewnętrznych lobby. Wszystkie:
- Material: Enum.Material.Concrete
- Color: Color3.fromRGB(45, 35, 50)
- Size: Vector3.new(2, 14, 2)
- Anchored: true
- W folderze MallLobby/Pillars

Pozycje (wewnątrz pokoju, blisko narożników):
1. "PillarNE" — Position (35, 7.5, -25)
2. "PillarNW" — Position (-35, 7.5, -25)
3. "PillarSE" — Position (35, 7.5, 25)
4. "PillarSW" — Position (-35, 7.5, 25)

KROK 7 — SpawnLocation
Przesuń (lub stwórz na nowo) SpawnLocation w pozycję Vector3.new(0, 1, 25) — gracz spawnuje przy południowej ścianie, twarzą do wnętrza lobby. Anchored: true.

KROK 8 — Globalne oświetlenie (Lighting service)
Ustaw właściwości w Lighting:
- Brightness = 1.5
- Ambient = Color3.fromRGB(180, 165, 200)
- OutdoorAmbient = Color3.fromRGB(100, 90, 120)
- ColorShift_Top = Color3.fromRGB(255, 200, 220)
- ColorShift_Bottom = Color3.fromRGB(150, 200, 255)
- GlobalShadows = true
- ShadowSoftness = 0.3
- ClockTime = 14

KROK 9 — Atmosphere
Sprawdź czy w Lighting istnieje Atmosphere. Jeśli nie — stwórz. Ustaw:
- Density = 0.3
- Offset = 0.15
- Color = Color3.fromRGB(220, 200, 230)
- Decay = Color3.fromRGB(180, 160, 200)
- Glare = 0.2
- Haze = 1.5

KROK 10 — ColorCorrection (post-processing)
W Lighting dodaj ColorCorrectionEffect (jeśli nie istnieje):
- Brightness = -0.05
- Contrast = 0.15
- Saturation = -0.2
- TintColor = Color3.fromRGB(255, 240, 230)

KROK 11 — Bloom (post-processing)
W Lighting dodaj BloomEffect (jeśli nie istnieje):
- Intensity = 0.6
- Size = 24
- Threshold = 0.95

KROK 12 — Raport końcowy
Po wszystkim wypisz:
- Listę wszystkich utworzonych obiektów (ścieżka + nazwa)
- Stan Lighting (wszystkie zmienione właściwości)
- Czy któryś krok nie wykonał się i dlaczego
- Sugestię co zrobić dalej

WAŻNE:
- Wszystkie obiekty Anchored: true
- Używaj batch creation (create_with_props) gdzie się da
- Nie dodawaj nic poza listą — koloru świateł, dodatkowych dekoracji etc.
- Po każdym kroku możesz krótko potwierdzić "✓ Krok X zrobiony"
```

---

## Po wykonaniu — twoja weryfikacja

1. **Wejdź do Studio** — kliknij Play (F5) lub przycisk Play
2. **Spawnujesz?** Postać powinna pojawić się przy południowej ścianie
3. **Atmosfera?** Wnętrze ma być lekko mgliste, fioletowo-różowy ambient
4. **Wyjdź z Play** (Esc lub przycisk Stop)

**Jeśli coś nie działa wizualnie:** zrób screenshot wnętrza + napisz co nie pasuje. Mogę dostosować kolory/wartości.

**Jeśli wszystko OK:** commit do GitHub i przechodzimy do Promptu 2.

## Commit message po sukcesie

```
PoC step 1: Mall Lobby skeleton + lighting setup

- Lobby box 80x14x60 (granite floor, plaster walls, smooth ceiling)
- 4 concrete pillars
- Liminal mall lighting (purple/pink ambient + atmosphere)
- ColorCorrection + Bloom post-processing
```

W terminalu (po sukcesie):
```cmd
git add .
git commit -m "PoC step 1: Mall Lobby skeleton + lighting setup"
git push
```

**Uwaga:** plik `.rbxl` powinien być w `.gitignore`. Commit nie będzie zawierał samego place'a (Studio zapis lokalnie). Jeśli chcesz mieć backup `.rbxl` — kopiuj ręcznie do `places/backup/` po każdym dużym kroku.
