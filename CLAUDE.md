# CLAUDE.md

Orientações para o Claude Code neste repositório de anotações de disciplina.

## O que é

Um livro Quarto com as anotações de uma disciplina, publicado em `felipelamarca.com/<repo>/`. O visual e os componentes vêm da extensão `_extensions/felipelmc/course-notes/`, mantida em [felipelmc/Course-Notes-Template](https://github.com/felipelmc/Course-Notes-Template). O `_quarto.yml` do repositório só tem os dados da disciplina e a ordem dos capítulos.

## Comandos

```bash
quarto preview                                              # servidor local com reload
quarto render                                               # renderiza tudo e atualiza _freeze/
python3 _extensions/felipelmc/course-notes/tools/check.py pre        # checagens do CI
python3 _extensions/felipelmc/course-notes/tools/check.py post _book
_extensions/felipelmc/course-notes/tools/nova-aula.sh 4 "Título" 2026-04-01
_extensions/felipelmc/course-notes/tools/pdf.sh trabalhos/tarefa-1  # PDF de um trabalho
quarto update extension felipelmc/Course-Notes-Template     # atualizar o template
```

## Publicação e `_freeze/`

- O CI (`.github/workflows/publish.yml`) **não executa R**. Ele renderiza com os resultados em `_freeze/`, que vai para o git, e publica no GitHub Pages (modo "GitHub Actions").
- Depois de editar qualquer página com blocos `{r}`, rode `quarto render` localmente e faça o commit de `_freeze/` junto. Se não fizer, `check.py pre` falha no CI.
- Pacotes de R usados nas páginas ficam listados no `DESCRIPTION`.
- Nunca faça o commit de `_book/` e nunca use `output-dir: docs`.

## Estrutura e convenções

- Aulas em `aulas/aula-NN.qmd` (dois dígitos). Cabeçalho: `title`, `aula` (número), `date` (opcional), `description`. Toda aula nova precisa entrar em `book.chapters` no `_quarto.yml`, com `text: "NN · Título"`.
- Esqueleto da aula com leituras: `## Leituras` (com `### @chave, cap. N` por texto lido), `## Anotações de aula`, `## Simulação` (opcional). Aula sem leituras: os tópicos ficam direto em `##`, e a simulação vem no fim.
- Citações literais: `> texto [@chave, p. N]`. Uma única bibliografia, `references.bib`, com chaves `sobrenomeANOpalavra`.
- **Nunca use `::: {#refs}`**: num livro, ele junta a bibliografia inteira numa página e esconde as referências das outras. As referências saem sozinhas no fim de cada página.
- Nunca declare `format: html` no `_quarto.yml` nem nas páginas; o formato vem da extensão.
- Trabalhos em `trabalhos/<slug>/index.qmd`, com os campos `tipo`, `date`, `description`, `image`, `metodos`, `dados`, `materiais` e `abstract` (ver `guia/trabalhos.qmd` no template). O PDF entregue fica na pasta e é a versão oficial. A vitrine **não mostra notas**.
- Slides ficam em `trabalhos/<slug>/slides/`, com `_quarto.yml` próprio (`type: default`) e `embed-resources: true`. Renderize com `quarto render trabalhos/<slug>/slides` e faça o commit do `index.html`.
- Simulações em OJS ficam no próprio arquivo da aula, numa seção `## Simulação` no fim (nunca em arquivo incluído: o `_freeze` só enxerga o texto do arquivo da aula); o texto que as apresenta, se escrito com IA, vai em `::: {.nota-ia collapse="false" ...}`. Importe de `/_extensions/felipelmc/course-notes/ojs/notes.js`, use `rng(semente)` e `palette(scheme())`, envolva em `::: cn-sim` dentro de `::: panel-tabset` com as abas "Interativo" e "Em R" (esta com `#| eval: false`).
- Figuras em R: `source(here::here("_extensions/felipelmc/course-notes/r/notes.R"))` e `theme_notes()`. Caminhos de dados sempre com `here::here()`, nunca absolutos.

## Matemática

- Linha em branco antes e depois de `$$`; `aligned` para alinhar.
- `^\top` para transposta, `\mathbb{E}`, `\mathbb{V}`, `\text{Cov}`, `\perp\!\!\!\perp`.
- Ambientes: `::: {#def-...}`, `::: {#thm-...}`, `::: prova` (recolhível).
- Resultado central: `$\boxed{...}$`.
- Opções de bloco em `#| chave: valor`, nunca `{r, eval=F}`.

## Política de conteúdo

- **Citações são literais.** Não corrija nada dentro de `>`, a não ser trocar `(p. N)` por `[@chave, p. N]` ou consertar LaTeX quebrado. Citação em outra língua fica na língua original.
- **Os comentários e as anotações de aula são do autor.** Polimento leve é permitido (ortografia, concordância, palavra faltando, travessão, títulos em *sentence case*). Não mude afirmações, números, ordem dos argumentos nem a língua, e não apague coloquialismos.
- **Texto escrito com IA vai sempre em `::: nota-ia`** (use `ferramenta="NotebookLM"` etc. quando não for o Claude). Nunca misture explicação gerada com a voz do autor fora dessa caixa.
- Erros de conteúdo (uma derivação errada, uma afirmação duvidosa) são apontados ao autor, e não corrigidos por conta própria.
- Nunca invente citação, página ou referência.

## Estilo de escrita do autor

- Sem negrito no corpo do texto (só na primeira ocorrência de um termo definido) e sem travessão; use vírgula, dois-pontos ou parênteses.
- Dois-pontos e ponto e vírgula em enumerações são bem-vindos.
- Conectivos dele: "De fato", "Note que", "Em particular", "por sua vez", "isto é", "sobretudo", "Por fim". Nunca: "Com efeito", "Nesse sentido", "Dito isso", "Cabe destacar", "É importante notar".
- Evite marcas de IA: crucial, inovador, abrangente, robusto (fora do sentido técnico), sinergia, panorama, cenário (metafórico), ademais, notavelmente, potencializar.
- Números com vírgula decimal e `%`. Títulos em *sentence case*. Termos técnicos em inglês em itálico.
- Comentários em scripts de R e Python são escritos sem acentuação, de propósito.

## Esta disciplina

Lego II: Modelos de Regressão Linear e Extensões (IESP-UERJ, 2025.2), com Rogério J. Barbosa. As aulas estão agrupadas por tema em partes do `_quarto.yml`; a primeira parte é o nivelamento em matemática (`aulas/nivelamento.qmd`, sem `aula:` e com `eyebrow: "Nivelamento"`). Não há datas das aulas.

- As aulas 4 e 5 ficam numa página só, `aulas/aula-04.qmd`, com `aula: "4–5"` (texto, impresso como está) e "04–05 · …" na barra lateral.
- As anotações manuscritas estão em PDF em `aulas/pdf-aulas/` (aulas 1, 2 e 6) e `aulas/pdf-aulas-nivelamento/`; as soluções de exercícios do Moore e Siegel, em `aulas/pdf-exercicios/moore2013mathematics_exercises_chapter_N.pdf`. Nos links, espaços viram `%20`.
- A aula 01 tem `execute: eval: false` (baixa a PNAD com `PNADcIBGE`): os blocos aparecem, mas não rodam.
- A aula 07 lê `data/data.csv` (dados de candidatos), que fica fora do git (`data` está no `.gitignore`). Os blocos que dependem dele têm `#| eval: false`, e o resultado da chamada foi copiado da versão publicada anterior.
- Os trabalhos leem os dados com caminhos relativos à própria pasta (`dados/...`), que funcionam no livro e no `tools/pdf.sh` (que renderiza uma cópia da pasta fora do projeto). A lista 2 lê o `.sav` do BH Area Survey com `foreign::read.spss`; esses dados estão no git duas vezes (zip e pasta).
- As notas de leitura (resumos e citações do Wooldridge, de Moore e Siegel, de King) estão em inglês, e as anotações de aula em português. Mantenha cada trecho na língua em que foi escrito.
