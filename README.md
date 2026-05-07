# Claude Skill Creator

> **In short:** Claude Code is Anthropic's CLI tool for AI-assisted development. A "skill" is a markdown file that tells Claude Code how to behave when triggered. This repo gives you an automated workflow to create those skills well, instead of writing them from scratch by trial and error.

A meta-skill for Claude Code that helps you create, test, iterate, and improve custom skills through a structured menu-driven conversational workflow. Skills are plain markdown — other LLMs can interpret them, but only Claude Code has been tested.

## What is a skill

A skill is a markdown file placed in a known directory that instructs an LLM how to behave when a specific trigger is detected. The LLM reads the file and follows the instructions. No runtime, no compilation, no API — just structured text that shapes behavior.

Different LLMs load skills from different locations:

| LLM | Skill location |
| --- | -------------- |
| Claude Code | `~/.claude/skills/[skill-name]/SKILL.md` |
| Cursor AI | `.cursorrules` or `.cursor/rules/` |
| Windsurf | `.windsurfrules` |
| ChatGPT | Custom instructions or system prompt |
| Generic | Any markdown file referenced in context |

The format this tool generates is designed for Claude Code. Other LLMs may interpret the same markdown instructions, but cross-platform compatibility has not been formally tested. Contributions welcome for verified adapters.

## Why this exists

Creating a good skill requires a specific discipline: clearly defining triggers, expected behavior, constraints, and output format. Without structure, you end up with vague prompts that produce inconsistent results.

The difference between asking Claude "help me write a skill" bare and using this tool is the structured interview. The menu forces you to think through purpose, triggers, complexity, tools, and output format before any generation happens. That specificity is what produces skills that work reliably instead of skills that work sometimes.

The design philosophy comes from a deliberation process (LLM Council with 5 independent advisors and peer review) that converged on one principle: the conversation is the product, not the folder structure.

## How it works

The Skill Creator operates in five phases.

### Phase 1 - Menu interview

The skill presents a structured menu with 7 questions:

1. **Purpose** — What type of skill? (code generation, analysis, workflow, content, data processing, custom)
2. **Trigger style** — Slash command, auto-detect, or both?
3. **Tools needed** — None, file system, shell, web, or multiple?
4. **Output format** — Code, text, structured data, mixed, or files + report?
5. **Boundaries** — What should this skill explicitly REFUSE to do?
6. **Autonomy level** — Ask before acting, act then confirm, or full autonomy?
7. **Description + example** — What should the skill do, and what does a concrete input/output look like?

If you provide a precise specification that answers these questions upfront, the menu is skipped and generation begins immediately. If you mention a non-Claude target LLM, the output is adapted to that platform's conventions.

### Phase 2 - Generate first draft

Based on your answers, the skill generates a complete structured markdown file with: trigger definition, numbered process steps, constraints (including your stated boundaries), and output format. Generated skills are capped at 150 lines — if longer, the skill is split. The file is placed in `~/.claude/skills/[skill-name]/SKILL.md`. The directory is created automatically if it does not exist.

### Phase 3 - Council validation

Before showing you the result, the generated skill is validated through one of two paths:

**Path A — LLM Council (recommended):** If the `llm-council` skill is installed, the draft is submitted to 5 independent advisors who evaluate quality, safety, edge cases, and clarity. The verdict is presented to you with what works, what was flagged, and specific fixes. Fixes are applied before you see the final draft. This is real peer review, not self-evaluation.

**Path B — Structural checklist (fallback):** If the Council is unavailable, the skill self-checks against 7 criteria: trigger clarity, imperative steps, constraints present (including boundaries you specified), output explicit, length under 150 lines, no contradictions, no trigger conflicts. Failures are fixed before showing you the result.

**Note:** Council validation requires the `llm-council` skill to be installed separately. Running the full Council spawns 5 sub-agents and consumes additional tokens — this is a quality tradeoff, not a requirement. Without it, the fallback checklist is used automatically and still catches structural issues.

### Phase 4 - Iterate with user

The generated skill is shown to you with the question: "What would you change?" Based on your feedback, the skill is edited directly and shown again. This loop repeats until you are satisfied.

### Phase 5 - Finalize and try it

Once approved, the skill confirms:

- The file location and trigger command
- 2-3 edge cases to watch for
- A concrete test instruction: "Open a new session and try: `[example prompt]`"

The real test happens in a fresh session where the skill is loaded from disk. This is intentional — it is the only honest way to validate that the skill works as intended.

## How Council validation works

Every skill created by this tool passes through a quality assurance step before reaching you. Here is what happens concretely:

1. The menu poses the 7 questions
2. Claude generates a first draft of the skill
3. The LLM Council intervenes — 5 independent advisors critique the generated skill (quality, safety, edge cases, clarity)
4. The Council's corrections are applied to the draft
5. You iterate on the result and test in a new session

Each skill created goes through this peer review before being delivered to you. It is not Claude validating its own work — it is 5 distinct perspectives looking for flaws.

### How the Council works internally

The Council uses 5 thinking roles, each with a different angle of critique:

| Role | Focus |
| ---- | ----- |
| The Contrarian | What will fail? What is missing? What is wrong? |
| The First Principles Thinker | Is this solving the right problem? Strip assumptions. |
| The Expansionist | What upside is being missed? What could be bigger? |
| The Outsider | What would confuse someone with zero context? |
| The Executor | Can this actually be done? What breaks in practice? |

These are not 5 different AI models. They are 5 independent prompts to the same model, each constrained to a specific thinking style. The diversity comes from the roles, not from the architecture. This is adapted from Andrej Karpathy's LLM Council methodology.

### Important nuances

- **Optional:** If the `llm-council` skill is not installed, the Skill Creator falls back to a structural checklist (less thorough but functional)
- **Token cost:** Running the full Council spawns 5 sub-agents, which consumes additional tokens. This is a quality tradeoff — you get better skills at higher cost per creation
- **Not a permanent supervisor:** The Council runs once per skill creation, at Phase 3. It does not monitor skills after they are deployed

## Installation

### Option 1 - Copy to your skills directory (recommended)

```bash
git clone https://github.com/valorisa/Claude-Skill-Creator.git
mkdir -p ~/.claude/skills
cp -r Claude-Skill-Creator/skill-creator ~/.claude/skills/skill-creator
```

Verify it worked:

```bash
ls ~/.claude/skills/skill-creator/SKILL.md
```

On Windows (PowerShell):

```powershell
git clone https://github.com/valorisa/Claude-Skill-Creator.git
New-Item -ItemType Directory -Force -Path $env:USERPROFILE\.claude\skills
Copy-Item -Recurse Claude-Skill-Creator\skill-creator $env:USERPROFILE\.claude\skills\skill-creator
```

Verify it worked:

```powershell
Test-Path $env:USERPROFILE\.claude\skills\skill-creator\SKILL.md
```

### Option 2 - Symlink for easy updates

Clone to any location you prefer (adjust paths as needed):

On macOS/Linux:

```bash
git clone https://github.com/valorisa/Claude-Skill-Creator.git ~/projects/Claude-Skill-Creator
ln -s ~/projects/Claude-Skill-Creator/skill-creator ~/.claude/skills/skill-creator
```

On Windows (requires Administrator or Developer Mode):

```powershell
git clone https://github.com/valorisa/Claude-Skill-Creator.git $env:USERPROFILE\projects\Claude-Skill-Creator
New-Item -ItemType SymbolicLink -Path $env:USERPROFILE\.claude\skills\skill-creator -Target $env:USERPROFILE\projects\Claude-Skill-Creator\skill-creator
```

### Verify installation

Start a new Claude Code session and type `/skill-creator`. Claude should respond by presenting the interview menu or asking what skill you want to create.

## Usage examples

### Example 1 — Create a skill from the menu

```text
> /skill-creator

[Claude presents the 7-question menu]

> Purpose: Code analysis
> Trigger: /explain-error (slash command)
> Tools: None
> Output: Text response
> Boundaries: Never guess if unsure — say "I don't recognize
  this error" instead of inventing explanations
> Autonomy: Act then confirm (show explanation, ask if helpful)
> Description: Takes an error message and explains what it means,
  why it happened, and how to fix it.
  Example input: "TypeError: Cannot read properties of undefined"
  Example output: A 3-paragraph explanation with cause + fix.

[Claude generates the skill]
[LLM Council reviews it: 5 advisors evaluate quality and safety]
[Council verdict shown: what works, what to fix]
[Fixes applied, user iterates until satisfied]
[Claude says: "Open a new session and try: paste an error message
 and say /explain-error"]
```

### Example 2 — Skip the menu with a precise spec

```text
> Create a skill called 'changelog-gen' for Claude Code that reads
  git log since last tag, groups commits by type (feat/fix/chore),
  and generates a CHANGELOG.md entry. Trigger: /changelog.
  Needs shell access for git commands. Output: markdown file.

[Claude recognizes all parameters are provided]
[Generates immediately without menu]
[Council validates, shows verdict, iterates]
```

### Example 3 — Create a skill for a different LLM

```text
> /skill-creator

> Purpose: Content creation
> Trigger: Auto-detect when user writes "generate docs"
> Tools: File system
> Output: Code (markdown files)
> Boundaries: Never modify source files, only generate new doc files
> Autonomy: Ask first (confirm which files to document)
> Description: Generate API documentation from source code comments
  and function signatures. Target: Cursor AI.
  Example input: a TypeScript file with exported functions
  Example output: a markdown file with function signatures + descriptions

[Claude detects "Target: Cursor AI" in description]
[Adapts output to .cursor/rules/ conventions]
[Council validates, iterates, finalizes]
```

### Example 4 — Improve an existing skill

```text
> My skill /pr-review takes too long and gives too many nitpick
  suggestions. Help me tighten it to focus only on bugs and
  security issues.

[Claude reads the existing skill]
[Identifies verbose sections and low-priority checks]
[Proposes targeted edits]
[Iterates until user approves]
```

## Project structure

```text
skill-creator/
  SKILL.md                        The skill itself
  templates/
    simple-skill.md               Example: commit message writer
    multi-step-skill.md           Example: API scaffolder
    interactive-skill.md          Example: doc translator
    workflow-skill.md             Example: PR reviewer
    data-processing-skill.md     Example: log analyzer
  tests/
    test-prompts.md              5 validation scenarios
```

### About the templates

The templates folder contains five reference skills covering different categories:

| Template | Type | Complexity | Tools |
| -------- | ---- | ---------- | ----- |
| simple-skill.md | Code generation | Simple | Shell (git) |
| multi-step-skill.md | Code generation | Multi-phase | File system |
| interactive-skill.md | Content creation | Interactive | File system |
| workflow-skill.md | Code analysis | Multi-phase | Shell (gh) |
| data-processing-skill.md | Data processing | Interactive | File system |

They serve as pattern references for the LLM when generating your new skill. The LLM adapts structure and depth to match what your skill actually needs.

### About the tests

The tests folder contains five scenarios covering:

1. Vague request (menu should appear and guide the user)
2. Precise specification (menu should be skipped)
3. Improvement of existing skill (read, analyze, propose edits)
4. Non-Claude target LLM (adapt to different conventions)
5. Complex multi-tool skill (handle dependencies and edge cases)

Use them to verify the skill creator behaves correctly after modifications.

## Design principles

1. **A skill is just a markdown file.** No build steps, no dependencies, no runtime. If you need more than a markdown file, you are building something else.

2. **The conversation is the product.** The value lives in the structured interview that forces specificity. The menu extracts intent. The validation catches errors. The iteration refines.

3. **Peer review over self-grading.** Generated skills are validated by the LLM Council (5 independent advisors), not by the same LLM that wrote them. The human makes the final call in a fresh session with a real task.

4. **Ship then improve.** A working skill with rough edges beats a perfect design document. Generate, validate, iterate, test in a new session. Do not plan in the abstract.

5. **No premature infrastructure.** No registries, no pattern libraries, no composition engines. Add complexity only when you have five working skills that demand it.

6. **Honest about scope.** This tool is designed and tested for Claude Code. Generated skills are plain markdown that other LLMs may interpret, but no cross-platform guarantee is made without explicit testing.

## Optional extensions

### LLM Council (recommended)

Phase 3 uses the `llm-council` skill for multi-perspective peer review of generated skills. Without it, a structural checklist is used as fallback — functional but less thorough.

The Council skill is not bundled in this repo. It evolves independently and bundling would create silent version drift. Install it from its upstream source:

```bash
git clone https://github.com/tenfoldmarc/llm-council-skill.git
mkdir -p ~/.claude/skills/llm-council
cp llm-council-skill/SKILL.md ~/.claude/skills/llm-council/SKILL.md
```

On Windows (PowerShell):

```powershell
git clone https://github.com/tenfoldmarc/llm-council-skill.git
New-Item -ItemType Directory -Force -Path $env:USERPROFILE\.claude\skills\llm-council
Copy-Item llm-council-skill\SKILL.md $env:USERPROFILE\.claude\skills\llm-council\SKILL.md
```

Source: [tenfoldmarc/llm-council-skill](https://github.com/tenfoldmarc/llm-council-skill) — MIT License, adapted from Andrej Karpathy's LLM Council methodology.

## Requirements

- Claude Code CLI (any recent version)
- Works on any platform (Windows, macOS, Linux)
- No external dependencies
- No admin privileges required (copy installation; symlink option requires admin on Windows)
- Optional: `llm-council` skill for full Council validation (see above)

## Uninstall

Remove the skill directory:

```bash
rm -rf ~/.claude/skills/skill-creator
```

On Windows (PowerShell):

```powershell
Remove-Item -Recurse -Force $env:USERPROFILE\.claude\skills\skill-creator
```

If you used the symlink installation (Option 2), also remove the cloned repo:

```bash
rm -rf ~/projects/Claude-Skill-Creator
```

This leaves no other traces. No config files, no registry entries, no global state.

## Contributing

Contributions are welcome. The bar for inclusion is simple: does this change make it faster to go from intent to working skill? If yes, open a pull request.

Areas where contributions would be particularly valuable:

- Additional template examples covering new skill categories
- Translations of the SKILL.md for non-English users
- Adaptations for additional LLM platforms
- Bug reports from edge cases encountered during real usage

## License

MIT License. See the LICENSE file for details.

## Comment fonctionne Claude Skill Creator (FR)

### En une phrase

C'est un fichier markdown (`SKILL.md`) qui dit à Claude Code comment vous guider dans la création d'autres fichiers markdown (skills).

### Le principe

Quand vous tapez `/skill-creator` dans Claude Code, Claude lit le fichier `~/.claude/skills/skill-creator/SKILL.md` et suit ses instructions à la lettre. Ce fichier décrit un processus en 5 phases.

### Phase 1 — Menu (7 questions)

Claude vous pose 7 questions pour comprendre ce que vous voulez :

| # | Question | Pourquoi |
| - | -------- | -------- |
| 1 | Quel type de skill ? | Choisir le bon pattern |
| 2 | Quel déclencheur ? | Slash command, auto-détection, ou les deux |
| 3 | Quels outils ? | Ce que la skill pourra faire (fichiers, shell, web) |
| 4 | Quel format de sortie ? | Code, texte, données structurées, mixte |
| 5 | Que doit-elle REFUSER de faire ? | Garde-fous et limites |
| 6 | Niveau d'autonomie ? | Demander avant d'agir, ou agir directement |
| 7 | Description + exemple concret | L'intent + un cas réel d'input/output |

Si vous donnez une spec précise d'entrée, le menu est sauté.

### Phase 2 — Génération

Claude écrit un fichier SKILL.md complet avec :

- Un avertissement de sécurité (permissions, pas d'opérations destructives sans confirmation)
- Section Trigger
- Section Process (étapes numérotées, impératives)
- Section Constraints (dont vos boundaries de la Q5)
- Section Output

Le fichier est plafonné à 150 lignes. Le dossier est créé automatiquement.

### Phase 3 — Validation par le Council

**Si `llm-council` est installé :** 5 conseillers indépendants critiquent la skill générée :

| Rôle | Question posée |
| ---- | -------------- |
| Contrarian | Qu'est-ce qui va casser ? |
| First Principles | Est-ce le bon problème ? |
| Expansionist | Qu'est-ce qu'on rate ? |
| Outsider | C'est confus pour un nouveau ? |
| Executor | Ça marche en pratique ? |

Leurs corrections sont appliquées avant de vous montrer le résultat.

**Si `llm-council` n'est pas installé :** une checklist structurelle vérifie 7 critères (trigger clair, étapes impératives, contraintes présentes, output explicite, < 150 lignes, pas de contradictions, pas de conflits).

### Phase 4 — Itération

Claude vous montre le résultat et demande : "Qu'est-ce que tu changerais ?" Vous donnez du feedback, Claude corrige, vous redemande. Boucle infinie jusqu'à votre satisfaction.

### Phase 5 — Finalisation + vrai test

Claude confirme l'emplacement du fichier, suggère 2-3 edge cases, et vous dit :

> "Ouvre une nouvelle session et essaie : [prompt concret]"

Le vrai test se fait dans une session fraîche où la skill est chargée depuis le disque.

### Ce que l'utilisateur installe

Uniquement le dossier `skill-creator/` copié dans `~/.claude/skills/`. Le reste (README, .github, etc.) c'est l'habillage GitHub pour la communauté.

### La chaîne de dépendances

```text
L'utilisateur tape /skill-creator
        ↓
Claude Code lit ~/.claude/skills/skill-creator/SKILL.md
        ↓
Suit le processus en 5 phases
        ↓
(optionnel) Invoque ~/.claude/skills/llm-council/SKILL.md pour valider
        ↓
Produit ~/.claude/skills/[nouvelle-skill]/SKILL.md
```

Tout est markdown. Rien d'autre.

## Credits

- Design methodology informed by three rounds of LLM Council deliberation (5 independent advisors with peer review, adapted from Andrej Karpathy's LLM Council concept)
- LLM Council also serves as the runtime validation mechanism for generated skills
- Built for use with Claude Code by Anthropic
