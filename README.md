# skill-videoprodutor â€” o Produtor

> **Norte:** entro com **um link / fonte de dados** â†’ sai o **plano + a execuÃ§Ã£o** de um **vÃ­deo profissional** (propaganda ou explicativo). Uma coisa sÃ³, ponta a ponta.

**videoprodutor** ("o Produtor") Ã© o **orquestrador** do ecossistema de vÃ­deo do usuÃ¡rio, empacotado como skill
instalÃ¡vel. Ele nÃ£o reinventa render nem geraÃ§Ã£o â€” **coordena as peÃ§as que jÃ¡ existem** (planejamento, direÃ§Ã£o,
prompt, imagem, voz, render) numa linha de montagem Ãºnica. SaÃ­da em **16:9 e 9:16**, dark premium INEMA.CLUB,
**tudo local** (sem chave de API). Imagem (flux2-klein) Ã© o padrÃ£o; **SVG Ã© fallback automÃ¡tico** quando nÃ£o hÃ¡
servidor de imagem.

**VersÃ£o da Skill:** `v0.2.0` Â· esquema `vX.yy.xxx` (yy = recurso, xxx = bug) Â· histÃ³rico em [`skill/videoprodutor/CHANGELOG.md`](skill/videoprodutor/CHANGELOG.md).

## Por que existe

O usuÃ¡rio jÃ¡ tem ~11 skills e 6 motores de vÃ­deo espalhados. **~70% da fÃ¡brica jÃ¡ existe**, mas falta a peÃ§a que
**conecta o link Ã  execuÃ§Ã£o automÃ¡tica**. Esta skill Ã© essa peÃ§a.

DecisÃ£o estratÃ©gica: **nÃ£o refazer do zero, nÃ£o manter silos** â€” montar a fÃ¡brica reusando o que jÃ¡ funciona,
com Remotion como motor de render unificado (camada cinema do pixflow + componentes do remotion-templates para
texto cinÃ©tico e ilustraÃ§Ã£o de tÃ³pico).

## A linha de montagem

```
LINK / FONTE
  â†’ entender (tema, dados, Ã¢ngulo)
  â†’ PLANO        (video-plan-editor â†’ plano-edicao.json: roteiro, beats, tÃ³picos, CTA)
  â†’ DIREÃ‡ÃƒO+IMG  (mdd/promptprof brief fiel â†’ flux2-klein imagem  OU  SVG auto)
  â†’ VOZ+TIMING   (inemavox/Kokoro narraÃ§Ã£o + duraÃ§Ãµes + timestamps)
  â†’ RENDER       (3 camadas: parallax/cinema + texto cinÃ©tico + ilustraÃ§Ã£o de tÃ³pico)
  â†’ VÃDEO PROFISSIONAL  (preset: propaganda | explicativo Â· 16:9 + 9:16)
```

## Instalar

```bash
npx skills add inematds/skill-videoprodutor
```

Ou manualmente: copie/symlink `skill/videoprodutor` para `~/.claude/skills/videoprodutor`.

## O que ela faz

As **3 camadas** que tornam um vÃ­deo "profissional" (nÃ£o slideshow):

1. **Cinema** â€” fundo com profundidade (imagem + vÃ©u + parallax/ken-burns).
2. **Texto cinÃ©tico** â€” nÃºmero/tÃ­tulo/Ãªnfase animados (GSAP).
3. **IlustraÃ§Ã£o do tÃ³pico** â€” Ã­cone/diagrama/contador que mostra o que se narra (SVG animado no fallback).

Linha de montagem: `link â†’ plano (vpe) â†’ roteiro â†’ voz (Kokoro) â†’ arte (flux2-klein OU SVG auto) â†’ compor (3 camadas) â†’ validar â†’ render 16:9 + 9:16`.

## IntegraÃ§Ã£o: `remotion-templates` (peÃ§a oficial das camadas 2 e 3)

Os [**remotion-templates**](https://github.com/inematds/remotion-templates) â€” 81 componentes Remotion (React, sÃ³ hooks do Remotion, sem CSS keyframes nem libs externas) â€” sÃ£o a biblioteca que o Produtor usa para **texto cinÃ©tico** e **ilustraÃ§Ã£o de tÃ³pico**. O mapa Ã© quase 1:1 com as 3 camadas:

| Camada | Categorias do remotion-templates |
|---|---|
| **1 Â· Cinema** | Cinematic (ken-burns, parallax-pan, vignette, film-burn) Â· Background Â· Transition |
| **2 Â· Texto cinÃ©tico** | Text (animated-text, typewriter, glitch, slide, pulsing) Â· Intro/Outro (chapter-title, lower-third, end-card) |
| **3 Â· IlustraÃ§Ã£o do tÃ³pico** | Charts & Data (stat-counter, progress, comparison, donut) Â· Content Animation (animated-list, progress-steps, countdown) |

**O melhor lar do `remotion-templates` Ã© o videoprodutor** (que jÃ¡ o adota como peÃ§a oficial) e o
[**video-plan-editor**](https://github.com/inematds/skill-video-plan-editor) (render nativo Remotion no
roadmap); o [**pixflow**](https://github.com/inematds/pixflow) Ã© o par complementar perfeito â€” faz o fundo
cinema, enquanto o remotion-templates faz o texto/dados por cima (mesmo motor Remotion no pipeline). As
skills `remotion`/`remotion-best-practices` o aproveitam como referÃªncia.

## ConteÃºdo

- `skill/videoprodutor/SKILL.md` â€” gatilho + fluxo + regras de ouro.
- `skill/videoprodutor/references/` â€” pipeline, mapa-libs-motion (qual lib por tarefa), safe-zones (9:16), fallback-svg.
- `skill/videoprodutor/scripts/` â€” molde testado: `composition-template.mjs` (3 camadas, auto imagemâ†’SVG, safe zones), `gen-imgs.mjs` (flux2-klein), `svg-icons.mjs` (Ã­cones SVG animados), `fetch-fonts.mjs`.

## DependÃªncias (resumo)

Node 22+, FFmpeg, HyperFrames (`npx hyperframes`), internet no render (GSAP via CDN), Kokoro TTS + GPU (voz).
Opcional (com fallback SVG): servidor de imagem inemaimg/flux2-klein em `:8000`.

PeÃ§as reusÃ¡veis vivem em projetos prÃ³prios: `~/projetos/pixflow` (render cinema), `~/projetos/skill-video-plan-editor`,
`~/projetos/mdd`, `~/projetos/promptprof`, `~/projetos/fontefilm`, `~/projetos/remotion-templates`,
`~/projetos/inemaimg` (flux2-klein), `~/projetos/inemavox` (voz).

## Origem

ExtraÃ­da e consolidada do projeto de arquitetura `videoprodutor` (docs `00..08` + caso de referÃªncia funcional
`videos/hormozi-12-dicas`, 3 camadas validadas). Esta skill Ã© a destilaÃ§Ã£o executÃ¡vel daquela anÃ¡lise.

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe