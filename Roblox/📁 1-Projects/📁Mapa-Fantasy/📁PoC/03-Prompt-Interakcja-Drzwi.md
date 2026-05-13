# PoC Prompt 3 v3 — Dwa drzwi (FIXED)

**Zmiany vs v2:**
- Bug fix: `markCompleted` i `hasCompleted` przeniesione PRZED `openMainDoors`
- ProximityPrompt Parent explicite w krokach 6 i 12
- Emoji 🔒 → `[!]`, Font: SourceSans
- Cleanup: usunięty zduplikowany komentarz z KROKU 9a

---

## Prompt do wklejenia w Claude Code

```
Buduj trzeci etap PoC — dwa zestawy drzwi z różną mechaniką. Wymaganie: Prompty 1 i 2 wykonane, lobby gotowe.

════════════════════════════════
CZĘŚĆ 1 — GŁÓWNE DRZWI (południe, Z=29.7)
════════════════════════════════

KROK 1 — Wytnij otwór w WallSouth
Obecny WallSouth: Position (0, 7.5, 30), Size (80, 14, 1). Usuń go.
Zastąp trzema częściami w MallLobby/Walls:

1a. "WallSouth_Left":
- Position: (-22.25, 7, 30), Size: (35.5, 14, 1)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

1b. "WallSouth_Right":
- Position: (22.25, 7, 30), Size: (35.5, 14, 1)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

1c. "WallSouth_Top":
- Position: (0, 12, 30), Size: (9, 4, 1)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

KROK 2 — Folder głównych drzwi
W MallLobby utwórz Model "MainDoors" z dzieckiem Folder "DoorParts".

KROK 3 — Dwie połówki głównych drzwi (szklane, 3x9)
3a. "MainDoorLeft":
- Position: (-1.75, 5, 29.7), Size: (3, 9, 0.5)
- Material: Glass, Color: RGB(200,230,240)
- Transparency: 0.3, Reflectance: 0.4, Anchored: true
- Parent: MallLobby/MainDoors/DoorParts

3b. "MainDoorRight":
- Position: (1.75, 5, 29.7), Size: (3, 9, 0.5)
- Material: Glass, Color: RGB(200,230,240)
- Transparency: 0.3, Reflectance: 0.4, Anchored: true
- Parent: MallLobby/MainDoors/DoorParts

KROK 4 — Futryna głównych drzwi
Wszystkie: Material: Metal, Color: RGB(170,175,180), Reflectance: 0.2, Anchored: true, Parent: MallLobby/MainDoors

4a. "MainFrameLeft": Position: (-3.5, 5, 29.7), Size: (0.5, 9, 0.6)
4b. "MainFrameRight": Position: (3.5, 5, 29.7), Size: (0.5, 9, 0.6)
4c. "MainFrameTop": Position: (0, 9.75, 29.7), Size: (7.5, 0.5, 0.6)

KROK 5 — Neon EXIT nad głównymi drzwiami (zielony)
5a. "ExitSignLeft":
- Position: (-1.5, 11, 29.5), Size: (2.5, 0.8, 0.3)
- Material: Neon, Color: RGB(0,220,80), Anchored: true
- Parent: MallLobby/MainDoors

5b. "ExitSignRight":
- Position: (1.5, 11, 29.5), Size: (2.5, 0.8, 0.3)
- Material: Neon, Color: RGB(0,220,80), Anchored: true
- Parent: MallLobby/MainDoors

5c. PointLight jako dziecko ExitSignLeft:
- Range: 8, Brightness: 2, Color: RGB(0,220,80), Shadows: false

KROK 6 — ProximityPrompt na MainDoorLeft
Stwórz ProximityPrompt jako BEZPOŚREDNIE DZIECKO MainDoorLeft:
- Parent: MallLobby/MainDoors/DoorParts/MainDoorLeft
- ActionText: "Wyjdź"
- ObjectText: "Wyjście"
- HoldDuration: 0
- KeyboardKeyCode: E
- MaxActivationDistance: 7
- RequiresLineOfSight: false

════════════════════════════════
CZĘŚĆ 2 — DRZWI ZAPLECZA (wschód, X=39.7)
════════════════════════════════

KROK 7 — Wytnij otwór w WallEast
Obecny WallEast: Position (40, 7.5, 0), Size (1, 14, 60). Usuń go.
Zastąp czterema częściami w MallLobby/Walls:

7a. "WallEast_Bottom":
- Position: (40, 3.25, 0), Size: (1, 5.5, 60)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

7b. "WallEast_Top":
- Position: (40, 11.75, 0), Size: (1, 4.5, 60)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

7c. "WallEast_Left":
- Position: (40, 7.5, -12.5), Size: (1, 5, 35)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

7d. "WallEast_Right":
- Position: (40, 7.5, 12.5), Size: (1, 5, 35)
- Material: Plaster, Color: RGB(230,220,200), Anchored: true

Otwór: X=40, Z od -5 do +5 (10 studs szeroki), Y od 1 do 10 (9 studs wysoki).

KROK 8 — Folder drzwi zaplecza
W MallLobby utwórz Model "BackDoors" z dzieckiem Folder "DoorParts".

KROK 9 — Drzwi zaplecza (metalowe, dwie połówki)
9a. "BackDoorLeft":
- Position: (39.7, 5, -2.25), Size: (0.5, 9, 4)
- Material: Metal, Color: RGB(60,55,65)
- Reflectance: 0.1, Anchored: true
- Parent: MallLobby/BackDoors/DoorParts

9b. "BackDoorRight":
- Position: (39.7, 5, 2.25), Size: (0.5, 9, 4)
- Material: Metal, Color: RGB(60,55,65)
- Reflectance: 0.1, Anchored: true
- Parent: MallLobby/BackDoors/DoorParts

KROK 10 — Futryna drzwi zaplecza
Wszystkie: Material: Metal, Color: RGB(45,40,50), Reflectance: 0.05, Anchored: true, Parent: MallLobby/BackDoors

10a. "BackFrameLeft": Position: (39.7, 5, -4.75), Size: (0.6, 9, 0.5)
10b. "BackFrameRight": Position: (39.7, 5, 4.75), Size: (0.6, 9, 0.5)
10c. "BackFrameTop": Position: (39.7, 9.75, 0), Size: (0.6, 0.5, 10)

KROK 11 — Czerwona dioda LED nad drzwiami zaplecza
11a. "BackLED":
- Position: (39.5, 10.5, 0), Size: (0.5, 0.5, 0.5)
- Material: Neon, Color: RGB(220,30,30), Anchored: true
- Parent: MallLobby/BackDoors

11b. PointLight jako dziecko BackLED:
- Range: 5, Brightness: 1.5, Color: RGB(220,30,30), Shadows: false

KROK 12 — ProximityPrompt na BackDoorLeft
Stwórz ProximityPrompt jako BEZPOŚREDNIE DZIECKO BackDoorLeft:
- Parent: MallLobby/BackDoors/DoorParts/BackDoorLeft
- ActionText: "Otwórz"
- ObjectText: "Drzwi zaplecza"
- HoldDuration: 0
- KeyboardKeyCode: E
- MaxActivationDistance: 7
- RequiresLineOfSight: false

════════════════════════════════
CZĘŚĆ 3 — GUI "ZAMKNIĘTE"
════════════════════════════════

KROK 13 — ScreenGui
W StarterGui utwórz ScreenGui "LockedDoorGui":
- ResetOnSpawn: true
- Enabled: false

W LockedDoorGui utwórz Frame "LockedFrame":
- Size: UDim2.new(0, 320, 0, 80)
- Position: UDim2.new(0.5, -160, 0.7, 0)
- BackgroundColor3: RGB(20,15,25)
- BackgroundTransparency: 0.2
- BorderSizePixel: 0

W LockedFrame utwórz TextLabel "LockedText":
- Size: UDim2.new(1, 0, 1, 0)
- Position: UDim2.new(0, 0, 0, 0)
- BackgroundTransparency: 1
- Text: "[!] Zamkniete — potrzebujesz klucza"
- TextColor3: RGB(220,30,30)
- TextScaled: true
- Font: SourceSans
- TextXAlignment: Center

W LockedFrame utwórz UICorner:
- CornerRadius: UDim.new(0, 8)

KROK 14 — RemoteEvent
W ReplicatedStorage utwórz RemoteEvent "ShowLockedDoor".

KROK 15 — LocalScript
W StarterPlayerScripts utwórz LocalScript "LockedDoorGuiController":

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local gui = playerGui:WaitForChild("LockedDoorGui")
local showLockedEvent = ReplicatedStorage:WaitForChild("ShowLockedDoor")

local function showLockedMessage()
    gui.Enabled = true
    task.wait(2)
    gui.Enabled = false
end

showLockedEvent.OnClientEvent:Connect(showLockedMessage)
print("[LockedDoorGui] gotowy")

════════════════════════════════
CZĘŚĆ 4 — DoorController (ServerScript)
════════════════════════════════

KROK 16 — Script "DoorController" w ServerScriptService
Wklej DOKŁADNIE ten kod (kolejność sekcji jest istotna dla Lua):

local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Referencje
local mallLobby = workspace:WaitForChild("MallLobby")
local mainDoors = mallLobby:WaitForChild("MainDoors")
local backDoors = mallLobby:WaitForChild("BackDoors")

local mainDoorL = mainDoors.DoorParts:WaitForChild("MainDoorLeft")
local mainDoorR = mainDoors.DoorParts:WaitForChild("MainDoorRight")
local mainPrompt = mainDoorL:FindFirstChildOfClass("ProximityPrompt")

local backDoorL = backDoors.DoorParts:WaitForChild("BackDoorLeft")
local backDoorR = backDoors.DoorParts:WaitForChild("BackDoorRight")
local backPrompt = backDoorL:FindFirstChildOfClass("ProximityPrompt")

local showLockedEvent = ReplicatedStorage:WaitForChild("ShowLockedDoor")

-- Pozycje zamknięte
local mainClosedL = mainDoorL.Position
local mainClosedR = mainDoorR.Position
local mainOpenL   = mainClosedL - Vector3.new(3.5, 0, 0)
local mainOpenR   = mainClosedR + Vector3.new(3.5, 0, 0)

-- TweenInfo
local openInfo  = TweenInfo.new(1.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local shakeInfo = TweenInfo.new(0.08, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, 3, true)

-- ════════════════════════════════
-- EASTER EGG HOOK (definicja PRZED openMainDoors — wymagane przez Lua)
-- ════════════════════════════════
local hasCompleted = false

local function markCompleted()
    if hasCompleted then return end
    hasCompleted = true
    print("[EASTER_EGG] Run ukończony. Flag ustawiony.")
    -- TODO faza 2: dodaj tutaj trigger easter egga
    -- Przykład: workspace.EasterEggTrigger.Enabled = true
end

-- ════════════════════════════════
-- GŁÓWNE DRZWI
-- ════════════════════════════════
local mainIsOpen      = false
local mainIsAnimating = false

local function openMainDoors()
    if mainIsAnimating or mainIsOpen then return end
    mainIsAnimating = true
    mainPrompt.Enabled = false

    local tL = TweenService:Create(mainDoorL, openInfo, {Position = mainOpenL})
    local tR = TweenService:Create(mainDoorR, openInfo, {Position = mainOpenR})
    tL:Play()
    tR:Play()
    tR.Completed:Wait()

    mainIsOpen = true
    mainIsAnimating = false

    markCompleted()  -- bezpieczne: zdefiniowane wyżej

    task.wait(5)

    mainIsAnimating = true
    local cL = TweenService:Create(mainDoorL, openInfo, {Position = mainClosedL})
    local cR = TweenService:Create(mainDoorR, openInfo, {Position = mainClosedR})
    cL:Play()
    cR:Play()
    cR.Completed:Wait()

    mainIsOpen      = false
    mainIsAnimating = false
    mainPrompt.Enabled = true
end

-- ════════════════════════════════
-- DRZWI ZAPLECZA — drżenie + GUI + dźwięk
-- ════════════════════════════════
local backIsShaking = false

local function shakeBackDoors(player)
    if backIsShaking then return end
    backIsShaking = true
    backPrompt.Enabled = false

    -- Dźwięk (chunk-chunk mechaniczny zamek)
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://9120386446"
    sound.Volume   = 0.6
    sound.RollOffMaxDistance = 20
    sound.Parent   = backDoorL
    sound:Play()
    game:GetService("Debris"):AddItem(sound, 3)

    -- Drżenie: 0.5 studs w osi X, powrót (3 razy)
    local origL = backDoorL.Position
    local origR = backDoorR.Position
    local offset = Vector3.new(0.5, 0, 0)

    local shakeL = TweenService:Create(backDoorL, shakeInfo, {Position = origL + offset})
    local shakeR = TweenService:Create(backDoorR, shakeInfo, {Position = origR - offset})
    shakeL:Play()
    shakeR:Play()
    shakeR.Completed:Wait()

    -- Resetuj pozycje (zabezpieczenie)
    backDoorL.Position = origL
    backDoorR.Position = origR

    -- GUI dla gracza który próbował
    showLockedEvent:FireClient(player)

    task.wait(0.5)
    backIsShaking      = false
    backPrompt.Enabled = true
end

-- ════════════════════════════════
-- POŁĄCZENIA
-- ════════════════════════════════
mainPrompt.Triggered:Connect(function(player)
    openMainDoors()
end)

backPrompt.Triggered:Connect(function(player)
    shakeBackDoors(player)
end)

print("[DoorController] gotowy | Główne: " .. mainDoorL.Name .. " | Zaplecze: " .. backDoorL.Name)

KROK 17 — Weryfikacja
Sprawdź że istnieją:
1. ServerScriptService/DoorController (Script)
2. StarterGui/LockedDoorGui (ScreenGui, Enabled: false)
3. StarterPlayerScripts/LockedDoorGuiController (LocalScript)
4. ReplicatedStorage/ShowLockedDoor (RemoteEvent)
5. ProximityPrompt w MainDoorLeft i BackDoorLeft

KROK 18 — Raport końcowy
Wypisz:
- Wszystkie stworzone/usunięte obiekty
- Czy któryś krok nie zadziałał
- Ewentualne ostrzeżenia

WAŻNE:
- DoorController = Script (nie LocalScript), w ServerScriptService
- LockedDoorGuiController = LocalScript, w StarterPlayerScripts
- Kolejność w skrypcie MUSI być zachowana (markCompleted przed openMainDoors)
- Wszystkie nowe obiekty Anchored: true
- Jeśli Sound ID nie zadziała, usuń Sound i zaraportuj
```

---

## Weryfikacja po wykonaniu

### Test 1 — główne drzwi (południe)
1. F5 → idź na południe
2. Prompt "Wyjdź / Wyjście" pojawia się przy drzwiach
3. E → drzwi rozsuwają się (1.2 sek)
4. Po 5 sek zamykają się same
5. Output: `[DoorController] gotowy | Główne: MainDoorLeft | Zaplecze: BackDoorLeft`
6. Output: `[EASTER_EGG] Run ukończony. Flag ustawiony.`

### Test 2 — drzwi zaplecza (wschód)
1. Idź na wschód
2. Prompt "Otwórz / Drzwi zaplecza"
3. E → drżenie (3× 0.08 sek), dźwięk chunk-chunk
4. GUI "[!] Zamkniete — potrzebujesz klucza" pojawia się na ekranie
5. GUI znika po 2 sek
6. Prompt wraca po 0.5 sek

### Jeśli błąd w Output:
- `attempt to call nil value 'markCompleted'` → sekcja Easter Egg nie jest przed openMainDoors, poprawiamy
- `ShowLockedDoor is not a valid member` → RemoteEvent nie istnieje w ReplicatedStorage, sprawdź KROK 14
- GUI się nie pokazuje → sprawdź czy LocalScript jest w StarterPlayerScripts (nie StarterGui)

## Commit message po sukcesie

```
PoC step 3: Double doors with interactions + easter egg hook

- Main doors (glass, south): open/close TweenService
- Back doors (metal, east): shake animation + locked GUI + sound
- Green EXIT neon + red LED diode
- ScreenGui LockedDoorGui + RemoteEvent system
- DoorController with markCompleted() easter egg hook
- WallSouth + WallEast split for door openings
```
