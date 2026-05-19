# Coleção de Vinil — CLAUDE.md

## Visão geral

Site de gerenciamento de coleção de discos de vinil do Fabio.  
Arquivo único: `index.html` com CSS e JavaScript vanilla embutidos.  
Hospedagem: GitHub Pages via `github.com/fabiofoltram/colecao-vinil-setup`.  
Deploy: qualquer push para `main` atualiza o site automaticamente.

## Como publicar alterações

```
git add index.html
git commit -m "descrição da mudança"
git push
```

O GitHub Pages reflete em ~1 minuto.

## Estrutura do arquivo

`index.html` tem três partes inline:
1. `<style>` — todo o CSS (variáveis de tema, layouts, componentes)
2. `<body>` — HTML estático (nav, pages, overlays)
3. `<script>` — toda a lógica JS, incluindo os dados da coleção

## Dados da coleção

Os discos ficam no array `records` no `<script>` (linha ~536):

```js
let records = [
  {
    artist: "Nome do Artista",
    title:  "Título do Álbum",
    label:  "Gravadora",
    format: "LP, Album",          // formato Discogs
    rating: 5,                    // 0–5 (0 = não avaliado)
    year:   1984,                 // ano de lançamento
    folder: "Nacionais antigos",  // ver pastas abaixo
    media:  "Near Mint (NM or M-)", // condição do disco
    sleeve: "Very Good Plus (VG+)", // condição da capa
    price:  100,                  // preço pago em BRL
    loc:    "Gil",                // seção física na estante (ver abaixo)
    rid:    1234567,              // ID do release no Discogs (null se não tiver)
    notes:  "texto livre",        // opcional
    cover:  "data:image/..."      // base64, opcional
  }
]
```

### Pastas (`folder`)
- `Nacionais antigos` / `Nacionais Novos`
- `Internacionais antigos` / `Internacionais novos`
- `Autografados`
- `Uncategorized`

### Seções físicas (`loc`) — categorias na estante
- `BR Rock` — Rock brasileiro
- `Eletro` — Eletrônico
- `Gil` — Seção dedicada ao Gilberto Gil
- `Hip Hop / Afrobeats`
- `Int Rock` — Rock internacional
- (pode haver outras — ver registros existentes)

### Condições de mídia/capa (padrão Discogs)
`Mint (M)` → `Near Mint (NM or M-)` → `Very Good Plus (VG+)` → `Very Good (VG)` → `Good Plus (G+)` → `Good (G)` → `Fair (F)` → `Poor (P)`  
Valores especiais: `No Cover` (sem capa)

## Páginas do site

| ID da page | Nav button | Descrição |
|---|---|---|
| `resumo` | Resumo | Dashboard com KPIs, gráficos, stats do setup |
| `collection` | Coleção | Grid de cards com filtro/busca |
| `setup` | Setup | Equipamentos e chain de áudio |
| `planning` | Planejamento | Recomendações e notas pessoais |
| `incoming` | 📦 A Caminho | Discos encomendados/a chegar |
| `admin` | + Disco | Formulário para adicionar disco |

## Setup de áudio (hardcoded no HTML)

| Componente | Modelo |
|---|---|
| Toca-discos | Pro-Ject Debut Carbon Evo |
| Agulha | Ortofon 2M Blue |
| Phono Preamp | Fosi Audio X5 |
| Interface | Steinberg UR44C |
| DAC / Amp | FiiO K11 |
| Monitores | Yamaha HS5 (par) |

## Temas

Três temas via CSS custom properties em `:root` / `body.theme-*`:
- `dark` (padrão) — fundo `#0a0a0a`, acento dourado `#c9a84c`
- `light` — fundo claro
- `classic` — marrom escuro estilo vintage

## Autenticação

Overlay de senha na abertura. Modo visitante (somente leitura) disponível.  
Botões admin (+ Disco, Export, Import) ficam bloqueados para visitantes.

## Tarefas comuns

**Adicionar disco:** inserir novo objeto no array `records` seguindo o schema acima.

**Atualizar info do setup:** buscar o bloco `.spec-item` correspondente na `<div class="dash-card">` da página `resumo` (linha ~234) e editar o `spec-val`.

**Mudar estilo:** editar variáveis CSS em `:root` ou `body.theme-*` no `<style>`.

**Adicionar gráfico/stat no Resumo:** editar as funções JS `buildKPIs()`, `buildCharts()` e adicionar o container HTML na `#page-resumo`.
