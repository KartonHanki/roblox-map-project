# PoC Prompt 2 — Detale liminal mall

**Cel:** Wypełnić lobby elementami które dają liminal aesthetic — neony, fontanna, info desk, mall directory. Wszystko z prymitywów Part.

**Czas oczekiwany:** 10-15 minut Claude Code

**Kryteria sukcesu:**
- Fontanna w środku lobby z neonowym wnętrzem
- Info desk z drewnianym blatem przy zachodniej ścianie
- Mall directory (płyta z neonem) przy wschodniej ścianie
- 6 neonowych pasów świetlnych (rury) — różowe + cyjanowe
- Lobby wygląda jak zamknięty mall z lat 90, nie jak pusta kostka

**Wymaganie:** Prompt 1 musi być wykonany. Lobby skeleton + lighting muszą być gotowe.

---

## Prompt do wklejenia w Claude Code

```
Buduj drugi etap PoC — detale Liminal Mall. Lobby skeleton z poprzedniego promptu jest gotowy. Teraz wypełniamy go elementami estetycznymi.

KROK 1 — Folder structure
W MallLobby utwórz dodatkowe foldery:
- "Fountain"
- "InfoDesk"
- "MallDirectory"
- "NeonStrips"

KROK 2 — Neonowe pasy świetlne na suficie (6 sztuk)
Sześć cylindrycznych "rur" neonowych biegnących równolegle do osi X (ze wschodu na zachód) na suficie. Wszystkie:
- Material: Enum.Material.Neon
- Anchored: true
- Shape: Cylinder (jeśli Weppy obsługuje, w przeciwnym razie Part z Size jak prostokąt)
- W folderze MallLobby/NeonStrips

Pasy 1-3 (różowe, RGB 255, 87, 200):
1. "NeonPink_North" — Position (0, 13.5, -18), Size (75, 0.4, 0.8)
2. "NeonPink_Center" — Position (0, 13.5, 0), Size (75, 0.4, 0.8)
3. "NeonPink_South" — Position (0, 13.5, 18), Size (75, 0.4, 0.8)

Pasy 4-6 (cyjanowe, RGB 87, 220, 255):
4. "NeonCyan_NorthMid" — Position (0, 13.5, -9), Size (75, 0.4, 0.8)
5. "NeonCyan_SouthMid" — Position (0, 13.5, 9), Size (75, 0.4, 0.8)
6. "NeonCyan_Edge" — Position (0, 13.5, 25), Size (75, 0.4, 0.8)

Pod każdym pasem dodaj PointLight (jako dziecko Part):
- Range: 12
- Brightness: 2
- Color: identyczny jak Color Part'a (różowy lub cyjan)
- Shadows: true

KROK 3 — Fontanna w centrum lobby
Fontanna składa się z trzech części:

3a. "FountainBase" (basen kafelkowy):
- Position: Vector3.new(0, 1, 0)
- Size: Vector3.new(16, 1, 16)
- Material: Enum.Material.Marble
- Color: Color3.fromRGB(120, 200, 210)
- Anchored: true
- Parent: MallLobby/Fountain

3b. "FountainRing" (krawędź wewnętrzna, lekko podniesiona):
- Position: Vector3.new(0, 1.6, 0)
- Size: Vector3.new(14, 0.4, 14)
- Material: Enum.Material.Marble
- Color: Color3.fromRGB(45, 35, 50)
- Anchored: true
- Parent: MallLobby/Fountain

3c. "FountainCore" (świecący środek — neon różowy):
- Position: Vector3.new(0, 2, 0)
- Size: Vector3.new(4, 1.5, 4)
- Material: Enum.Material.Neon
- Color: Color3.fromRGB(255, 87, 200)
- Anchored: true
- Parent: MallLobby/Fountain

3d. PointLight w FountainCore:
- Range: 20
- Brightness: 3
- Color: Color3.fromRGB(255, 87, 200)
- Shadows: true

KROK 4 — Info Desk (przy zachodniej ścianie)
Info desk składa się z trzech części:

4a. "InfoDeskBase" (front lady):
- Position: Vector3.new(-32, 2, 0)
- Size: Vector3.new(2, 4, 12)
- Material: Enum.Material.Wood
- Color: Color3.fromRGB(155, 110, 75)
- Anchored: true
- Parent: MallLobby/InfoDesk

4b. "InfoDeskTop" (blat):
- Position: Vector3.new(-31, 4.2, 0)
- Size: Vector3.new(4, 0.4, 12)
- Material: Enum.Material.Wood
- Color: Color3.fromRGB(155, 110, 75)
- Anchored: true
- Parent: MallLobby/InfoDesk

4c. "InfoDeskBack" (ścianka tylna z logo):
- Position: Vector3.new(-30, 5, 0)
- Size: Vector3.new(2, 6, 12)
- Material: Enum.Material.SmoothPlastic
- Color: Color3.fromRGB(45, 35, 50)
- Anchored: true
- Parent: MallLobby/InfoDesk

4d. "InfoDeskNeonSign" (neonowy napis "INFO" jako pasek):
- Position: Vector3.new(-29, 6.5, 0)
- Size: Vector3.new(0.4, 1.5, 6)
- Material: Enum.Material.Neon
- Color: Color3.fromRGB(87, 220, 255)
- Anchored: true
- Parent: MallLobby/InfoDesk

KROK 5 — Mall Directory (przy wschodniej ścianie)
Tablica informacyjna mall'u — duża pionowa płyta z neonowym obramowaniem.

5a. "DirectoryFrame" (rama):
- Position: Vector3.new(38, 7, 0)
- Size: Vector3.new(0.4, 8, 12)
- Material: Enum.Material.Metal
- Color: Color3.fromRGB(45, 35, 50)
- Reflectance: 0.2
- Anchored: true
- Parent: MallLobby/MallDirectory

5b. "DirectoryScreen" (ekran):
- Position: Vector3.new(37.7, 7, 0)
- Size: Vector3.new(0.2, 6, 10)
- Material: Enum.Material.Neon
- Color: Color3.fromRGB(87, 220, 255)
- Anchored: true
- Parent: MallLobby/MallDirectory
- Transparency: 0.3

5c. "DirectoryBorderTop" (neonowa ramka góra):
- Position: Vector3.new(37.5, 11.2, 0)
- Size: Vector3.new(0.4, 0.4, 12.2)
- Material: Enum.Material.Neon
- Color: Color3.fromRGB(255, 87, 200)
- Anchored: true
- Parent: MallLobby/MallDirectory

5d. "DirectoryBorderBottom" (neonowa ramka dół):
- Position: Vector3.new(37.5, 2.8, 0)
- Size: Vector3.new(0.4, 0.4, 12.2)
- Material: Enum.Material.Neon
- Color: Color3.fromRGB(255, 87, 200)
- Anchored: true
- Parent: MallLobby/MallDirectory

5e. PointLight w DirectoryScreen:
- Range: 15
- Brightness: 2.5
- Color: Color3.fromRGB(87, 220, 255)
- Shadows: true

KROK 6 — Raport końcowy
Wypisz:
- Wszystkie utworzone obiekty (ścieżka + nazwa)
- Liczba PointLight'ów dodanych
- Czy któryś krok się nie udał
- Krótka ocena: czy lobby teraz wygląda jak liminal mall (porównanie do KROKU 12 z Promptu 1)

WAŻNE:
- Wszystkie obiekty Anchored: true
- Batch creation gdzie możliwe
- Jeśli Cylinder shape nie działa, użyj zwykłego Part z proporcjonalnym Size
- NIE dodawaj nic poza listą (np. dodatkowych ławek, ścieżek, tekstur)
- Jeśli zauważysz konflikt z istniejącymi obiektami z Promptu 1 — raportuj
```

---

## Po wykonaniu — twoja weryfikacja

1. **Wejdź do Studio i Play (F5)**
2. **Co powinieneś zobaczyć:**
   - Sufit ma 6 świecących pasów (3 różowe, 3 cyjanowe)
   - W centrum fontanna z różowym świecącym środkiem
   - Po lewej (zachód) info desk drewniany z cyjanowym neonem "INFO"
   - Po prawej (wschód) tablica informacyjna z cyjanowym ekranem i różową ramką
   - Wnętrze ma teraz prawdziwy "vibe" — nie jest pustą kostką
3. **Test atmosfery** — gdy stoisz pośrodku, kolorowe światła powinny mieszać się na ścianach

**Jeśli coś nie pasuje wizualnie:** screenshot + opis. Korekta wartości to 1 prompt.

**Jeśli OK:** commit i przechodzimy do Promptu 3 (interakcja z drzwiami).

## Commit message po sukcesie

```
PoC step 2: Liminal mall details (neons, fountain, info desk, directory)

- 6 neon strips on ceiling (3 pink, 3 cyan) with PointLights
- Central fountain with neon core
- Wooden info desk with cyan neon sign
- Mall directory screen with pink neon frame
```
