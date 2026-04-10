# Veille Tech — Log

Log de veille alimenté automatiquement via la commande `/veille <url>`.
Entrées triées de la plus récente à la plus ancienne.

---

## 2026-04-10 — claude-subconscious — Agent background mémoire persistante pour Claude Code

**URL** : https://github.com/letta-ai/claude-subconscious
**Type** : GitHub repo
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `claude-code` `memory` `letta` `background-agent`

### Objectif
Agent background (Letta SDK) qui surveille les sessions Claude Code, explore le codebase, maintient 8 blocs de mémoire persistants cross-sessions, et injecte des guidances via stdout sans modifier CLAUDE.md. Fonctionne via les hooks SessionStart / UserPromptSubmit / PreToolUse / Stop.

### Ce qu'il y a à garder
- **Hook UserPromptSubmit pour injecter du contexte** : pattern direct pour enrichir chaque prompt avec la mémoire accumulée — applicable au pipeline cinehome pour que chaque agent ait le contexte du THREAD en cours
- **8 blocs mémoire** : core directives, active guidance, user preferences, project context, session patterns, pending work, self-improvement, tool usage — structure réutilisable pour notre système memory/
- **Mode `whisper` (stdout uniquement)** : inject context sans modifier les fichiers — pattern propre qui évite les conflits de file ownership entre agents
- **Async transcript processing** au Stop hook : analyser ce qui s'est passé APRÈS la session pour enrichir la mémoire — pattern qu'on a pas encore dans notre pipeline

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : letta-ai (équipe Letta, ex-MemGPT), actif
- **Dépendances** : Letta Code SDK, `LETTA_API_KEY` requis
- **Data** : Les transcripts de sessions sont envoyés à app.letta.com pour le traitement mémoire — données de code potentiellement sensibles transitent par Letta cloud
- **Verdict** : 🟡 Vigilance — concept très fort mais dépendance cloud Letta obligatoire, pas self-hostable facilement. Ne pas utiliser sur des projets avec code propriétaire sensible

### Applicabilité projets

**CinéHome** : Tarpin pertinent. Le pattern des 8 blocs mémoire et l'injection via UserPromptSubmit pourrait enrichir notre système memory/ actuel. En particulier le "session patterns" et "pending work" — exactement ce qu'on fait manuellement dans forum.md. À étudier pour remplacer/compléter le polling forum.md par un système mémoire plus structuré.

**Equizio** : Moyen — utile pour garder le contexte sur les gaps connus (tests, monitoring, SEO) entre sessions de dev, éviter de re-expliquer le contexte à chaque session. Mais la dépendance Letta cloud est un frein pour un projet en prod.

---

## 2026-04-10 — Pixel-Perfect — Librairie composants React copy-paste

**URL** : https://github.com/vansh-nagar/Pixel-Perfect
**Type** : GitHub repo / librairie UI
**Score** : ⭐⭐⭐☆☆ (3/5)
**Tags** : `react` `composants` `ui` `copy-paste`

### Objectif
Librairie de composants React TypeScript précis et production-ready, modèle copy-paste à la shadcn/ui. Registry avec variant "New York", Next.js-based, Tailwind CSS intégré. Tagline : "Build beautiful, responsive interfaces in minutes. Copy - Paste - Done."

### Ce qu'il y a à garder
- **Modèle registry copy-paste** : pattern identique à shadcn/ui mais avec des composants potentiellement plus "pixel-perfect" sur les détails visuels
- **Variant "New York"** : style épuré, proche de ce qu'on utilise sur Equizio
- **347 stars** : petit mais actif, communauté qui grandit
- Aller voir `www.pixel-perfect.space/` pour voir les composants en action avant d'en piquer

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : vansh-nagar (solo), 12 forks
- **Dépendances** : Next.js, TypeScript, Tailwind CSS, PostCSS — stack standard, rien de suspect
- **Data** : Composants statiques, zéro cloud, zéro tracking
- **Verdict** : 🟢 Safe — composants copy-paste locaux, stack classique, MIT

### Applicabilité projets

**CinéHome** : Pas applicable — pas une app grand public, pas de composants UI needed.

**Equizio** : Utile pour les 96 illustrations à créer et les composants manquants (admin panel, notifications UI, A11Y). Avant de coder from scratch, checker si Pixel-Perfect a pas déjà un composant qui correspond. Stack identique (Next.js + Tailwind + TypeScript).

---

## 2026-04-10 — Manim Video Skill — Pipeline animations mathématiques 3Blue1Brown

**URL** : https://github.com/NousResearch/hermes-agent/tree/main/skills/creative/manim-video
**Type** : GitHub repo / skill agent
**Score** : ⭐⭐☆☆☆ (2/5)
**Tags** : `manim` `animation` `python` `video-generation`

### Objectif
Skill pour l'agent Hermes (NousResearch) qui automatise la création de vidéos d'animation mathématiques style 3Blue1Brown. Pipeline complet : planning → génération code Manim → rendu → stitching scènes → raffinement itératif. Python 3.10+, Manim CE, LaTeX, ffmpeg.

### Ce qu'il y a à garder
- **Pattern skill Hermes** : structure du skill (planning → code gen → render → stitch) réutilisable pour créer des skills d'agent similaires dans notre pipeline
- **Raffinement itératif automatisé** : l'agent s'auto-corrige les erreurs de rendu Manim — pattern "test → fix → re-render" applicable au QA agent
- Repo hermes-agent a **50k stars** et des dizaines d'autres skills créatifs à explorer

### Sécurité
- **Licence** : MIT (hermes-agent)
- **Mainteneurs** : NousResearch (équipe sérieuse, recherche LLM open-source)
- **Dépendances** : Manim CE + LaTeX + ffmpeg — stack très lourde (>2GB), pas légère
- **Data** : Local uniquement, rendu en local
- **Verdict** : 🟢 Safe — tout local, dépendances connues, mais installation lourde

### Applicabilité projets

**CinéHome** : Pas applicable directement. Mais la structure du skill Hermes (planning → exec → validate → iterate) est un beau pattern à s'inspirer pour enrichir nos agents cinehome, en particulier le QA-tester.

**Equizio** : Zéro applicabilité directe. Si un jour on veut des vidéos explicatives pour les Galops, intéressant, mais c'est du very nice-to-have.

---

## 2026-04-10 — ai-workflow-kit — Skills, agents et hooks pour Claude Code/Cursor/Copilot

**URL** : https://github.com/bezael/ai-workflow-kit
**Type** : GitHub repo / toolkit
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `claude-code` `hooks` `skills` `workflow`

### Objectif
Toolkit JS/Shell qui standardise les workflows AI-assisted dev avec skills (`/ak:commit`, `/ak:review`, `/ak:plan`...), agents spécialisés (frontend, api, test, refactor, docs), et 5 hooks automatiques (pre-bash-safety, pre-commit-secrets, post-write-format, post-edit-lint, notify-done). Compatible Claude Code, Cursor, GitHub Copilot.

### Ce qu'il y a à garder
- **Pattern `/ak:` prefix pour les skills** : évite les collisions avec les skills natifs Claude Code — à adopter pour nos skills custom
- **pre-commit-secrets hook** : bloque les API keys avant les commits — déjà inspiré pour nos hooks globaux `~/.claude/hooks/`
- **Agents spécialisés avec `/ak:frontend`** : génère des composants UI en suivant le design system du projet — pattern direct pour enrichir notre senior-engineer agent
- **notify-done hook** : notif desktop quand l'IA finit — redondant avec vibe-island mais pattern réutilisable
- **`npx ai-workflow-kit`** : installation one-liner, modèle d'installation à s'inspirer pour distribuer notre pipeline

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : bezael (1 contributeur, 32 stars, 17 commits) — bus factor = 1
- **Dépendances** : Node.js, Prettier/Biome, ESLint, Git — stack standard
- **Data** : Scripts locaux uniquement, zéro cloud
- **Verdict** : 🟢 Safe — scripts locaux, MIT, stack classique. Bus factor 1 = attention si le repo disparaît

### Applicabilité projets

**CinéHome** : On a déjà piqué les patterns hooks (pre-commit-secrets, post-write-format, post-edit-lint installés en session 3). Le skill `/ak:frontend` et `/ak:test` sont des patterns à intégrer dans nos agents senior-engineer et qa-tester. Le prefix `/ak:` est une bonne pratique à adopter pour nos skills custom.

**Equizio** : Le hook pre-commit-secrets est directement utile (clés Stripe/Supabase à protéger). Le skill `/ak:review` pour les PRs equizio.

---

## 2026-04-10 — code-review-graph — MCP knowledge graph AST local (-6.8x tokens)

**URL** : https://github.com/tirth8205/code-review-graph
**Type** : GitHub repo / MCP server
**Score** : ⭐⭐⭐⭐⭐ (5/5)
**Tags** : `mcp` `code-review` `knowledge-graph` `token-optimization`

### Objectif
Système de graphe de connaissance local (Tree-sitter + SQLite) qui mappe un codebase en AST persistant. Le MCP expose 22 outils à Claude Code pour requêter uniquement les fichiers pertinents à un changement. Résultat : -6.8x tokens en moyenne, -49x sur les monorepos. 19+ langages supportés.

### Ce qu'il y a à garder
- **`code-review-graph build`** : une seule commande pour indexer tout le repo → graphe persistant SQLite, incrémental (<2s de re-parse sur modif)
- **Blast-radius analysis** : donne l'impact exact d'un changement (fonctions, classes, fichiers affectés) — exactly ce que le PR-REVIEWER agent a besoin avant de review
- **22 outils MCP** : semantic search, dependency graph, architecture viz D3.js, watch mode — arsenal complet pour les agents
- **-49x tokens sur monorepos** : cinehome est un monorepo Next.js + Supabase, impact potentiel énorme
- **Watch mode** : mise à jour auto du graphe sur chaque edit git — pas besoin de rebuild manuel

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : tirth8205, 7.6k stars, actif
- **Dépendances** : Tree-sitter, SQLite, Python 3.10+ — stack locale uniquement
- **Data** : Tout en local (SQLite), zéro cloud, zéro tracking
- **Verdict** : 🟢 Safe — local-only, open-source, MIT, pas d'accès réseau

### Applicabilité projets

**CinéHome** : ✅ DÉJÀ INSTALLÉ sur cinehome (session 4, 2026-04-06). Graphe buildé : 42 004 nœuds, 196 363 edges sur 2012 fichiers. À activer en relançant Claude Code dans cinehome pour que les agents (pr-reviewer, bug-finder) utilisent le graphe automatiquement.

**Equizio** : À installer sur equizio aussi — même bénéfice tokens sur un projet Next.js + Supabase. `pip install code-review-graph && code-review-graph build` dans le dossier equizio.

---

## 2026-04-10 — tarkan-image-generator — Générateur images Imagen/Gemini (Next.js + Convex)

**URL** : https://github.com/onuro/tarkan-image-generator
**Type** : GitHub repo / app full-stack
**Score** : ⭐⭐⭐☆☆ (3/5)
**Tags** : `image-generation` `nextjs` `convex` `google-ai`

### Objectif
App full-stack TypeScript de génération d'images via Google Imagen 4 / Gemini. Support multi-modèles, 14 images de référence par génération, 11 style presets, prompt expansion auto, tracking historique. Stack : Next.js 16 + React 19 + Tailwind v4 + Convex (serverless + realtime DB) + Google Generative AI SDK.

### Ce qu'il y a à garder
- **Convex comme backend serverless + realtime DB** : alternative à Supabase pour les features realtime (status tracking, historique). Pattern intéressant pour des features "live" sur Equizio
- **Prompt expansion automatique via AI** : améliore les prompts utilisateur avant génération — pattern réutilisable pour n'importe quelle feature de génération de contenu
- **Style presets (11 presets)** : UI pattern propre pour guider les utilisateurs dans leurs choix
- **Stack Next.js 16 + React 19** : version très récente à surveiller pour la migration Equizio

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : onuro (solo), 16 stars — très petit projet
- **Dépendances** : Convex (cloud, terms of service à lire), Google AI SDK (données envoyées à Google)
- **Data** : Prompts et images de référence transitent par Google Cloud via Generative AI SDK
- **Verdict** : 🟡 Vigilance — Google AI cloud obligatoire, données de génération chez Google. Convex = autre dépendance cloud. Pas d'enjeu pour un usage perso/proto, attention en prod

### Applicabilité projets

**CinéHome** : Pas applicable directement. Mais le pattern Convex realtime + status tracking pourrait servir pour un hub de monitoring d'agents plus robuste que le polling forum.md actuel.

**Equizio** : Pertinent pour les 96 illustrations de quiz à créer. Pas pour intégrer dans l'app (hors scope), mais pour générer les illustrations en batch via un workflow similaire. Le style preset "watercolor" ou "illustration" pourrait convenir au style Equizio.

---

## 2026-04-10 — SuperCmd — Launcher macOS open-source (Raycast + AI + Voice)

**URL** : https://github.com/SuperCmdLabs/SuperCmd
**Type** : GitHub repo / outil desktop
**Score** : ⭐⭐☆☆☆ (2/5)
**Tags** : `macos` `launcher` `raycast` `electron`

### Objectif
Launcher macOS open-source Electron + TypeScript qui combine Raycast + dictée vocale + TTS + AI (OpenAI, Anthropic, Ollama) + mémoire. Compatible extensions Raycast existantes. ~1900 stars.

### Ce qu'il y a à garder
- **Compatibilité runtime Raycast** : peut exécuter les extensions Raycast existantes — intéressant si on a déjà des extensions Raycast custom
- **AI provider pluggable** : Anthropic, OpenAI, Ollama — pattern de config multi-LLM réutilisable
- **Voice input hold-to-speak** : pattern UX interessant pour un outil de dev

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : SuperCmdLabs, ~1900 stars, TypeScript + Swift
- **Dépendances** : Electron 28 — grosse dépendance, surface d'attaque large. Xcode CLI tools requis
- **Data** : Selon le provider AI configuré (OpenAI/Anthropic/Ollama), les requêtes transitent par leur cloud respectif
- **Verdict** : 🟡 Vigilance — Electron + accès système complet (hotkeys, clipboard, accessibility). Légitime mais surface d'attaque large

### Applicabilité projets

**CinéHome** : Pas applicable — outil perso macOS, pas relié au pipeline.

**Equizio** : Pas applicable.

---

## 2026-04-10 — awesome-design-md — 62+ DESIGN.md files pour marques connues (40k stars)

**URL** : https://github.com/VoltAgent/awesome-design-md
**Type** : GitHub repo / collection
**Score** : ⭐⭐⭐⭐⭐ (5/5)
**Tags** : `design-system` `design-md` `vibe-coding` `agents`

### Objectif
Collection de 62+ fichiers DESIGN.md extraits des sites de marques connues (Stripe, Vercel, Linear, Supabase, Notion, Claude, Nike...). Chaque fichier contient couleurs, typographie, composants, layout, responsive — format prêt pour les agents de code pour générer de l'UI pixel-perfect en accord avec un design system.

### Ce qu'il y a à garder
- **Drop `DESIGN.md` dans le projet → les agents génèrent de l'UI cohérente** : mécanisme ultra simple, pas de tooling, juste un fichier MD que Claude Code lit
- **DESIGN.md Claude existe** : permet de générer de l'UI dans le style exact de Claude.ai — utile pour l'agent-hub ou tout outil interne
- **DESIGN.md Stripe + Supabase** : directement applicables à Equizio (Stripe intégré, Supabase backend)
- **DESIGN.md Linear + Vercel** : applicables pour des dashboards style admin panel
- **Format standardisé** : color palettes + typography + components + elevation + responsive + agent-ready prompts — format à adopter pour nos propres projets
- **40k stars** — c'est une ressource de référence absolue pour le vibe coding

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : VoltAgent (équipe), community contributions, très actif
- **Dépendances** : Zéro — fichiers Markdown statiques
- **Data** : Zéro — fichiers locaux, rien envoyé nulle part
- **Verdict** : 🟢 Safe — fichiers markdown statiques, zéro risque

### Applicabilité projets

**CinéHome** : Ajouter `DESIGN.md` Linear ou Vercel dans cinehome pour que les agents (senior-engineer, pr-reviewer) génèrent de l'UI cohérente. Le DESIGN.md Claude peut servir pour l'agent-hub.html.

**Equizio** : Ajouter `DESIGN.md` Stripe + Supabase dans equizio — les agents généreront des composants (admin panel, forms, notifications) cohérents avec les outils déjà utilisés. Game changer pour les 96 illustrations et l'admin panel manquant.

---

## 2026-04-10 — Bridge DS — Compilateur design Figma depuis Claude Code (CSpec + recipes)

**URL** : https://github.com/noemuch/bridge
**Type** : GitHub repo / outil Claude Code
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `figma` `claude-code` `design-system` `compiler`

### Objectif
Compilateur qui transforme Claude Code en designer Figma intelligent. L'utilisateur décrit un design → Claude génère une CSpec (YAML) → le compilateur produit du Plugin API code → exécuté dans Figma via WebSocket/MCP. Produit des designs conformes au design system sans scripting manuel. 112 stars, MIT.

### Ce qu'il y a à garder
- **CSpec (YAML compilable)** : format déclaratif qui décrit un design system en termes que Claude comprend et peut compiler — alternative aux prompts freeform, plus reproductible
- **Recipe system** : templates paramétrés qui s'améliorent à l'usage itératif — pattern "prompt → validate → améliorer le template" applicable à nos agents
- **`/design-workflow make` command** : une seule commande remplace le workflow fragmenté Figma — modèle pour simplifier notre pipeline
- **Knowledge base extraction depuis les librairies Figma** : auto-analyse le design system existant → alimente le compilateur. Synergies avec les DESIGN.md d'awesome-design-md

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : noemuch (solo), 112 stars, actif
- **Dépendances** : Claude Code desktop, Node.js 18+, figma-console-mcp, Figma Desktop/cloud
- **Data** : Les specs design transitent par Claude API (standard). Figma MCP = accès en lecture/écriture sur tes fichiers Figma
- **Verdict** : 🟡 Vigilance — accès write sur Figma via MCP = vérifier les permissions. Claude API standard sinon

### Applicabilité projets

**CinéHome** : Peu applicable directement (pas de workflow Figma dans le pipeline). Mais le pattern CSpec + compiler est inspirant pour créer un format déclaratif pour nos agents (ex: le CTO agent pourrait générer une "TaskSpec" YAML que le Senior Engineer compile en code).

**Equizio** : Directement applicable pour le workflow design des 96 illustrations et du futur admin panel. Décrire un composant → Bridge génère le design Figma conforme au design system → export vers le code.

---

## 2026-04-10 — smallest-agent — Agent minimaliste 493 bytes (code golf)

**URL** : https://github.com/obra/smallest-agent
**Type** : GitHub repo / expérimentation
**Score** : ⭐⭐⭐☆☆ (3/5)
**Tags** : `agent-minimal` `code-golf` `nodejs` `éducatif`

### Objectif
Exploration des limites minimales d'un agent IA fonctionnel. L'agent (493 bytes de JS minifié) a été "lâché sur lui-même" pendant ~20 minutes pour se réduire au maximum tout en restant fonctionnel. Shell 57% + JS 43%. **ATTENTION : accès bash illimité non sandboxé.**

### Ce qu'il y a à garder
- **`src/smallest-agent.commented.js`** : la version annotée est une masterclass sur ce qu'est l'essentiel d'un agent (fetch → parse → execute → loop) — à lire pour comprendre l'archi minimale
- **Prompt loop minimaliste** : pattern de base tool-use → parse response → execute tool → loop, sans framework. Utile pour comprendre ce que Claude Code fait sous le capot
- **Auto-optimisation** : l'agent s'est lui-même réduit → pattern méta-agent "améliore-toi" qu'on pourrait expérimenter dans notre pipeline (feedback-analyst qui améliore ses propres prompts)

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : obra (Jesse Vincent, dev connu), 79 stars, 18 forks
- **Dépendances** : Node.js, npm, Terser pour minification
- **Data** : Appelle l'API Claude/OpenAI configurée — données envoyées selon le provider
- **Verdict** : 🔴 Risque — **bash access illimité et non sandboxé, explicitement documenté dans le README**. "IT CAN DO ANYTHING. IT MIGHT DECIDE TO ERASE ALL YOUR FILES". Usage éducatif UNIQUEMENT, jamais en prod, jamais sur un vrai codebase

### Applicabilité projets

**CinéHome** : Éducatif uniquement. Lire `smallest-agent.commented.js` pour comprendre l'archi agent bare-metal, puis s'en inspirer pour simplifier certains agents du pipeline qui font trop de choses.

**Equizio** : Zéro applicabilité directe.

---

## 2026-04-10 — PraisonAI — Multi-Agent Framework Python (+ TypeScript + Rust)

**URL** : https://github.com/MervinPraison/PraisonAI
**Type** : GitHub repo / framework
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `multi-agent` `python` `mcp` `llm-framework`

### Objectif
Framework Python (+ TS + Rust) pour créer des agents autonomes solo ou multi-agents avec 5 lignes de code. Memory intégrée, RAG intégré, support 100+ LLMs (dont Anthropic), planning mode, MCP stdio/HTTP/WebSocket. Mode Jul côté releases : v4.5.140 le jour même de la veille, ~1 release/jour depuis un an.

### Ce qu'il y a à garder
- **Quick start 3 lignes** : `Agent(instructions="...")` + `.start("tâche...")` — zero boilerplate, idéal pour prototyper un nouveau worker rapidement avant de le formaliser en agent Claude Code
- **MCP support natif** : stdio / HTTP / WebSocket — peut se brancher direct sur nos MCP existants (Supabase, Vercel, code-review-graph)
- **Planning mode** : décompose automatiquement les tâches complexes en sous-tâches → pattern qu'on peut s'inspirer pour le CTO agent (routing + decomposition)
- **Prompt caching + context compaction** intégrés → patterns à regarder pour réduire les tokens dans notre pipeline
- **Dashboard "Claw"** : visual flow builder pour visualiser les pipelines d'agents — alternative plus prod-ready à notre hub pixel art HTML
- **Telemetrie Langfuse** : monitoring de sessions agents avec traces → à investiguer pour tracker token conso par agent (qu'on fait manuellement via `/cost` forum.md)
- **SDK TypeScript `praisonai-ts`** : si on veut des agents côté Next.js (ex: dans cinehome directement sans Python)

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : MervinPraison (principal) + contributeurs communautaires, 3 364 commits, ~1 release/jour
- **Dépendances** : Lourdes — Python + 100+ intégrations LLM, FastAPI, bases de données (PostgreSQL, Redis, MongoDB), SDKs multiples
- **Data** : Envoie les prompts/données aux LLM providers configurés (OpenAI, Anthropic, etc.) — API keys requises, données transitent par les clouds respectifs
- **Verdict** : 🟡 Vigilance — framework légitime et actif, mais exposition data via les LLM providers et surface de dépendances large. Ne jamais passer de données sensibles sans chiffrement/anonymisation

### Applicabilité projets

**CinéHome** : Pas question de remplacer notre pipeline Claude Code par PraisonAI — nos agents sont déjà bien buildés et Claude Code est notre stack. Mais 3 choses à piquer :
1. **Patterns MCP multi-transport** (stdio/HTTP/WS) → améliorer la façon dont nos agents s'interfacent avec les MCP
2. **Planning mode** → enrichir le CTO agent avec une décomposition plus systématique des THREADs
3. **Langfuse integration** → remplacer le `/cost` manuel par du vrai monitoring de pipeline

**Equizio** : Pas directement applicable — framework d'agents multi-agents, pas pertinent pour une app quiz solo. Seul intérêt marginal : le SDK TypeScript `praisonai-ts` pourrait servir si un jour on veut un agent de support intégré dans l'app (ex: chatbot aide aux révisions Galop), mais c'est du nice-to-have lointain.

---

## 2026-04-10 — Arbor — Native Desktop App for Agentic Coding

**URL** : https://github.com/penso/arbor
**Type** : GitHub repo
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `rust` `agentic-coding` `worktree` `mcp`

### Objectif
Application desktop native (Rust + GPUI) pour les workflows de coding agentique. Gère les git worktrees, terminal embarqué, chat avec des agents IA (Claude, Codex, OpenAI-compat), processus, et un serveur MCP intégré. Installable via `brew install penso/arbor/arbor`.

### Ce qu'il y a à garder
- **Worktree management depuis les issues GitHub/GitLab** : crée une branche + worktree automatiquement depuis une issue — chaque agent du pipeline pourrait avoir son propre worktree isolé
- **`arbor-mcp`** : serveur MCP intégré — s'interface directement avec Claude Code et les agents, ouvre des possibilités d'orchestration externe
- **Agent Chat natif** : support Claude + OpenAI-compatible, avec visibilité sur l'agent en cours — potentiellement un meilleur hub de monitoring que l'agent-hub.html actuel
- **Process management via Procfile / arbor.toml** : gérer le lifecycle du pipeline (lancer bug-finder, attendre qa-tester, etc.) avec auto-restart
- **Remote daemon SSH/Mosh** : multi-host, remote worktrees — utile si le pipeline tourne sur un serveur distant
- **593 stars, MIT, Rust nightly** — actif, pas encore mainstream mais momentum réel

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : penso (principal), 506 commits, projet solo avec contributions ponctuelles
- **Dépendances** : Rust nightly (2025-11-30) + GPUI, monorepo Cargo multi-crates. Compilé nativement, pas de dépendances runtime externes lourdes
- **Data** : App desktop native avec accès système complet (terminal PTY, git, SSH/Mosh). Les clés API LLM sont stockées localement. Aucun cloud propriétaire — tout tourne en local
- **Verdict** : 🟡 Vigilance — MIT et local-only c'est bien, mais accès système complet (terminal, git, SSH) = surface d'attaque large si un agent déraille. Rust nightly = potentiel de breaking changes. Projet encore jeune (597 stars)

### Applicabilité projets

**CinéHome** : Directement pertinent pour le pipeline multi-agent. Le `arbor-mcp` peut s'interfacer avec les agents Claude Code existants. La gestion des worktrees git par issue est parfaite pour le workflow `BUG-FINDER → SENIOR-ENGINEER` (chaque fix dans son propre worktree = zéro conflit de file ownership). À investiguer comme alternative/complément au forum.md pour l'orchestration, et comme remplacement du hub pixel art pour le monitoring en temps réel.

**Equizio** : Pas directement applicable — Arbor est un outil d'orchestration d'agents, pas pertinent pour une app quiz. Cependant, si Equizio passe un jour en multi-dev avec des branches par feature (Stripe live, SEO, tests), la gestion worktree automatique depuis les issues GitHub pourrait simplifier le workflow solo dev.

---

## 2026-04-10 — Claude Managed Agents: How to Deploy your First One Today

**URL** : https://return-my-time.kit.com/2872b904f5 (article newsletter)
**Type** : Article / Newsletter
**Score** : ⭐⭐⭐⭐⭐ (5/5)
**Tags** : `claude-agents` `anthropic-api` `managed-agents` `infrastructure`

### Objectif
Présentation de **Claude Managed Agents** (public beta) — Anthropic prend en charge toute l'infra (sandboxing, state management, tool execution) pour déployer des agents Claude en production. Tu définis ce que ton agent fait, ils font tourner ça sur leur cloud. 4 concepts clés : Agent (config), Environment (container), Session (instance active), Events (streaming I/O).

### Ce qu'il y a à garder
- **CLI `ant`** : `brew install anthropic-cli` — outil officiel Anthropic pour setup rapide
- **`agent_toolset_20260401`** : outil tout-en-un qui active bash + file read/write + web search + web fetch + grep + glob en un seul flag
- **Header beta requis** : `"anthropic-beta: managed-agents-2026-04-01"` dans toutes les requêtes API
- **Permission system** : `always_allow` vs `always_ask` **par outil** — hybride possible (read auto, bash requires approval). MCP tools default à `always_ask`. Aucun framework open-source ne fait ça out-of-the-box
- **Pricing** : token rates standards + `$0.08/session-hour` → une session de 10 min = quelques cents
- **Sessions persistantes** : fichiers qui restent en vie, historique de conv, peut tourner des heures
- **Console visuelle** : `platform.claude.com` pour builder les agents sans toucher l'API
- **Vault système** pour les secrets MCP — credentials jamais dans la config agent

### Sécurité
- **Licence** : N/A — article/newsletter, pas de code open-source
- **Mainteneurs** : Anthropic (infra managée officielle)
- **Dépendances** : N/A — service cloud Anthropic, CLI `ant` via Homebrew
- **Data** : Les agents tournent sur le cloud Anthropic dans des containers sandboxés. Les données passent par l'API Anthropic (mêmes conditions que Claude API standard). Vault système pour les secrets MCP
- **Verdict** : 🟢 Safe — infra officielle Anthropic avec sandboxing, permission system granulaire par outil, et vault pour les secrets. Même niveau de confiance que l'API Claude standard

### Applicabilité projets

**CinéHome** : Directement applicable au pipeline multi-agent. Aujourd'hui le pipeline tourne en local via Claude Code avec forum.md comme bus de comm. Managed Agents permettrait de déployer chaque agent (bug-finder, senior-engineer, etc.) comme un agent cloud indépendant avec son propre container et ses propres tools. Le `agent_toolset_20260401` couvre exactement ce dont les workers ont besoin. Le permission system `always_ask` pour les bash commands serait parfait pour le deploy-guard. À explorer dès que le pipeline local est validé.

**Equizio** : Intéressant pour automatiser des tâches récurrentes sans pipeline local. Un Managed Agent dédié pourrait : (1) monitorer les erreurs Supabase et créer des issues automatiquement, (2) vérifier quotidiennement que le build Vercel passe et que Stripe webhook fonctionne, (3) scanner les quiz pour détecter les questions dupliquées ou mal formatées dans la DB. Le pricing `$0.08/session-hour` est négligeable pour des tâches ponctuelles. À considérer quand Equizio sera en Stripe live et aura besoin de monitoring prod.

---

## 2026-04-10 — boneyard

**URL** : https://github.com/0xGF/boneyard
**Type** : GitHub repo / librairie npm
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `skeleton-loading` `react` `vite-plugin` `ux`

### Objectif
Framework TypeScript qui génère automatiquement des skeleton loading screens pixel-perfect à partir de ton UI réelle. Le CLI ouvre un headless browser, visite l'app, capture la layout de chaque `<Skeleton name="...">` à plusieurs breakpoints, et sort des `.bones.json`. Zéro mesure manuelle. Compatible React, Vue, Svelte 5, Angular, React Native. 4270 stars, mis à jour aujourd'hui.

### Ce qu'il y a à garder
- **Pattern wrapping minimal** : `<Skeleton name="blog-card" loading={isLoading}>{contenu réel}</Skeleton>` — aucun markup skeleton à écrire, juste wrapper et nommer
- **Vite plugin** : `boneyardPlugin()` dans `vite.config.ts` = capture automatique au démarrage du dev server + re-capture à chaque HMR. Zéro terminal supplémentaire
- **Multi-breakpoints** : capture aux widths 375/768/1280 par défaut — skeletons responsive out of the box
- **Props utiles** : `animate: 'pulse' | 'shimmer' | 'solid'`, `stagger` (délai entre bones), `transition` (fade out quand loading finit), `darkColor` pour dark mode
- **React Native** : scan via fiber tree + `UIManager`, même format `.bones.json` cross-platform — zero overhead en prod
- **`fixture` prop** : pour le CLI, passer du mock content si le composant a besoin de data pour render — évite de dépendre d'une API en dev

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : 0xGF (principal), 4.3k stars, TypeScript 88.8%
- **Dépendances** : TypeScript, supporte React/Vue/Svelte 5/Angular/React Native. CLI utilise un headless browser (Puppeteer/Playwright) pour capturer les layouts — dev-only, pas en prod
- **Data** : 100% client-side en production. Le CLI capture la layout en dev local uniquement (headless browser sur localhost). Aucune donnée envoyée à l'extérieur, les `.bones.json` restent dans le projet
- **Verdict** : 🟢 Safe — librairie UI pure, client-side only, zéro réseau en prod. Le CLI dev tourne en local. MIT, bien maintenu, aucun risque data

### Applicabilité projets

**CinéHome** : Direct (Next.js + Vite) — brancher `boneyardPlugin()`, wrapper les composants `<BlogCard>`, `<MovieCard>` etc. avec `<Skeleton>`, lancer le build — skeletons générés automatiquement.

**Equizio** : Directement applicable sur les pages quiz qui chargent des données Supabase (liste des galops, questions du quiz, profil utilisateur, leaderboard). Wrapper `<QuizCard>`, `<GalopList>`, `<ProfileStats>`, `<LeaderboardRow>` avec `<Skeleton>` pour remplacer les spinners actuels par des skeletons pixel-perfect. Le multi-breakpoints 375/768/1280 est pile ce qu'il faut pour le responsive mobile-first d'Equizio. Pattern non-invasif = zéro refacto des composants existants, juste wrapping.

---

## 2026-04-10 — Hermes Agent: The Complete Guide (Orange Book)

**URL** : https://github.com/alchaincyf/hermes-agent-orange-book
**Type** : GitHub repo — guide PDF (éducatif, pas un framework)
**Score** : ⭐⭐⭐☆☆ (3/5)
**Tags** : `claude-agents` `memory-system` `skills` `agent-architecture`

### Objectif
Guide complet (17 chapitres, 5 parties, dispo en PDF EN + CN) sur Hermes Agent de Nous Research — un framework d'agent avec learning loops auto-améliorants, mémoire 3 couches, et création automatique de Skills. Fait par HuaShu (花叔), 300K+ followers, série "Orange Book". 1.2k stars, 130 forks.

### Ce qu'il y a à garder
- **Mémoire 3 couches** : short-term (contexte courant) / working memory (interactions récentes) / long-term (knowledge persistant) → à mapper sur notre `forum.md` + memory Claude Code
- **Skills comme unités atomiques réutilisables** : chaque skill = input/output contract clair, composables entre eux → exactement le pattern `.claude/commands/` + subagents
- **5 composantes fondamentales** : instructions, constraints, feedback, memory, orchestration → framework conceptuel utile pour auditer nos 8 agents cinehome
- **Learning loop** : Observe → Analyze → Decide → Act → Reflect → chaque cycle produit des artifacts pour le cycle suivant → à intégrer dans le flow `feedback-analyst` → `cto` → ...
- **"Harness Engineering"** : concept clé — la config du harness (settings, hooks, permissions) EST une compétence à part entière, pas juste du setup

### Sécurité
- **Licence** : CC BY-NC-SA 4.0 (non-commercial, partage sous mêmes conditions)
- **Mainteneurs** : alchaincyf / HuaShu (花叔), créateur de contenu 300K+ followers, 1.4k stars
- **Dépendances** : Aucune — repo de documentation (PDFs + README), pas de code exécutable
- **Data** : Contenu informationnel uniquement. Aucune exécution de code, aucune collecte de données
- **Verdict** : 🟢 Safe — guide PDF éducatif, rien à exécuter. Attention : licence non-commerciale (CC BY-NC-SA 4.0), ne pas réutiliser le contenu dans un produit payant

### Applicabilité projets

**CinéHome** : Pas de code à intégrer directement (c'est un guide pas un lib), mais :
- Le schéma **5 composantes** est un bon outil d'audit pour les agents cinehome — vérifier que chaque agent a bien les 5 dimensions couvertes
- Le concept de **learning loop explicite** manque dans notre pipeline actuel — à ajouter : après chaque THREAD, le CTO devrait logguer un "Reflect" dans forum.md pour améliorer les prochaines itérations
- La **mémoire 3 couches** confirme notre approche `forum.md` (working) + `memory/` Claude (long-term) — on est sur le bon chemin

**Equizio** : Pas directement applicable — guide conceptuel sur l'architecture d'agents, pas pertinent pour une app quiz. Le concept de "learning loop" (Observe → Analyze → Decide → Act → Reflect) pourrait inspirer le système de spaced repetition SM-2 déjà prévu (la boucle de révision des quiz est conceptuellement similaire), mais c'est un stretch.

---

## 2026-04-10 — Top 35 MCP Servers That Turn Claude Into a Productivity Machine

**URL** : https://t.me/zodchixquant (thread Telegram collé manuellement, pas d'URL directe)
**Type** : Article / liste curatée
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `mcp` `claude-agents` `productivity` `pipeline`

### Objectif
Liste curatée de 35 MCP servers testés et retenus par un vibe coder, triés par catégorie (Search, Scraping, Browser, Dev, DB, Memory, Productivity, Finance, Design, Infra). Chaque entrée inclut lien GitHub, pricing, et résumé en 1-2 lignes.

### Ce qu'il y a à garder
- **Context7** : live docs pour n'importe quel framework — Claude arrête d'halluciner des APIs outdatées. Gratuit, open-source. **Must-have pour les agents qui touchent à Next.js/Supabase/Vercel APIs**
- **Firecrawl** : n'importe quelle URL → markdown propre en quelques secondes, JS rendering, full-site crawl. Free: 500 crédits lifetime. Idéal pour `feedback-analyst` qui doit scraper du contenu web
- **Playwright** : Claude contrôle un vrai Chrome — clicks, forms, screenshots, tests sans écrire de scripts. Parfait pour `qa-tester` agent
- **Sentry** : erreurs prod en contexte, stack traces complets dans le prompt. `bug-finder` peut puller les erreurs directement
- **Vercel MCP** : check logs de build, inspecte erreurs, manage projets. `deploy-guard` peut valider le statut avant d'autoriser
- **Tavily** : web search AI-optimized, retourne du contenu propre pas juste des liens. Free 1000 queries/mo — pour les agents qui ont besoin de chercher de l'info
- **Règle d'or** : 3-5 MCP max = sweet spot. Au-delà, on brûle des tokens sur les tool descriptions avant même de poser une question. Claude Code a un lazy-loading (Tool Search) mais garder lean

### Sécurité
- **Licence** : N/A — article curate sur Telegram, pas de code source
- **Mainteneurs** : N/A — liste compilée par un vibe coder, pas un projet maintenu
- **Dépendances** : N/A — chaque MCP listé a ses propres dépendances (voir repos individuels)
- **Data** : Contenu informationnel uniquement. Chaque MCP mentionné a son propre modèle de données — à évaluer individuellement avant installation
- **Verdict** : 🟢 Safe — article informatif, rien à exécuter directement. La sécurité de chaque MCP listé doit être évaluée séparément avant adoption

### Applicabilité projets

**CinéHome** : Directement actionnable sur le pipeline multi-agent :
- Ajouter **Context7** globalement → réduit hallucinations API dans tous les agents
- Brancher **Sentry** sur `bug-finder` → il pull les vraies erreurs prod au lieu de scanner statiquement
- Brancher **Vercel MCP** sur `deploy-guard` → validation de build automatique
- Brancher **Playwright** sur `qa-tester` → tests E2E sans écrire de scripts manuels
- `feedback-analyst` + **Firecrawl** → peut aller chercher du contenu web en plus des iMessages

**Equizio** : Plusieurs MCP directement actionnables sur les gaps identifiés :
- **Sentry MCP** → comble le gap #2 (zéro error monitoring) — brancher Sentry sur equizio.fr et utiliser le MCP pour debugger les erreurs Supabase/Stripe en contexte
- **Playwright MCP** → comble le gap #1 (zéro tests) — Claude peut écrire et runner des tests E2E sur les flows critiques (inscription, achat galop, passage de quiz) sans setup Playwright manuel
- **Context7** → éviter les hallucinations sur les APIs Next.js 14 / Supabase / Stripe lors du dev
- **Vercel MCP** → inspecter les logs de build et les erreurs de déploiement sur equizio.fr directement depuis Claude Code

---

## 2026-04-10 — private-journal-mcp

**URL** : https://github.com/obra/private-journal-mcp
**Type** : GitHub repo / MCP server
**Score** : ⭐⭐⭐☆☆ (3/5)
**Tags** : `claude-agents` `mcp` `memory` `embeddings-local`

### Objectif
MCP server qui donne à Claude un journal privé 100% local — stockage markdown timestampé + recherche sémantique via embeddings générés en local (`@xenova/transformers`, modèle `all-MiniLM-L6-v2`). Zéro cloud, zéro fuite de données.

### Ce qu'il y a à garder
- **Dual storage pattern** : notes projet dans `.private-journal/` (local projet), notes globales dans `~/.private-journal/` — séparation propre à reproduire dans d'autres systèmes de mémoire
- **5 sections sémantiques** : feelings / project_notes / user_context / technical_insights / world_knowledge — bonne taxonomie pour segmenter ce qu'un agent mémorise
- **Local embeddings sans API** : `@xenova/transformers` + ONNX.js = recherche sémantique offline, pattern réutilisable si on veut une search sur nos memories sans passer par OpenZeubi
- **Format entrée** : markdown avec YAML frontmatter + fichier `.embedding` JSON associé — simple et extensible

### Sécurité
- **Licence** : MIT
- **Mainteneurs** : obra (Jesse Vincent), 324 stars, TypeScript 95.6%
- **Dépendances** : TypeScript + `@xenova/transformers` (ONNX.js) pour embeddings locaux. Pas de SDK cloud, pas d'API externe
- **Data** : 100% local et offline. Stockage markdown dans `.private-journal/` (projet) et `~/.private-journal/` (global). Embeddings generees en local via ONNX — aucune donnee ne quitte la machine, jamais
- **Verdict** : 🟢 Safe — architecture privacy-first exemplaire. Zéro cloud, zéro API, zéro fuite. Exactement ce qu'on veut pour un journal privé. Dépendances minimales et bien connues

### Applicabilité projets

**CinéHome** : Peu pertinent pour remplacer notre système memory actuel (trop petit volume, déjà bien structuré). Valeur potentielle : si le pipeline multi-agents grossit et qu'on veut que les agents puissent **retrouver** des insights passés via recherche sémantique plutôt que lecture linéaire. Le dual-storage pattern est à garder en tête pour la v2 des templates agents dans `claude-divers/templates/agents/`.

**Equizio** : Pas directement applicable comme MCP server. Cependant, le pattern **local embeddings** (`@xenova/transformers` + ONNX.js) est intéressant pour une future feature de recherche sémantique dans les quiz : un utilisateur qui tape "comment mettre un filet" retrouverait les questions sur l'équipement du cheval même sans match exact de mots-clés. Faisable côté client (ONNX.js tourne dans le browser) sans coût API. À garder en tête pour la v2 post-Stripe-live.

---

## 2026-04-10 — Guide Lovable Animations (GSAP + Three.js)

**URL** : https://x.com/damienghader/status/2033879887233142955?s=20
**Type** : Thread X / guide pratique
**Score** : ⭐⭐⭐⭐☆ (4/5)
**Tags** : `lovable` `gsap` `three.js` `prompt-engineering` `animation`

### Objectif
Guide de prompt engineering pour générer des animations scroll cinématiques (GSAP ScrollTrigger, Three.js, shaders) dans Lovable sans toucher un IDE. Basé sur 3 projets réels : Nøva, Oak Atelier, VØID Intelligence.

### Ce qu'il y a à garder
- **Pinner Three.js** : toujours spécifier `three@0.136.0` dans le prompt — les version mismatches cassent silencieusement
- **Le pont scrollProgress** : une variable `scrollProgress` (0→1) pilote tout le 3D via `ScrollTrigger.create` + `onUpdate` → pattern réutilisable dans n'importe quel projet Three.js
- **Canvas fixed + pointer-events: none** : layer 3D fixe sous le HTML scrollable — évite tous les conflits d'interaction
- **clamp() pour la typo** : `clamp(min, Xvw, max)` sur tous les titres, jamais de breakpoints typographiques
- **Specs d'animation précises** : "animate from x:-100, scale:0.3, rotation:15" > "slide in" — la précision = le résultat
- **overflow-x: hidden** sur html + body + #root : obligatoire pour éviter le scroll horizontal mobile
- **Template de prompt universel** : structure en blocs (Tech & Libraries / Color Palette / Sections / Animations / Responsive) — directement adaptable pour Lovable ou Cursor

### Sécurité
- **Licence** : N/A — thread X / guide pratique, pas de repo avec licence
- **Mainteneurs** : N/A — @damienghader, créateur de contenu, pas un projet maintenu
- **Dépendances** : N/A — guide de prompt engineering, pas de code à installer. Recommande Three.js + GSAP (librairies établies et bien auditées)
- **Data** : Contenu informationnel uniquement. Aucune exécution de code, aucune collecte de données
- **Verdict** : 🟢 Safe — guide/tutorial informatif, rien à exécuter. Les librairies recommandées (GSAP, Three.js) sont des standards de l'industrie

### Applicabilité projets

**CinéHome** : Adapter le template de prompt pour du vibe coding dans n'importe quel projet Next.js avec animations. Le pattern scrollProgress peut directement entrer dans un agent `senior-engineer` qui génère des pages d'accueil animées. Le guide Lovable = base pour créer un skill `/lovable-animated-page` si besoin.

**Equizio** : Applicable pour la landing page / page marketing d'Equizio. Le pattern `scrollProgress` + GSAP ScrollTrigger peut rendre la page d'accueil plus engageante (animation des galops 1-9 au scroll, illustrations cheval qui défilent, stats de progression). Le `clamp()` pour la typo responsive et le `overflow-x: hidden` sont des quick wins applicables immédiatement. Utile aussi pour les 96 illustrations Midjourney à venir — les animer au scroll plutôt que les afficher statiquement.

---
