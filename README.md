# Google Scholar Scraper

Ferramenta simples para pesquisa exploratória de publicações acadêmicas a
partir de palavras-chave. Ela usa a biblioteca
[`scholarly`](https://pypi.org/project/scholarly/) para consultar resultados
públicos do Google Scholar e exportar seus metadados.

## O que a ferramenta faz

Para cada consulta, o programa:

1. envia uma busca por palavras-chave ao Google Scholar;
2. lê até `--limit` resultados;
3. extrai título, autores, ano, periódico/evento, resumo e URLs;
4. reutiliza resultados já armazenados em um cache SQLite;
5. remove duplicatas por URL e, quando não há URL, por título;
6. grava os resultados em `data/publicacoes.csv` e `data/publicacoes.json`.

O cache reduz requisições repetidas. Entre consultas novas, o programa espera
um intervalo aleatório de 15 a 30 segundos por padrão.

## O que ela não faz

Este projeto **não compara automaticamente os trabalhos**. Em particular, ele
não:

- calcula similaridade entre títulos ou resumos;
- classifica relevância, qualidade ou contribuição científica;
- identifica qual artigo é melhor ou mais próximo do projeto;
- lê o texto integral, reproduz experimentos ou valida resultados;
- gera revisão sistemática, citações ou conclusões acadêmicas.

Portanto, a comparação é uma etapa posterior e humana. O CSV/JSON fornece
uma lista inicial de candidatos e alguns metadados para triagem. A análise
comparativa deve ser feita lendo as fontes e registrando, para cada trabalho,
objetivo, público-alvo, dados de entrada, método, saída, limitações e relação
com o problema estudado.

## Organização

```text
GoogleScholarScraper/
├── scraper.py          # implementação e interface de linha de comando
├── test_scraper.py     # testes unitários offline
├── queries.json        # consultas de exemplo, editáveis
├── requirements.txt    # dependências de execução
├── .gitignore          # ignora ambiente virtual e dados gerados
└── README.md
```

## Instalação no Windows

Na pasta deste projeto:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Se a política do PowerShell impedir a ativação, use diretamente o interpretador
do ambiente:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

## Uso

Edite `queries.json` e execute uma coleta pequena:

```powershell
python scraper.py --queries queries.json --limit 5
```

Para salvar em outra pasta:

```powershell
python scraper.py --queries queries.json --limit 3 --output data\rodada-01
```

Parâmetros importantes:

- `--limit 3`: máximo de resultados por consulta;
- `--delay-min 30 --delay-max 60`: aumenta a espera entre consultas;
- `--cache data\cache.sqlite3`: define o arquivo de cache;
- `--no-cache`: ignora o cache existente e faz novas consultas.

## Testes

Os testes não acessam a internet nem exigem a instalação de `scholarly`:

```powershell
python -m unittest discover -v
```

## Uso responsável e limitações

Use a ferramenta somente para pesquisa acadêmica responsável. Se aparecer
CAPTCHA, erro `429` ou bloqueio, interrompa a execução e tente novamente em
outro momento. Não use proxy, rotação de IP, múltiplas contas ou automação de
desafios. Verifique as condições de uso do Google Scholar e as regras da sua
instituição.

O `scholarly` depende da interface pública do Google Scholar, que pode mudar
sem aviso. Os metadados devem ser conferidos na publicação original, no DOI
ou no portal da biblioteca antes de serem usados como evidência. A ferramenta
é um apoio à descoberta de literatura, não substitui bases institucionais nem
a leitura crítica das fontes.
