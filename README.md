# carregamento_miguel

Página com os preços de carregamento nos postos públicos da rede MOBI.E, agrupados em perfis de preço,
com o preço final para cada fornecedor de energia (Enable, MEO Energia, Atlante).

Publicada pelo GitHub Pages a partir do `index.html` na raiz do ramo `main`.

## Como funciona

Uma GitHub Action (`.github/workflows/site.yml`) corre:

- **todos os dias** (05:17 UTC): descarrega as tarifas e as coordenadas da MOBI.E, valida-as e, se algo mudou,
  regenera o `index.html` e faz commit;
- **em cada push** a `config/`, `scripts/`, `template/` ou `data/`: só regenera a página com os dados atuais;
- **manualmente**: separador *Actions* → *Atualizar dados e gerar a página* → *Run workflow*.

Se um download falhar ou vier incompleto (menos de 80% dos postos da versão anterior), os dados anteriores
mantêm-se, a página é gerada na mesma e a execução fica marcada a vermelho.

## O que alterar e onde

| Para… | Editar |
|---|---|
| Mudar preços de energia, TAR ou horários de vazio, ou acrescentar um fornecedor | `config/fornecedores.yml` |
| Mudar ou acrescentar cenários (energia, tempo, potências) | `config/site.yml` → `cenarios` |
| Mudar regras (IVA, margem dos perfis, exclusões, municípios excluídos) | `config/site.yml` → `regras` |
| Usar um ficheiro de tarifas ou coordenadas obtido à mão | substituir `data/tarifas.csv` ou `data/coords.csv` |
| Mudar o aspeto da página | `template/index.html` |

Basta fazer commit da alteração: a Action reconstrói e publica a página.

## Correr localmente

```bash
pip install -r requirements.txt
python scripts/fetch_tarifas.py   # opcional: descarrega tarifas
python scripts/fetch_coords.py    # opcional: descarrega coordenadas (~170 MB)
python scripts/build.py           # gera index.html
```

## Ficheiros

- `data/tarifas.csv`: ficheiro "Descarregar Tarifas" da MOBI.E.
- `data/coords.csv`: coordenadas extraídas do feed DATEX II do Ponto de Acesso Nacional (`site_id`, identificadores, `lat`, `lon`).
- `data/meta.json`: datas e origem dos dados.
