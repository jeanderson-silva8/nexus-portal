# 🔮 NEXUS PORTAL — Experiência 3D Interativa

> Experiência web cinematográfica com **cena 3D em tempo real**, shaders customizados, animações GSAP e personalização visual — construída com React, Three.js e TypeScript.

[![Acessar Experiência](https://img.shields.io/badge/🌐_ACESSAR-nexus--portal.vercel.app-8B5CF6?style=for-the-badge)](https://nexus-portal-one.vercel.app)
![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-r184-000000?style=for-the-badge&logo=three.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![GSAP](https://img.shields.io/badge/GSAP-3-88CE02?style=for-the-badge&logo=greensock&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-7-646CFF?style=for-the-badge&logo=vite&logoColor=white)

🟢 **LIVE DEMO:** [Acesse o NEXUS PORTAL Ao Vivo Aqui](https://nexus-portal-one.vercel.app)
🛡️ **Auditoria de Segurança Aplicada:** [Veja a auditoria 2026-05-21](docs/AUDIT_REPORT_2026-05-21.md)

<div align="center">
  <div style="max-width: 800px; background-color: #161b22; border: 1px solid #30363d; border-bottom: none; border-top-left-radius: 8px; border-top-right-radius: 8px; padding: 10px; font-family: monospace; font-size: 13px; color: #8b949e; text-align: left;">
    🔴 &nbsp; 🟡 &nbsp; 🟢 &nbsp;&nbsp;&nbsp; <b>~/nexus-portal</b>
  </div>
  <video src="https://github.com/user-attachments/assets/6e9159c5-2600-432a-9db8-a78dd9571b49"></video>
</div>

---

## 🌌 Sobre o Projeto

A maioria dos portfólios de desenvolvedor frontend é uma página estática — bonita, mas que não prova domínio técnico real. Renderização 3D em tempo real no navegador, shaders customizados e animação sincronizada ao scroll são habilidades difíceis de demonstrar com uma landing page comum. Quem olha não consegue distinguir "sabe CSS" de "sabe computação gráfica".

O **NEXUS PORTAL** é a solução para isso: uma **experiência web imersiva** que combina computação gráfica 3D em tempo real com design de interface premium — um showcase técnico que prova, na prática, domínio de WebGL, shaders GLSL e animação de alto desempenho.

O artefato Nexus — um anel tridimensional com shader próprio — gira num campo de partículas e estrelas procedurais, enquanto o usuário navega por seções com scroll suave e transições cinematográficas. A cena 3D é renderizada continuamente a 60fps, e o usuário pode personalizar cor, brilho e densidade de partículas ao vivo.

---

## ⚠️ Escopo deste projeto

Nexus Portal é um **showcase técnico de frontend** — uma experiência 3D estática servida como HTML/JS/CSS pela CDN da Vercel.

- **Sem backend, sem banco, sem autenticação** — não há dados de usuário, não há API. É intencional: o objetivo é demonstrar engenharia de frontend/WebGL, não arquitetura de servidor.
- **Sem persistência** — a personalização (cor, brilho, partículas) vive só no estado da sessão (Zustand), não é salva.
- A [auditoria de segurança 2026-05-21](docs/AUDIT_REPORT_2026-05-21.md) reflete esse escopo: a maioria dos itens do checklist universal é N/A por não haver superfície de servidor — e o que se aplica a um site estático (CSP, headers, dependências, secrets) foi verificado e está coberto.

---

## 🎯 Destaques Técnicos & Desafios Superados

**Manter 60fps estáveis numa cena 3D contínua sem vazar memória nem travar a interface durante o scroll.**

Renderização 3D em tempo real no navegador é frágil: um `requestAnimationFrame` não-limpo, um `EffectComposer` esquecido ou uma rotação dependente de frame-rate degradam a experiência em segundos. Três decisões resolveram:

1. **Memory leak prevention** — todo `requestAnimationFrame`, listener e recurso de WebGL tem cleanup explícito no unmount dos componentes 3D (como `EffectComposer` e listeners de redimensionamento). Sem isso, navegar entre seções acumularia render loops fantasma.
2. **Frame-rate independent** — rotações e animações usam `delta time`, não contagem de frames. A cena se comporta igual em um monitor 60Hz e em um 144Hz.
3. **Ponte UI ↔ Canvas via Zustand** — o estado da cena 3D (cor, brilho, densidade de partículas) vive num store Zustand único. A UI React e o canvas WebGL leem da mesma fonte sem re-renders desnecessários — o slider de personalização atualiza o shader sem reconstruir a cena.

---

## 📐 Decisões Arquiteturais (Trade-offs)

- **React Three Fiber em vez de Three.js puro** — R3F traz o modelo declarativo do React pra cena 3D (componentes, hooks, reconciliação). Trade-off: camada de abstração a mais sobre o Three.js; mitigado porque o ganho de manutenibilidade compensa num projeto com várias seções.
- **Shader GLSL customizado em vez de biblioteca de post-processing** — o efeito de brilho do anel Nexus foi escrito direto no shader, sem `@react-three/post-processing`. Trade-off: mais código GLSL pra manter; ganho: bundle menor e controle total do efeito, sem passe de render extra.
- **Zustand em vez de Context API** — estado global leve pra ponte UI↔3D. Context API causaria re-render em cascata a cada ajuste do slider; Zustand atualiza só quem assina aquele pedaço do estado.
- **Lenis para scroll suave** — scroll nativo não sincroniza bem com `GSAP ScrollTrigger`. Lenis dá controle fino do progresso de rolagem, essencial pras animações cinematográficas.

---

## 🔒 Segurança — camadas e status

> *Auditoria 2026-05-21 (modo padrão): site estático bem-feito, **0 críticos**, 3 polimentos menores aplicados. Como não há backend, autenticação nem dados de usuário, a maioria dos 57 itens do checklist universal é N/A por escopo — a tabela abaixo lista o que de fato se aplica a um frontend estático.*

| Camada | Implementação | Status |
|--------|---------------|:------:|
| Headers de segurança | `vercel.json` com CSP, HSTS preload, `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy`, `Permissions-Policy` | ✅ |
| CSP | `default-src 'self'`, `frame-ancestors 'none'`, `object-src 'none'`, `base-uri 'self'`; `'unsafe-inline'` em `script-src` por limitação do Vite (bootstrap inline) | ✅ |
| Dependências | 9 dependências de produção, 0 CVEs conhecidos (`npm audit` limpo) | ✅ |
| Secrets | Nenhum secret no projeto — site estático sem API key; `.env` nunca versionado | ✅ |
| Injeção / XSS | Zero `eval`, zero `dangerouslySetInnerHTML`, zero `innerHTML`; todo texto renderizado via JSX (escapado por padrão) | ✅ |
| Erros | Sem vazamento de stack — não há backend nem `console.log` de dados sensíveis | ✅ |
| Build | `tsc -b` (type-check estrito) bloqueia build com erro de tipo | ✅ |

**Nota de escopo:** itens de backend (autenticação, autorização, rate limit, fail-fast de envs, SQL injection, SSRF) são **N/A** — não há servidor. Isso não é lacuna, é a natureza de um showcase 3D estático. Detalhe completo na [matriz de cobertura da auditoria](docs/AUDIT_REPORT_2026-05-21.md).

---

## ✨ Principais Funcionalidades

- **Cena 3D em tempo real** — canvas WebGL em background fixo, com controle de órbita e zoom; câmera adaptável a mobile e desktop.
- **Shader GLSL customizado** — anel Nexus com shader próprio e uniforms dinâmicos (cor e brilho controlados pela UI).
- **Campo de partículas procedural** — estrelas e partículas geradas em código, sem assets externos.
- **Personalização ao vivo** — sliders de tema de cor, brilho e densidade de partículas atualizam o shader em tempo real.
- **Loading screen cinematográfica** — overlay de introdução com portal animado em Canvas 2D.
- **Scroll suave + animações sincronizadas** — Lenis + GSAP ScrollTrigger para transições que respondem ao progresso da rolagem.
- **Efeitos de áudio** — feedback sonoro de hover/click para reforço tátil.
- **Cursor customizado e micro-interações** — componentes `Magnetic`, `DecodeText`, `CustomCursor` para uma camada extra de polish.

---

## 🛠️ Stack Tecnológico & Arquitetura

### Renderização 3D
- **Three.js** (r184) — engine WebGL
- **@react-three/fiber** (v9) — renderer React declarativo para Three.js
- **@react-three/drei** (v10) — helpers (controles de órbita, etc.)
- **Shader GLSL customizado** — efeito de brilho do anel Nexus

### Frontend
- **React 19 + TypeScript** — base reativa com tipagem estrita
- **Vite 7** — build e dev server
- **TailwindCSS 3.4** — estilização utilitária da interface
- **GSAP 3 + ScrollTrigger** — animações cinematográficas sincronizadas ao scroll
- **Lenis** — scroll suave de alto desempenho
- **Zustand 5** — estado global, ponte entre UI React e canvas 3D

### Deploy
- **Vercel** — hospedagem estática (CDN edge), com headers de segurança via `vercel.json`

---

## 📂 Visão Geral da Estrutura

```text
nexus-portal/
├── index.html               # Entry point — preconnect de fontes, root
├── vercel.json               # Headers de segurança (CSP, HSTS, etc.)
├── src/
│   ├── main.tsx              # Bootstrap React
│   ├── App.tsx               # Composição das seções
│   ├── components/
│   │   ├── three/            # Cena 3D: NexusRing (shader), ParticleField,
│   │   │                     #          StarField, SceneContent
│   │   ├── Navigation.tsx    # Navbar com transição no scroll
│   │   ├── LoadingScreen.tsx # Overlay de introdução (Canvas 2D)
│   │   └── ui-custom/        # Magnetic, DecodeText, CustomCursor,
│   │                         # GlowSlider, ColorSwatch, InfoCard, SpecCard
│   ├── sections/             # Hero, About, Customize, Explore, Specs
│   ├── hooks/                # useAudio, use-mobile
│   └── store/                # useStore — estado global Zustand (UI ↔ 3D)
└── docs/
    └── AUDIT_REPORT_2026-05-21.md   # Auditoria de segurança
```
---

## 👑 Autor

**Jeanderson Silva** 🤓✍️

*Desenvolvedor Full-Stack | Engenheiro Frontend | Arquiteto de Software*

Construído desde a engenharia de shaders GLSL customizados e renderização 3D em tempo real (Three.js + React Three Fiber) até a prevenção de memory leaks em canvas WebGL, passando por scroll cinematográfico com Lenis, animações de alto desempenho com GSAP e arquitetura de estado que conecta UI React ao canvas 3D sem re-renders desnecessários.

Sinta-se à vontade para auditar a lógica do shader, explorar o gerenciamento de estado 3D com Zustand ou testar a experiência imersiva ao vivo!
