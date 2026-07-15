# olimpiadas-applheta

Repositório de **conteúdo** do Applheta. O app lê estes arquivos diretamente do GitHub
(via `raw.githubusercontent.com`), então **editar um JSON aqui atualiza o app**
sem precisar publicar uma nova versão.

## Estrutura

```
├── information.json          # Calendário de olimpíadas (tela Calendário)
├── links.json                # Links de provas/simulados (tela Hub)
├── cortes.json               # Notas de corte por olimpíada/nível (tela Hub)
└── cursos/
    ├── index.json            # Lista de cursos disponíveis
    └── <curso>/              # matematica | informatica | robotica | quimica | economia
        ├── curso.json        # Módulos → tópicos → aulas do curso
        └── materiais/        # PDFs de material e exercícios
            └── <topico-id>/
                ├── aula-01.pdf
                └── exercicios-01.pdf
```

## Como adicionar/editar uma aula

1. Abra `cursos/<curso>/curso.json`.
2. Encontre o tópico (pelo `id` ou `titulo`) e adicione um objeto ao array `aulas`:

```json
{
  "numero": "05",
  "titulo": "Nome da Aula",
  "subtitulo": "Subtítulo curto",
  "descricao": "Uma ou duas frases sobre o que a aula cobre.",
  "dificuldade": 3,
  "objetivo": "O que o aluno saberá fazer ao final. (opcional)",
  "prerequisitos": ["Tópico anterior"],
  "resumo": "Resumo conceitual exibido após o vídeo. (opcional)",
  "videoAula": "https://www.youtube.com/watch?v=XXXXXXXX",
  "material": "materiais/<topico-id>/aula-05.pdf",
  "exercicios": "materiais/<topico-id>/exercicios-05.pdf",
  "cor": "#2196F3",
  "icone": "star"
}
```

3. Suba o PDF correspondente em `cursos/<curso>/materiais/<topico-id>/`.
4. Commit + push na branch `main`. Pronto — o app passa a exibir a aula.

### Campos da aula

| Campo | Obrigatório | Descrição |
|---|---|---|
| `numero` | sim | Ordem de exibição, string com dois dígitos (`"01"`) |
| `titulo` | sim | Nome da aula (usado também como ID de progresso — evite renomear) |
| `subtitulo` | sim | Linha curta abaixo do título |
| `descricao` | sim | 1–2 frases sobre o conteúdo |
| `dificuldade` | sim | 1 (Base) a 5 (Internacional) |
| `objetivo` | não | O que o aluno deve dominar ao final |
| `prerequisitos` | não | Lista de tópicos necessários antes da aula |
| `resumo` | não | Resumo conceitual exibido após o vídeo |
| `videoAula` | sim | Link do YouTube |
| `material` | sim | Caminho relativo ao curso (`materiais/...`) ou URL completa |
| `exercicios` | sim | Idem `material` |
| `cor` | sim | Hex `#RRGGBB` (ou `#AARRGGBB` com transparência) |
| `icone` | não | Nome do ícone Material em snake_case (ex.: `star`, `functions`) |

### Campos do tópico

| Campo | Descrição |
|---|---|
| `id` | Slug único (kebab-case, sem acento) — nomeia a pasta de materiais |
| `titulo` | Nome exibido no card e na tela de aulas (também é o ID de progresso) |
| `subtitulo` | Descrição curta no card |
| `categoria` | Etiqueta pequena do card (ex.: "Álgebra", "Projeto") |
| `cor` / `icone` | Estilo do card |
| `destaque` | `true` faz o card ocupar a largura toda (opcional) |
| `aulas` | Array de aulas (pode ser `[]` enquanto o conteúdo não existe) |

### Ícones

Use o nome do ícone do Material Icons em snake_case (o mesmo nome do Flutter:
`Icons.nome_do_icone`). Se o app não conhecer o nome, ele usa um ícone de
livro como padrão — nada quebra.

## Como adicionar um curso novo

1. Crie `cursos/<novo-curso>/curso.json` seguindo o schema acima
   (`id`, `materia`, `titulo`, `subtitulo`, `modulos`).
2. Registre o curso em `cursos/index.json` (o campo `materia` deve bater com o
   nome da matéria usado no app, ex.: `"Física"`).
3. Crie a pasta `materiais/` com os PDFs.

## Arquivos legados (raiz)

- `information.json` — eventos do calendário: `[{"data": "AAAA-MM-DD", "nome": "...", "fase": "..."}]`
- `links.json` — provas e simulados do Hub: `[{"nome": "...", "url": "..."}]`
- `cortes.json` — notas de corte: `{"OLIMPÍADA": {"Nível": [ouro, prata, bronze]}}`

## Regras de ouro

- **JSON válido sempre** — um erro de vírgula derruba o curso inteiro no app.
  Valide antes de commitar (ex.: `python -m json.tool cursos/matematica/curso.json`).
- **Não renomeie `titulo` de tópicos/aulas já publicados** sem necessidade:
  o progresso dos alunos é salvo por título.
- PDFs: mantenha o padrão `aula-NN.pdf` / `exercicios-NN.pdf` dentro de
  `materiais/<topico-id>/`.
