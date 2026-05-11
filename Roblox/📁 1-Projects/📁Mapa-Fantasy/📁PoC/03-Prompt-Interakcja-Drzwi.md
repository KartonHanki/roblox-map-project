# PoC Prompt 3 — Interakcja: drzwi + ProximityPrompt

**Cel:** Dodać działającą interakcję — gracz podchodzi do drzwi, pojawia się prompt "Press E", po naciśnięciu drzwi się otwierają (animacja przesunięcia w bok). To finalny test PoC: czy AI radzi sobie z **kodem serwerowym + obiektami interaktywnymi**.

**Czas oczekiwany:** 10-15 minut Claude Code

**Kryteria sukcesu:**
- Dwustronne drzwi (ServerScript dla logiki)
- ProximityPrompt z napisem "Otwórz drzwi" pojawia się gdy gracz blisko
- Po naciśnięciu E drzwi rozsuwają się płynnie (TweenService)
- Drzwi same się zamykają po 5 sekundach
- Brak błędów w konsoli Studio

**Wymaganie:** Prompt 1 i 2 muszą być wykonane.

---

## Prompt do wklejenia w Claude Code

```
Buduj trzeci etap PoC — drzwi z interakcją. To jest test pełnego pipeline: obiekty + skrypt serwerowy + ProximityPrompt + TweenService.

KROK 1 — Folder structure
W MallLobby utwórz Model o nazwie "ExitDoors". W tym Modelu utwórz dziecko Folder "DoorParts".

KROK 2 — Drzwi (dwie połówki)
Drzwi znajdują się w południowej ścianie (gracz wchodzi do mall'u przez te drzwi). Każda połówka:
- Material: Enum.Material.Glass
- Color: Color3.fromRGB(200, 230, 240)
- Transparency: 0.3
- Reflectance: 0.4
- Anchored: true (NA RAZIE — w skrypcie będziemy ruszać przez Tween)
- Parent: MallLobby/ExitDoors/DoorParts

2a. "DoorLeft":
- Position: Vector3.new(-2, 5, 29.5)
- Size: Vector3.new(4, 10, 0.5)

2b. "DoorRight":
- Position: Vector3.new(2, 5, 29.5)
- Size: Vector3.new(4, 10, 0.5)

KROK 3 — Futryna drzwi (ramka metalowa)
Trzy części futryny dookoła drzwi:

3a. "DoorFrameLeft":
- Position: Vector3.new(-4.25, 5, 29.5)
- Size: Vector3.new(0.5, 10, 0.6)
- Material: Enum.Material.Metal
- Color: Color3.fromRGB(170, 175, 180)
- Reflectance: 0.2
- Anchored: true
- Parent: MallLobby/ExitDoors

3b. "DoorFrameRight":
- Position: Vector3.new(4.25, 5, 29.5)
- Size: Vector3.new(0.5, 10, 0.6)
- Material: Enum.Material.Metal
- Color: Color3.fromRGB(170, 175, 180)
- Reflectance: 0.2
- Anchored: true
- Parent: MallLobby/ExitDoors

3c. "DoorFrameTop":
- Position: Vector3.new(0, 10.25, 29.5)
- Size: Vector3.new(9, 0.5, 0.6)
- Material: Enum.Material.Metal
- Color: Color3.fromRGB(170, 175, 180)
- Reflectance: 0.2
- Anchored: true
- Parent: MallLobby/ExitDoors

KROK 4 — Wytnij dziurę w ścianie południowej (WallSouth)
Obecna WallSouth ma rozmiar (80, 14, 1). Trzeba ją zastąpić TRZEMA mniejszymi częściami żeby było wejście drzwiowe pośrodku:

4a. Usuń stary "WallSouth" z MallLobby/Walls.

4b. Stwórz "WallSouth_Left":
- Position: Vector3.new(-22.25, 7, 30)
- Size: Vector3.new(35.5, 14, 1)
- Material: Enum.Material.Plaster
- Color: Color3.fromRGB(230, 220, 200)
- Anchored: true
- Parent: MallLobby/Walls

4c. Stwórz "WallSouth_Right":
- Position: Vector3.new(22.25, 7, 30)
- Size: Vector3.new(35.5, 14, 1)
- Material: Enum.Material.Plaster
- Color: Color3.fromRGB(230, 220, 200)
- Anchored: true
- Parent: MallLobby/Walls

4d. Stwórz "WallSouth_Top" (nad drzwiami):
- Position: Vector3.new(0, 12.5, 30)
- Size: Vector3.new(9, 3, 1)
- Material: Enum.Material.Plaster
- Color: Color3.fromRGB(230, 220, 200)
- Anchored: true
- Parent: MallLobby/Walls

KROK 5 — ProximityPrompt
Stwórz ProximityPrompt jako dziecko DoorLeft:
- ActionText: "Otwórz drzwi"
- ObjectText: "Wyjście z Mall"
- HoldDuration: 0
- KeyboardKeyCode: Enum.KeyCode.E
- MaxActivationDistance: 8
- RequiresLineOfSight: false
- Style: ProximityPromptStyle.Default

KROK 6 — Server Script
W ServerScriptService stwórz Script (NIE LocalScript) o nazwie "DoorController". Wklej do niego dokładnie ten kod:

local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")

local doors = Workspace:WaitForChild("MallLobby"):WaitForChild("ExitDoors")
local doorLeft = doors.DoorParts.DoorLeft
local doorRight = doors.DoorParts.DoorRight
local prompt = doorLeft:FindFirstChildOfClass("ProximityPrompt")

local closedPosLeft = doorLeft.Position
local closedPosRight = doorRight.Position
local openPosLeft = closedPosLeft - Vector3.new(4, 0, 0)
local openPosRight = closedPosRight + Vector3.new(4, 0, 0)

local isOpen = false
local isAnimating = false

local tweenInfo = TweenInfo.new(
	1.2,
	Enum.EasingStyle.Quad,
	Enum.EasingDirection.Out
)

local function openDoors()
	if isAnimating or isOpen then return end
	isAnimating = true
	prompt.Enabled = false
	
	local tweenL = TweenService:Create(doorLeft, tweenInfo, {Position = openPosLeft})
	local tweenR = TweenService:Create(doorRight, tweenInfo, {Position = openPosRight})
	tweenL:Play()
	tweenR:Play()
	tweenR.Completed:Wait()
	
	isOpen = true
	isAnimating = false
	
	task.wait(5)
	
	isAnimating = true
	local closeL = TweenService:Create(doorLeft, tweenInfo, {Position = closedPosLeft})
	local closeR = TweenService:Create(doorRight, tweenInfo, {Position = closedPosRight})
	closeL:Play()
	closeR:Play()
	closeR.Completed:Wait()
	
	isOpen = false
	isAnimating = false
	prompt.Enabled = true
end

prompt.Triggered:Connect(openDoors)

print("[DoorController] gotowy. Drzwi: " .. doorLeft.Name .. ", " .. doorRight.Name)

KROK 7 — Test logiki
Po stworzeniu skryptu:
- Sprawdź że ServerScriptService zawiera Script "DoorController"
- Sprawdź że ProximityPrompt jest dzieckiem DoorLeft
- Sprawdź że oba DoorLeft i DoorRight są Anchored

KROK 8 — Raport końcowy
Wypisz:
- Wszystkie utworzone/zmienione obiekty
- Czy WallSouth został poprawnie zastąpiony 3 częściami
- Czy DoorController jest w ServerScriptService
- Stan ProximityPrompt (właściwości)
- Czy któryś krok się nie udał

WAŻNE:
- DoorController musi być Script, NIE LocalScript
- DoorController musi być w ServerScriptService, NIE w Workspace
- ProximityPrompt jest dzieckiem DoorLeft (nie obu drzwi)
- Drzwi muszą być Anchored, mimo że Tween będzie nimi ruszać (Tween działa na Position bez fizyki)
```

---

## Po wykonaniu — twoja weryfikacja (krytyczna)

1. **Wejdź do Studio, kliknij Play (F5)**
2. **Idź do drzwi** (na południe od fontanny)
3. **Powinno wyskoczyć:** "Press E — Otwórz drzwi" gdy jesteś bliżej niż 8 studs
4. **Naciśnij E** — drzwi rozsuwają się w boki
5. **Czekaj 5 sekund** — drzwi same się zamykają
6. **Output (ekran F9)** — powinieneś zobaczyć log: `[DoorController] gotowy. Drzwi: DoorLeft, DoorRight`

**Jeśli ProximityPrompt się nie pokazuje:**
- Sprawdź czy jest dzieckiem DoorLeft (Explorer → MallLobby → ExitDoors → DoorParts → DoorLeft)
- Sprawdź MaxActivationDistance (8)
- Sprawdź czy gracz jest bliżej niż 8 studs

**Jeśli drzwi się nie ruszają po E:**
- Otwórz Output (View → Output lub F9)
- Sprawdź czy są errory
- Skopiuj treść erroru i wklej tutaj

**Jeśli wszystko działa:** **PoC ZAKOŃCZONY** — masz potwierdzenie że pipeline Claude Code → Weppy → Studio działa dla obiektów + interakcji + skryptów. Gratulacje.

## Commit message po sukcesie

```
PoC step 3: Interactive doors with ProximityPrompt

- Glass double doors with metal frame (south wall)
- WallSouth split into 3 pieces (left, right, top above doors)
- ProximityPrompt with "Press E" to open
- ServerScript DoorController using TweenService
- Auto-close after 5 seconds
```

W terminalu:
```cmd
git add .
git commit -m "PoC step 3: Interactive doors with ProximityPrompt"
git push
```

## Po zakończeniu PoC

**Następny krok zależy od wyniku:**

### Jeśli PoC działa (najczęstszy scenariusz):
1. Zrób screenshot Studio + screenshot wnętrza w Play mode
2. Wklej tutaj
3. Decydujemy co dalej:
   - Pełen MVP (5 pokojów + lobby + boss puzzle)
   - Sprzedaż samego Liminal Mall jako asset/template
   - Inny pivot

### Jeśli PoC nie działa wizualnie:
- Liminal Mall bez assetów wygląda generic = trzeba kupić assety lub zmienić setting
- Dyskutujemy alternatywę

### Jeśli PoC nie działa technicznie:
- Debug konkretnego kroku
- Może trzeba zmienić MCP server (ale Weppy zadziałał na cube'ie, więc raczej OK)

**Cierpliwości — to jest realny moment prawdy. Wynik tych 3 promptów zdecyduje o reszcie projektu.**
