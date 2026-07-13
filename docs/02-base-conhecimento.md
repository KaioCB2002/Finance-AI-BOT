# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Realizei os ajustes no perfil de investidor para se assemelhar melhor a minha situação e adicionei diversos dados ao histórico de interações para deixar mais robusto.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

import pandas as pd
import json


def carregar_contexto():
    # Leitura dos arquivos
    historico = pd.read_csv("dados/historico_atendimento.csv")
    transacoes = pd.read_csv("dados/transacoes.csv")

    with open("dados/perfil_investidor.json", encoding="utf-8") as f:
        perfil = json.load(f)

    with open("dados/produtos_financeiros.json", encoding="utf-8") as f:
        produtos = json.load(f)

    contexto = f"""
# Base de Conhecimento

## Perfil do Investidor
{json.dumps(perfil, indent=2, ensure_ascii=False)}

## Produtos Financeiros
{json.dumps(produtos, indent=2, ensure_ascii=False)}

## Histórico de Atendimento
{historico.to_markdown(index=False)}

## Histórico de Transações
{transacoes.to_markdown(index=False)}

Utilize exclusivamente essas informações para responder as perguntas.
Caso a informação não exista na base de dados, informe que ela não foi encontrada.
"""

    return contexto

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

não

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
