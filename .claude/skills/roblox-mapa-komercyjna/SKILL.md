---
name: roblox-mapa-komercyjna
description: Tworzenie i utrzymanie komercyjnych map Roblox w Roblox Studio z użyciem Weppy MCP. Use when the user mentions Roblox, Luau, Weppy, Rojo, ServerScriptService, ReplicatedStorage, mapy, terrain, GUI, RemoteEvent, DataStore, Studio, lub prosi o budowę/edycję obiektów Studio.
---

# Konwencje projektu
- Język: Luau, zawsze `--!strict`
- Kod i komentarze: PL; nazwy techniczne: EN
- MCP: Weppy (`npx -y @weppy/roblox-mcp`)
- Bezpieczeństwo: walidacja po stronie serwera dla każdego RemoteEvent
- Persystencja: DataStoreService z pcall + retry × 3
- GitHub: commituj po każdej sesji z opisem zmian
- Obsidian: zapisuj podsumowania sesji w /Roblox/AI-Notes