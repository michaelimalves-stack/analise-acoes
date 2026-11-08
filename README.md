# Análise de ações brasileiras com Python

Projeto de análise histórica de PETR4, VALE3 e ITUB4 que compara retorno e risco usando Python. O script consulta quatro anos de dados, apresenta indicadores no terminal e exibe a evolução de um valor inicial de R$ 1.000 aplicado separadamente em cada ação.

## Objetivo

Explorar o uso de séries temporais financeiras para comparar o desempenho individual de três ações brasileiras, com preços ajustados. O projeto nasceu em um notebook do Google Colab e foi organizado aqui como um script executável.

## Tecnologias

| Biblioteca | Uso no projeto |
| --- | --- |
| pandas | Organização dos preços, retornos, correlação e tabela de resumo |
| NumPy | Fator de anualização da volatilidade |
| yfinance | Download do histórico de mercado |
| Matplotlib | Gráfico comparativo da evolução dos preços ajustados |

## Como a análise funciona

1. Consulta `PETR4.SA`, `VALE3.SA` e `ITUB4.SA` com `period="4y"` e `auto_adjust=True`.
2. Seleciona a coluna `Close` dos dados retornados.
3. Calcula retorno acumulado, retorno anualizado, retornos diários, volatilidade anualizada e drawdown máximo de cada ação.
4. Calcula a correlação entre os retornos diários das ações.
5. Reúne os principais indicadores em uma tabela.
6. Normaliza cada série para começar em R$ 1.000 e exibe um gráfico comparativo.
7. Identifica a ação com maior retorno acumulado, menor volatilidade e drawdown máximo menos intenso no período consultado.

## Indicadores e fórmulas

Considere `P_t` como o preço ajustado na data `t`, `P_0` como o primeiro preço e `P_f` como o último.

| Indicador | Cálculo | Interpretação |
| --- | --- | --- |
| Retorno acumulado | `(P_f / P_0 - 1) × 100` | Variação percentual em todo o período |
| Retorno anualizado | `((P_f / P_0) ** (1 / anos) - 1) × 100` | Taxa anual composta equivalente |
| Retorno diário | `P_t / P_(t-1) - 1` | Variação entre observações consecutivas |
| Volatilidade anualizada | `desvio padrão amostral dos retornos diários × √252 × 100` | Dispersão dos retornos, anualizada com 252 pregões |
| Drawdown | `(P_t / maior preço até t - 1) × 100` | Queda em relação ao pico anterior |
| Drawdown máximo | `mínimo da série de drawdowns` | Queda mais intensa do histórico analisado |
| Correlação | Correlação de Pearson dos retornos diários | Associação linear entre os movimentos das ações |

A duração em anos é calculada pela diferença entre a última e a primeira data disponíveis, dividida por `365.25`. A evolução normalizada é calculada por `(P_t / P_0) × 1000`.

Como o drawdown é expresso com valores negativos ou zero, o maior valor entre os drawdowns máximos representa a queda menos intensa.

## Como executar

É necessário ter Python com `pip`, acesso à internet e um ambiente com suporte à exibição de gráficos.

Abra um terminal dentro da pasta do projeto e execute:

```powershell
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe "ações.py"
```

No Linux ou macOS, use:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python ações.py
```

O terminal apresenta os indicadores e a tabela de resumo. O gráfico abre em uma janela; dependendo do ambiente, feche essa janela para que as últimas impressões do script sejam executadas.

## Estrutura

```text
analise-acoes/
├── ações.py          # Script original com a correção da variável
├── README.md         # Apresentação, metodologia e execução
├── requirements.txt  # Bibliotecas utilizadas
└── .gitignore        # Exclusões de arquivos locais do Git
```

## Premissas e limites

- Cada ação é analisada individualmente. Apesar do nome `Carteira` no código, não há pesos, rebalanceamento ou cálculo de desempenho de uma carteira conjunta.
- O período é móvel: datas e resultados mudam conforme o dia da execução e os dados disponíveis.
- Os indicadores usam preços ajustados retornados pelo provedor; não há modelagem separada de fluxos de dividendos, impostos ou custos de negociação.
- O código depende de um download bem-sucedido e de histórico suficiente. Não implementa tratamento específico para falhas de rede, séries vazias ou preços ausentes nas extremidades.
- O cálculo dos retornos diários descarta linhas com valores ausentes antes da volatilidade e da correlação.
- As dependências não estão fixadas em versões, pois não foi validada uma combinação de versões neste ambiente.

## Validação

Foi realizada revisão estática do código e conferência da substituição da variável. Não foi possível executar o script no ambiente de preparação porque o comando Python disponível aponta para um atalho sem instalação utilizável. Portanto, não são apresentados resultados numéricos nem gráficos como se tivessem sido gerados em uma execução validada.
