# previs-o-da-inadimplencia-
inadimplencia-sarima/
├── README.md
├── LICENSE
├── .gitignore
├── renv.lock                  # opcional: reprodutibilidade dos pacotes
├── data/
│   ├── raw/
│   │   └── sgs_21082.csv      # backup caso a API do BCB esteja fora
│   └── processed/
│       └── previsao_12m.csv   # tabela de previsões
├── R/
│   ├── 00_pacotes.R
│   ├── 01_importacao.R        # baixar_sgs() e criação do ts
│   ├── 02_exploratoria.R      # plot, STL, monthplot, log vs. original
│   ├── 03_estacionariedade.R  # ADF, ndiffs/nsdiffs, FAC/FACP
│   ├── 04_selecao_modelo.R    # grade de 36 modelos, AIC/BIC
│   ├── 05_diagnostico.R       # coeftest, Ljung-Box, Jarque-Bera, resíduos
│   ├── 06_previsao.R          # in-sample, hold-out, previsão 12 meses
│   └── main.R                 # roda tudo em ordem
├── outputs/
│   ├── figures/
│   │   ├── 01_serie_original.png
│   │   ├── 02_decomposicao_stl.png
│   │   ├── 03_monthplot.png
│   │   ├── 04_original_vs_log.png
│   │   ├── 05_serie_diferenciada.png
│   │   ├── 06_fac.png
│   │   ├── 07_facp.png
│   │   ├── 08_residuos.png
│   │   ├── 09_fac_residuos_quadrado.png
│   │   ├── 10_real_vs_ajustado.png
│   │   └── 11_previsao.png
│   └── tables/
│       ├── comparacao_modelos.csv
│       └── testes_diagnostico.csv
└── report/
    └── Relatorio_Econometria.pdf

# Inadimplência na Carteira de Crédito (2011–2026): modelagem SARIMA

Trabalho de Econometria de Séries Temporais, Ibmec BH.
Autoras: Aline Boy Paiva e Maria Eduarda Gomes.

## Resumo
Modelagem da série mensal de inadimplência da carteira de crédito total
(BCB/SGS 21082) com a metodologia Box-Jenkins. Modelo selecionado por
AIC/BIC: SARIMA(1,1,1)(1,0,1)[12]. Previsão de 12 meses (out/2026–set/2027).

## Dados
- Fonte: Banco Central do Brasil, SGS, série 21082
- Frequência mensal, mar/2011 a set/2026 (187 observações)
- Baixados via API: `https://api.bcb.gov.br/dados/serie/bcdata.sgs.21082`

## Metodologia
1. Análise exploratória e decomposição STL
2. Transformação logarítmica (descartada)
3. Testes de estacionariedade (ADF), definição de d e D
4. Grade de 36 modelos SARIMA (p,q ∈ 0–2; P,Q ∈ 0–1)
5. Seleção por AIC/BIC
6. Diagnóstico dos resíduos
7. Ajuste in-sample e previsão

## Principais resultados
| Item | Resultado |
|---|---|
| Modelo | SARIMA(1,1,1)(1,0,1)[12] |
| ADF (nível) | p = 0,674 (não estacionária) |
| ADF (1ª diferença) | p = 0,027 |
| Ljung-Box (lag 12 / 24) | p = 0,066 / 0,212 |
| Pico previsto | 5,51% (ago/2027) |

(inserir as figuras principais aqui)

## Como reproduzir
    install.packages(c("forecast","tseries","lmtest","jsonlite"))
    source("R/main.R")

## Pacotes
forecast, tseries, lmtest, jsonlite

## Referências
Box, G.; Jenkins, G. Time Series Analysis: Forecasting and Control.
Hyndman, R.; Athanasopoulos, G. Forecasting: Principles and Practice.
