# Veille Tech — Log

Log de veille alimenté automatiquement via la commande `/veille <url>`.
Entrées triées de la plus récente à la plus ancienne.

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

### Applicabilité projet
Directement pertinent pour le pipeline multi-agent cinehome. Le `arbor-mcp` peut s'interfacer avec les agents Claude Code existants. La gestion des worktrees git par issue est parfaite pour le workflow `BUG-FINDER → SENIOR-ENGINEER` (chaque fix dans son propre worktree = zéro conflit de file ownership). À investiguer comme alternative/complément au forum.md pour l'orchestration, et comme remplacement du hub pixel art pour le monitoring en temps réel.

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

### Applicabilité projet
**Directement applicable au pipeline multi-agent cinehome.** Aujourd'hui le pipeline tourne en local via Claude Code avec forum.md comme bus de comm. Managed Agents permettrait de déployer chaque agent (bug-finder, senior-engineer, etc.) comme un agent cloud indépendant avec son propre container et ses propres tools. Le `agent_toolset_20260401` couvre exactement ce dont les workers ont besoin. Le permission system `always_ask` pour les bash commands serait parfait pour le deploy-guard. À explorer dès que le pipeline local est validé.

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

### Applicabilité projet
Direct pour **cinehome** (Next.js + Vite) : brancher `boneyardPlugin()`, wrapper les composants `<BlogCard>`, `<MovieCard>` etc. avec `<Skeleton>`, lancer le build — skeletons générés automatiquement.
Idem pour **Equizio** (Next.js). Gain de temps réel vs coder les skeletons à la main ou utiliser des placeholders approximatifs. Pattern simple et non-invasif : wrapping pur, pas de refacto du composant sous-jacent.

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

### Applicabilité projet
Pas de code à intégrer directement (c'est un guide pas un lib), mais :
- Le schéma **5 composantes** est un bon outil d'audit pour les agents cinehome — vérifier que chaque agent a bien les 5 dimensions couvertes
- Le concept de **learning loop explicite** manque dans notre pipeline actuel — à ajouter : après chaque THREAD, le CTO devrait logguer un "Reflect" dans forum.md pour améliorer les prochaines itérations
- La **mémoire 3 couches** confirme notre approche `forum.md` (working) + `memory/` Claude (long-term) — on est sur le bon chemin

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

### Applicabilité projet
Directement actionnable sur le pipeline multi-agent cinehome :
- Ajouter **Context7** globalement → réduit hallucinations API dans tous les agents
- Brancher **Sentry** sur `bug-finder` → il pull les vraies erreurs prod au lieu de scanner statiquement
- Brancher **Vercel MCP** sur `deploy-guard` → validation de build automatique
- Brancher **Playwright** sur `qa-tester` → tests E2E sans écrire de scripts manuels
- `feedback-analyst` + **Firecrawl** → peut aller chercher du contenu web en plus des iMessages

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

### Applicabilité projet
Peu pertinent pour remplacer notre système memory actuel (trop petit volume, déjà bien structuré). Valeur potentielle : si le pipeline multi-agents grossit et qu'on veut que les agents puissent **retrouver** des insights passés via recherche sémantique plutôt que lecture linéaire. Le dual-storage pattern est à garder en tête pour la v2 des templates agents dans `claude-divers/templates/agents/`.

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

### Applicabilité projet
Adapter le template de prompt pour du vibe coding dans n'importe quel projet Next.js avec animations. Le pattern scrollProgress peut directement entrer dans un agent `senior-engineer` qui génère des pages d'accueil animées. Le guide Lovable = base pour créer un skill `/lovable-animated-page` si besoin.

---
