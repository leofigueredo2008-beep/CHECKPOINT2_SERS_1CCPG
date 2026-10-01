# CHECKPOINT2_SERS_1CCPG

| Turma | Nome | RM |
|---|---|---|
| 1CCPG | Leonardo Figueredo dos Santos | 573653 |
| 1CCPG | Lucas Ramos de Souza | 573901 |
| 1CCPG | Pablo Renato dos Santos Sobral de Carvalho | 569894 |

# Avaliação — APIs, energias renováveis e aprendizado de máquina

Use o notebook de apoio para consultar **duas APIs públicas** e gerar os arquivos `aneel_classificacao_orange.csv` e `meteo_regressao_orange.csv`. Em seguida, desenvolva **duas tarefas independentes em Python**: uma de classificação e outra de regressão. **Treine e compare três algoritmos diferentes em cada tarefa.** O notebook fornece apenas a conexão às APIs e a preparação dos CSVs: bibliotecas, modelos, análises e resultados de ML devem ser acrescentados por você.

As consultas públicas escolhidas **não exigem token**. Caso um serviço passe a exigir credenciais, nunca as publique no GitHub.

## Tarefa 1 — Classificação da fonte renovável

**Fonte:** [SIGA — ANEEL](https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel). Cada linha do CSV representa um empreendimento de geração no Brasil. A coluna `fonte` reúne **Solar** (`UFV`), **Eólica** (`EOL`) e **Hidráulica** (`UHE`, `PCH`, `CGH`). O cadastro inclui empreendimentos em diferentes fases; os dados não medem energia gerada.

| Atributo no CSV | Origem na API | Descrição | Papel |
|---|---|---|---|
| `potencia_kw` | `MdaPotenciaOutorgadaKw` | Potência outorgada em quilowatts; não representa energia produzida | Entrada |
| `latitude` | `NumCoordNEmpreendimento` | Latitude aproximada, em graus decimais | Entrada |
| `longitude` | `NumCoordEEmpreendimento` | Longitude aproximada, em graus decimais | Entrada |
| `fonte` | `SigTipoGeracao` | Categoria da fonte, agrupada em três classes | Alvo |

**Seu trabalho:** explore a quantidade de linhas, valores ausentes, distribuição das classes e características das entradas. Defina `X` e `y`; separe treino/teste de forma **estratificada** (sugestão: 80%/20%) e fixe a semente. Treine **três classificadores distintos** nas mesmas divisões e compare **Accuracy, Precision, Recall, F1** e **matriz de confusão**. Informe se usou média `macro`, `weighted` ou outra para métricas multiclasses. Interprete as classes mais confundidas e as limitações de prever a fonte apenas por potência e localização. Faça padronização dentro do treino quando o algoritmo exigir; não use como entrada nome, código CEG, sigla ou descrição que entregue a resposta.

## Tarefa 2 — Regressão da radiação solar

**Fonte:** [API histórica Open-Meteo](https://open-meteo.com/en/docs/historical-weather-api). Dados horários estimados para **Petrolina (PE)**, coordenadas aproximadas **−9,39, −40,50**, de **01/04/2025 a 30/06/2025**, no fuso `America/Recife`. Cada linha do CSV é uma hora local entre **7h e 17h**. Os valores históricos são derivados de modelos/reanálise, não de um painel fotovoltaico.

| Atributo no CSV | Origem na API | Descrição | Papel |
|---|---|---|---|
| `data_hora` | `time` | Data e hora local; use para ordenar e separar por tempo | Identificação, não entrada |
| `temperatura_c` | `temperature_2m` | Temperatura do ar a 2 m, em °C | Entrada |
| `umidade_pct` | `relative_humidity_2m` | Umidade relativa a 2 m, em % | Entrada |
| `nuvens_pct` | `cloud_cover` | Cobertura total de nuvens, em % | Entrada |
| `vento_kmh` | `wind_speed_10m` | Velocidade do vento a 10 m, em km/h | Entrada |
| `hora` | Derivada de `time` | Hora local do registro, de 7 a 17 | Entrada |
| `radiacao_w_m2` | `shortwave_radiation` | Radiação solar global horizontal média da hora anterior, em W/m² | Alvo |

**Seu trabalho:** explore as variáveis e os dados ausentes; apresente ao menos uma visualização e defina `X` e `y`. Use as **primeiras 80% das horas para treino** e as últimas 20% para teste, preservando a ordem temporal. Treine **três regressores diferentes** usando a mesma divisão. Compare **MAE** (W/m²), **MSE** ((W/m²)²) e **R²**; faça um gráfico de valores reais × previstos. Explique o papel da hora do dia e por que estimar radiação **não equivale a prever a geração elétrica**. Não inclua `radiacao_w_m2` ou uma transformação direta dela em `X`.
