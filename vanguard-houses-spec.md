# Vanguard Houses — Arena de Quadras
## Spec de construção · Claude Code

---

## Visão geral

Site de marcação de jogos com **duas dobras únicas**:

1. **Hero** — headline + botão "Reservar horário"
2. **Modal de Reserva** — abre ao clicar no botão; fluxo em etapas: modalidade → data/horário → dados → confirmação

Stack sugerida: **HTML + CSS + JS vanilla** (arquivo único) ou **React + Tailwind** se preferir componentes.

---

## Paleta de cores

```css
--azul-escuro:   #0B2447;   /* fundo, headers */
--azul-medio:    #19376D;   /* cards, nav */
--azul-claro:    #2E6BD6;   /* botões primários, destaques */
--azul-brilho:   #4A90E2;   /* hover, bordas ativas */
--branco:        #FFFFFF;
--branco-suave:  #F0F4FA;   /* fundo de inputs */
--cinza-borda:   rgba(255,255,255,0.12);
--texto-muted:   rgba(255,255,255,0.50);
```

---

## Tipografia

```
Display / Headlines : Bebas Neue (Google Fonts)
Body / UI           : DM Sans (Google Fonts)
Weights usados      : 300, 400, 500, 700
```

---

## DOBRA 1 — Hero

### Layout

```
┌─────────────────────────────────────────────────────────┐
│  HEADER (fixo, translúcido)                             │
│  Logo [VH]  VANGUARD HOUSES         [Reservar →]        │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [IMAGEM DE FUNDO — área verde/pergolado]               │
│  overlay azul escuro 0.72 opacity                       │
│  + ruído/grain sutil                                     │
│                                                         │
│         VANGUARD HOUSES          ← eyebrow pequeno      │
│                                                         │
│    RESERVE                                              │
│    SUA QUADRA           ← Bebas Neue, ~120px            │
│    AGORA                                                │
│                                                         │
│    Futevôlei · Vôlei de Praia · Beach Tennis            │
│    ── subtítulo muted ──                                │
│                                                         │
│    [ RESERVAR HORÁRIO → ]   ← botão azul-claro          │
│                                                         │
│    ┌────────┐ ┌────────┐ ┌────────┐                     │
│    │  3     │ │  8     │ │ 7 dias │                     │
│    │ modal. │ │quadras │ │/semana │  ← stats bar        │
│    └────────┘ └────────┘ └────────┘                     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Imagem de fundo

- Arquivo: `background-arena.jpg`  
  *(usar a foto do pergolado / quadra verde — imagem fornecida pelo cliente)*
- CSS: `object-fit: cover; object-position: center`
- Overlay: `linear-gradient(160deg, rgba(11,36,71,0.80) 40%, rgba(25,55,109,0.65) 100%)`
- A imagem aparece como **marca d'água clara** — estrutura visível, mas o texto em branco tem total legibilidade
- Efeito opcional: `filter: brightness(0.55) saturate(0.7)` na tag `<img>` antes do overlay

### Header

```
height: 70px
background: rgba(11,36,71,0.85) + backdrop-filter: blur(14px)
border-bottom: 1px solid rgba(255,255,255,0.08)
padding: 0 5vw
justify-content: space-between
```

- **Logo**: `[VH]` em caixa 36×36px borda branca + texto "VANGUARD / HOUSES SPORTS"
- **Botão header**: igual ao CTA principal, menor — `padding: 9px 22px`

### Headline

```
eyebrow : font-size 11px · letter-spacing 5px · color azul-brilho · uppercase
título  : Bebas Neue · clamp(70px, 10vw, 130px) · line-height 0.9 · branco
subtítulo: DM Sans 300 · 16px · texto-muted · max-width 440px
```

### Botão CTA principal

```css
background    : #2E6BD6
color         : #FFFFFF
padding       : 16px 40px
border-radius : 4px
font-weight   : 700
font-size     : 12px
letter-spacing: 2.5px
text-transform: uppercase
transition    : background 0.25s, transform 0.25s, box-shadow 0.25s

:hover {
  background  : #4A90E2
  transform   : translateY(-2px)
  box-shadow  : 0 12px 36px rgba(46,107,214,0.45)
}
```

**Ao clicar → abre o Modal de Reserva (etapa 1)**

### Stats bar

Três blocos lado a lado no rodapé do hero:

| Número | Label |
|--------|-------|
| 3 | Modalidades |
| 8 | Quadras |
| 7 dias | Por semana |

```css
fundo   : rgba(255,255,255,0.06)
borda   : 1px solid rgba(255,255,255,0.10)
número  : Bebas Neue 48px cor azul-brilho (#4A90E2)
label   : DM Sans 10px letter-spacing 2px texto-muted
```

---

## DOBRA 2 — Modal de Reserva

O modal cobre a tela inteira com overlay escuro. Internamente tem **4 etapas** em sequência (wizard).

### Estrutura do overlay

```css
position      : fixed
inset         : 0
background    : rgba(7,18,36,0.92)
backdrop-filter: blur(10px)
z-index       : 500
display       : flex
align-items   : center
justify-content: center
padding       : 20px
```

### Caixa do modal

```css
background    : #0B2447
border        : 1px solid rgba(255,255,255,0.10)
border-radius : 8px
width         : 100%
max-width     : 560px
padding       : 48px
position      : relative
animation     : slideUp 0.35s cubic-bezier(0.34,1.56,0.64,1)
```

Botão fechar `×` no canto superior direito.

### Barra de progresso (topo do modal)

```
[●────────────] Passo 1 de 4
```

```css
barra total  : width 100%, height 3px, background rgba(255,255,255,0.10), border-radius 2px
barra ativa  : background #2E6BD6, transition width 0.4s ease
dots         : 4 círculos 8px, azul-claro quando ativo, cinza quando futuro
```

---

### ETAPA 1 — Escolha a modalidade

**Título:** `QUAL ESPORTE?` · Bebas Neue 32px

**Três cards lado a lado:**

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│      ⚽       │  │      🏐      │  │      🎾      │
│              │  │              │  │              │
│  FUTEVÔLEI   │  │    VÔLEI     │  │    BEACH     │
│              │  │   DE PRAIA   │  │    TENNIS    │
│  4 quadras   │  │  2 quadras   │  │  2 quadras   │
└──────────────┘  └──────────────┘  └──────────────┘
```

**Card base:**
```css
background    : rgba(255,255,255,0.04)
border        : 1.5px solid rgba(255,255,255,0.08)
border-radius : 6px
padding       : 28px 20px
text-align    : center
cursor        : pointer
transition    : all 0.2s

:hover {
  border-color: #2E6BD6
  background  : rgba(46,107,214,0.10)
}

.selecionado {
  border-color: #4A90E2
  background  : rgba(46,107,214,0.18)
  box-shadow  : 0 0 0 3px rgba(46,107,214,0.25)
}
```

**Botão avançar** (aparece após seleção):
```
[ PRÓXIMO → ]  → mesma estilização do CTA principal
```

---

### ETAPA 2 — Escolha a data

**Título:** `QUANDO VOCÊ JOGA?` · Bebas Neue 32px

**Carrossel de datas (próximos 14 dias):**

```
← [  DOM  ] [  SEG  ] [  TER  ] [  QUA  ] [  QUI  ] →
   [ 01   ] [  02   ] [  03   ] [  04   ] [  05   ]
```

```css
cada pill  : 56px × 70px
fundo      : rgba(255,255,255,0.04)
borda      : 1px solid rgba(255,255,255,0.08)
border-r   : 4px
scroll     : overflow-x: auto; scrollbar: none

.ativo {
  background  : #2E6BD6
  border-color: #2E6BD6
  color       : white
}

dia nome   : 9px, letter-spacing 1.5px, uppercase, texto-muted
dia número : Bebas Neue 22px, branco
```

**Grade de horários** (abaixo das datas):

```
07:00  08:00  09:00  10:00  11:00  12:00
13:00  14:00  15:00  16:00  17:00  18:00
19:00  20:00  21:00  22:00
```

```css
display      : grid
grid-template-columns: repeat(4, 1fr)   /* mobile: repeat(3,1fr) */
gap          : 8px

cada slot:
  padding    : 10px
  font-size  : 13px
  fundo      : rgba(255,255,255,0.04)
  borda      : 1px solid rgba(255,255,255,0.08)
  border-r   : 4px
  text-align : center
  cursor     : pointer

.ocupado {
  opacity       : 0.22
  cursor        : not-allowed
  text-decoration: line-through
}

.selecionado {
  background   : #2E6BD6
  border-color : #2E6BD6
  color        : white
  font-weight  : 700
}
```

Horários ocupados simulados: índices 2, 5, 9 (ou definir via JS array `OCUPADOS`).

**Botões:** `[ ← VOLTAR ]` e `[ PRÓXIMO → ]`

---

### ETAPA 3 — Seus dados

**Título:** `SEUS DADOS` · Bebas Neue 32px

**Campos:**

| Campo | Tipo | Placeholder | Required |
|-------|------|-------------|----------|
| Nome completo | text | Seu nome | ✓ |
| WhatsApp | tel | (85) 9 0000-0000 | ✓ |
| Nº de jogadores | select | Selecionar | — |
| Observações | textarea | Alguma preferência de quadra... | — |

**Select de jogadores:** 2, 4, 6, 8, 10+

**Estilo dos inputs:**
```css
background    : rgba(255,255,255,0.05)
border        : 1px solid rgba(255,255,255,0.10)
border-radius : 4px
padding       : 13px 16px
color         : white
font-family   : DM Sans
font-size     : 14px
width         : 100%

:focus {
  border-color : #2E6BD6
  outline      : none
  background   : rgba(46,107,214,0.06)
}

::placeholder { color: rgba(255,255,255,0.22) }
```

**Label acima de cada input:**
```css
font-size     : 10px
letter-spacing: 2px
text-transform: uppercase
color         : rgba(255,255,255,0.45)
margin-bottom : 6px
```

**Botões:** `[ ← VOLTAR ]` e `[ CONFIRMAR RESERVA ]`

---

### ETAPA 4 — Confirmação

**Ícone:** círculo 64px fundo `rgba(46,107,214,0.15)` borda `#2E6BD6` com `✓` 28px branco

**Texto:**
```
RESERVA ENVIADA!        ← Bebas Neue 38px branco
```
```
[Nome], sua reserva para [Modalidade]
no dia [DD] às [HH:00] foi recebida!
Confirmaremos pelo WhatsApp em até 15 min.
```
```css
font-size : 14px
color     : rgba(255,255,255,0.55)
line-height: 1.7
```

**Botão:**
```
[ FECHAR ]   → fecha o modal, reseta o form
```

---

## Animações

```css
/* Entrada do modal */
@keyframes slideUp {
  from { opacity: 0; transform: scale(0.88) translateY(24px); }
  to   { opacity: 1; transform: scale(1)    translateY(0);    }
}

/* Scroll line hero */
@keyframes scrollLine {
  0%   { opacity: 0; transform: scaleY(0); transform-origin: top; }
  50%  { opacity: 1; transform: scaleY(1); }
  100% { opacity: 0; transform: scaleY(0); transform-origin: bottom; }
}

/* Pulse dot badge */
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1);   }
  50%      { opacity: 0.4; transform: scale(1.5); }
}

/* Reveal on scroll */
.reveal {
  opacity   : 0;
  transform : translateY(28px);
  transition: opacity 0.65s ease, transform 0.65s ease;
}
.reveal.visible {
  opacity   : 1;
  transform : translateY(0);
}
```

---

## Responsivo

| Breakpoint | Ajuste |
|------------|--------|
| `< 768px` | Cards de modalidade empilhados (1 coluna) |
| `< 768px` | Horários: 3 colunas |
| `< 768px` | Hero padding reduzido, stats bar oculta |
| `< 768px` | Modal padding: 28px 20px |
| `< 480px` | Headline: clamp(54px, 14vw, 80px) |

---

## Estrutura de arquivos sugerida

```
vanguard-houses/
├── index.html
├── style.css
├── main.js
└── assets/
    └── background-arena.jpg   ← foto do pergolado (fornecida pelo cliente)
```

Ou **arquivo único** `index.html` com `<style>` e `<script>` inline.

---

## Lógica JS — resumo

```js
// Estado global
const estado = {
  etapa      : 1,         // 1–4
  modalidade : null,      // 'futevolei' | 'volei' | 'beach'
  data       : null,      // Date object
  horario    : null,      // '09:00'
  nome       : '',
  whatsapp   : ''
};

// Funções principais
abrirModal()        // exibe overlay, reseta estado para etapa 1
fecharModal()       // esconde overlay
irParaEtapa(n)      // renderiza etapa n, atualiza barra de progresso
buildDates()        // gera pills dos próximos 14 dias
buildHorarios()     // gera grade, marca OCUPADOS[]
confirmarReserva()  // valida, avança para etapa 4
```

---

## Checklist de entrega

- [ ] Header fixo translúcido com logo VH
- [ ] Hero com imagem de fundo como marca d'água + overlay azul
- [ ] Headline em Bebas Neue + subtítulo + botão CTA
- [ ] Stats bar (3 quadros)
- [ ] Modal com 4 etapas e barra de progresso
- [ ] Etapa 1: 3 cards de modalidade clicáveis
- [ ] Etapa 2: carrossel de datas + grade de horários com ocupados bloqueados
- [ ] Etapa 3: formulário com validação básica (campos required)
- [ ] Etapa 4: tela de confirmação com dados preenchidos
- [ ] Animação de entrada do modal
- [ ] Reveal on scroll nos elementos do hero
- [ ] Responsivo mobile
- [ ] Cores: azul + branco (sem dourado)
