# 🔮 NEXUS PORTAL — Experiência 3D Interativa

> Experiência web cinematográfica com **cena 3D em tempo real**, shaders customizados, animações GSAP e personalização visual — construída com React, Three.js e TypeScript.

[![Acessar Experiência](https://img.shields.io/badge/🌐_ACESSAR-nexus--portal.vercel.app-8B5CF6?style=for-the-badge)](https://nexus-portal-one.vercel.app)
![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-r170-000000?style=for-the-badge&logo=three.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![GSAP](https://img.shields.io/badge/GSAP-3-88CE02?style=for-the-badge&logo=greensock&logoColor=white)

🟢 **LIVE DEMO:** [Acesse o NEXUS PORTAL Ao Vivo Aqui](https://nexus-portal-one.vercel.app)

---

## 🌌 Sobre o Projeto

O **NEXUS PORTAL** é uma experiência web imersiva que combina computação gráfica 3D em tempo real com design de interface premium. O artefato Nexus — um anel tridimensional com shader customizado — gira em um campo de partículas e estrelas, enquanto o usuário navega por seções com scroll suave e animações cinematográficas.

### ✨ Features

- 🎬 **Cena 3D em Tempo Real** — Canvas WebGL com controle de órbita e zoom
- 💎 **Shader Customizado** — GLSL no anel Nexus com uniforms dinâmicos
- ✨ **Campo de Partículas** — Sistema de estrelas e partículas procedurais
- 🎨 **Personalização ao Vivo** — Tema de cor, brilho e densidade de partículas
- 🔊 **Efeitos de Áudio** — Sons de hover/click para feedback tátil
- 🌊 **Scroll Suave** — Navegação com Lenis + GSAP ScrollTrigger
- ⚡ **Loading Cinematográfico** — Tela de carregamento animada premium
- 📱 **Responsivo** — Câmera 3D adaptável para mobile e desktop

---

## 🛠️ Stack Tecnológico

| Tech | Uso |
|---|---|
| **React 19 + TypeScript** | Base reativa com tipagem estrita |
| **Three.js + R3F + Drei** | Renderização 3D, shaders e helpers |
| **GSAP + ScrollTrigger** | Animações cinematográficas sincronizadas |
| **Zustand** | Estado global (câmera, tema, partículas) |
| **Tailwind CSS** | Estilização da interface UI |
| **Lenis** | Scroll suave de alto desempenho |
| **Vite 7** | Build e dev server otimizado |

---

## 📁 Estrutura

```
src/
├── components/
│   ├── three/          → Cena 3D, NexusRing, partículas
│   ├── ui/             → Componentes base (Radix)
│   └── ui-custom/      → Slider de personalização, HUD
├── sections/           → Seções da página (Hero, Features, etc)
├── hooks/              → Hooks customizados (áudio, scroll)
└── store/              → Estado global Zustand
```

---

## 🚀 Como Rodar

```bash
git clone https://github.com/silvajeanderson165-creator/nexus-portal.git
cd nexus-portal
npm install
npm run dev
```

O app fica disponível em `http://localhost:3000`

---

## 🎯 Destaques Técnicos

- **Memory Leak Prevention** — Cleanup de `requestAnimationFrame` e `EffectComposer`
- **Frame-rate Independent** — Rotações com `delta time` para consistência entre dispositivos
- **Performance** — 60fps estável com otimização de re-renders e shader uniforms

---

## 👑 Autor

**Jeanderson Silva** 🤓✍️

*Desenvolvedor Full-Stack | Engenheiro Frontend | Arquiteto de Software*

Construído desde a engenharia de shaders GLSL customizados e renderização 3D em tempo real (Three.js + R3F) até a prevenção de memory leaks em canvas WebGL, passando por scroll cinematográfico com Lenis e animações de alto desempenho com GSAP.

Sinta-se à vontade para auditar a lógica do shader, explorar o gerenciamento de estado 3D com Zustand ou testar a experiência imersiva ao vivo!
