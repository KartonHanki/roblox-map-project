# PoC Liminal Mall — instrukcja workflow

**Cel paczki:** zwalidować w 3 krokach że pipeline Claude Code + Weppy + Roblox Studio działa do budowy mapy escape-room.

---

## Pliki w tej paczce

| Plik | Co zawiera | Kiedy użyć |
|---|---|---|
| `00-Liminal-Mall-Style-Guide.md` | Paleta, materiały, oświetlenie | Referencja — czytaj przed promptami |
| `01-Prompt-Setup-Sceny.md` | Lobby box + lighting | Pierwszy prompt do Claude Code |
| `02-Prompt-Detale-Liminal.md` | Neony + fontanna + meble | Drugi prompt (po sukcesie 01) |
| `03-Prompt-Interakcja-Drzwi.md` | Drzwi + ProximityPrompt + skrypt | Trzeci prompt (po sukcesie 02) |
| `README.md` (ten plik) | Workflow | Czytaj teraz |

---

## Workflow — jak używać

### Przed startem

**1. Roblox Studio otwarte z `MallPoC.rbxl`**
- Plik powinien być w `roblox-map-project/places/MallPoC.rbxl`
- Weppy plugin aktywny (kontrolka połączenia w panelu Weppy)

**2. VS Code otwarte z folderem `roblox-map-project`**
- Claude Code panel widoczny
- `/mcp` w Claude Code potwierdza że weppy jest connected

**3. Backup placeholder przed każdym promptem**
- W Studio: File → Save As → `places/backup/MallPoC_before_step_X.rbxl`
- Daje rollback gdyby coś poszło nie tak

### Wykonywanie kolejnych promptów

**Krok A:** Otwórz plik `01-Prompt-Setup-Sceny.md` w VS Code

**Krok B:** Skopiuj **całą sekcję między ``` (kod blok)** — to jest prompt dla Claude Code

**Krok C:** Wklej do Claude Code (pole "ctrl esc to focus or unfocus Claude")

**Krok D:** Czekaj — Claude Code raportuje krok po kroku

**Krok E:** Po zakończeniu sprawdź Studio:
- Czy obiekty się pojawiły
- Czy wymiary i kolory zgadzają się
- Czy Play mode (F5) działa poprawnie

**Krok F:** Jeśli OK — commit do GitHub
**Krok G:** Jeśli źle — screenshot + opis tutaj, debugujemy

**Krok H:** Powtórz dla `02-Prompt-Detale-Liminal.md`, potem `03-Prompt-Interakcja-Drzwi.md`

---

## Co robić gdy coś nie działa

### Claude Code mówi że nie ma narzędzi Weppy
- W Claude Code wpisz `/mcp` — sprawdź status weppy
- Restart Claude Code (`/quit` → `claude`)
- Restart Studio (zamknij → otwórz `MallPoC.rbxl`)

### Obiekt nie powstał / niepoprawne wymiary
- Zrób screenshot Explorer w Studio (drzewo obiektów)
- Wklej tutaj output Claude Code
- Naprawiamy konkretny krok

### Studio zawiesza się przy Play
- Zamknij Studio bez zapisywania
- Otwórz ostatni backup
- Przeanalizujemy który skrypt/obiekt powoduje problem

### Limit Claude Code się skończył
- Rozkład 3 prompty × ~20 minut = ~60 minut Claude Code
- Jeśli limit się wyczerpie w trakcie — nic nie tracisz, prompty są w plikach
- Po resecie kontynuujesz od miejsca w którym przerwało

---

## Po PoC — co dalej

### Jeśli wszystkie 3 prompty zadziałały:

**Sukces — pipeline działa.** Mamy do dyskusji:

1. **Jakość wizualna** — czy Liminal Mall z prymitywów wygląda atmosferycznie, czy generic?
2. **Czas na pokój** — ile zajęło zbudowanie 1 pokoju? Mnożymy × 5 pokojów + lobby = realny harmonogram MVP
3. **Czy potrzebujemy assetów** — może okaże się że bez Toolbox/BuiltByBit jakość jest niewystarczająca
4. **Następny pokój** — wybór z listy (księgarnia, sklep zoologiczny, food court, sklep z elektroniką, salon piękności)

### Jeśli któryś prompt się nie udał:

Mamy **konkretny problem do debugowania**, nie ogólne "AI nie radzi sobie". Wracamy tutaj, analizujemy, naprawiamy.

---

## Komunikacja w trakcie PoC

Po każdym prompcie wklej tutaj:
- ✅ lub ❌ czy działa
- Screenshot Studio (po Play mode i bez)
- Output Claude Code (skopiuj odpowiedź)
- Twoje obserwacje (np. "wygląda dziwnie", "kolory nie pasują", "działa, ale z opóźnieniem")

To pozwoli mi odpowiednio reagować — czy idziemy dalej czy poprawiamy.

---

## Commitowanie

Po **każdym udanym kroku** (1, 2, 3) — commit do GitHub. Gotowe message są w plikach promptów.

Generalna zasada: **commit działającego stanu, nigdy zepsutego**. Jeśli coś nie działa, debugujesz lokalnie do skutku, dopiero wtedy commit.

---

## Czas oczekiwany na cały PoC

- **Przygotowanie** (przeczytanie style guide, backup): 10 min
- **Prompt 1** (lobby): 15 min Claude Code + 5 min weryfikacja = 20 min
- **Prompt 2** (detale): 20 min Claude Code + 5 min weryfikacja = 25 min
- **Prompt 3** (drzwi): 20 min Claude Code + 10 min weryfikacja + debug = 30 min
- **Total: ~85 min** (jedna sesja)

Realne że może się rozłożyć na 2 dni jeśli będą problemy do naprawienia po drodze. To normalne.
