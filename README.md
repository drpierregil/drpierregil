# PhiGenix Local (offline)

Appli Electron **100% locale** avec LLM embarqué via **llama.cpp**, architecture **multi‑agents** (PhiMEDS, PhiADVICES, PhiCROSS_SELL, PhiCHIPS) et orchestrateur **PhiBRAIN**.

## Arborescence
```
phi_local/
├─ package.json
├─ electron-builder.yml
├─ src/
│  ├─ main/
│  │  ├─ main.js
│  │  ├─ orchestrator.js
│  │  ├─ schema.js
│  │  ├─ agents/
│  │  │  ├─ phiMEDS.prompt.txt
│  │  │  ├─ phiADVICES.prompt.txt
│  │  │  ├─ phiCROSS_SELL.prompt.txt
│  │  │  └─ phiCHIPS.prompt.txt
│  │  └─ llm/
│  │     ├─ llamaServer.js
│  │     └─ llmClient.js
│  ├─ preload.js
│  └─ renderer/
│     ├─ index.html
│     └─ renderer.js
├─ data/
│  └─ ocr_sample.json
├─ bin/
│  ├─ darwin/llama_server        (à déposer)
│  └─ win32/llama_server.exe     (à déposer)
└─ models/
   └─ model.gguf                 (à déposer)
```

## Prérequis
- Node.js 18+ (ou 20+)
- Binaire `llama_server` local (Metal conseillé sur Apple Silicon)
- Un modèle **GGUF** local (ex: TinyLlama 1.1B Chat Q4_K_M ou Mistral‑7B‑Instruct Q4_K_M)

## Démarrage
```bash
npm install
# Déposez `bin/darwin/llama_server` (macOS) ou `bin/win32/llama_server.exe` (Windows)
# Déposez `models/model.gguf`
npm run dev
```
Une fenêtre Electron s’ouvre et remplit l’UI avec la génération locale sur `data/ocr_sample.json`.

## Build offline
```bash
npm run build:mac   # macOS (DMG)
# Sur Windows:
npm run build:win   # NSIS
```
> Les dossiers `bin/`, `models/` et `data/` sont inclus comme **extraResources** (hors ASAR).

## Modifier les sous‑agents
Les prompts sont dans `src/main/agents/*.prompt.txt`. Modifiez à chaud puis relancez l’app.

## Remplacer le modèle
Remplacez `models/model.gguf` (même nom de fichier) pour changer de LLM sans rebuild.

## Notes
- L’orchestrateur valide le JSON avec **AJV** (anti sortie cassée).
- `PhiCROSS_SELL` est **offline** : pas d’appel à Perplexity ; le LLM propose 5 OTC logiques. 
- Par défaut, `llama_server` est lancé sur `127.0.0.1:8080` avec `--parallel 1` pour la robustesse.
