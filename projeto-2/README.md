# Projeto 2 — Ranking e busca com CLIP

[Visão geral do repositório](../README.md) · [Relatório consolidado](../relatorio/Projeto_da_Disciplina.pdf)

O [notebook A2](A2_clip_ads16.ipynb) usa `openai/clip-vit-base-patch32` em avaliação e sem gradientes. Não há treino ou fine-tuning. A implementação compara embeddings normalizados de anúncios e textos, conta correspondências para 20 conceitos fixos e retorna cinco imagens distintas para cada uma de oito consultas.

## Executar

Instale as dependências de [requirements.txt](requirements.txt), selecione o kernel local e execute as células em ordem em um kernel novo, a partir de `projeto-2/`. CUDA é usada quando disponível; CPU também é aceita. O batch de inferência é 32. O cabeçalho apresenta estimativas de recursos e duração.

O dataset é reutilizado quando encontrado no cache oficial do KaggleHub ou baixado quando necessário. Os pesos CLIP também precisam estar em cache ou ser obtidos pela biblioteca Transformers.

## Dados e interpretação

Fonte: [ADS-16 no Kaggle](https://www.kaggle.com/datasets/groffo/ads16-dataset), associado a [Personality in Computational Advertising: A Benchmark](https://ceur-ws.org/Vol-1680/paper3.pdf). Preserve as condições de uso da fonte. O corpus auditado contém 20 categorias de 15 anúncios: 300 imagens únicas. O pacote inclui um slot excedente `1/16`; sua exclusão autorizada é conferida e registrada. O corpus diverge do enunciado antigo de 16 categorias e 500 imagens; não são duplicadas imagens para atingir esse número.

São comparados os cortes 0,20, 0,25 e 0,30; o ranking usa similaridade maior ou igual a 0,25. Frequência semântica é a quantidade acima do corte, sem anotação manual de objetos. Similaridade não é probabilidade calibrada. A implementação não inclui attention maps.

## Resultado da execução v3 — 08/10/2026

| Conceito | Imagens com similaridade ≥ 0,25 |
|---|---:|
| `isolated_product` | 197/300 |
| `people_using_product` | 164/300 |
| `text_logo` | 114/300 |
| `package` | 75/300 |
| `person` | 56/300 |

A execução v3 foi concluída em CPU local com dois threads. A inferência e as figuras levaram cerca de 66 segundos, além da inicialização do Python e das bibliotecas. A ordem dos líderes e os caminhos das oito buscas não mudaram em relação à v2. `people_using_product` contou 164/300, contra 163/300 na v2 em CUDA; uma imagem próxima do corte mudou de lado devido às diferenças numéricas entre dispositivos. O hardware da v2 foi RTX 3050 Laptop de 6 GB, com PyTorch 2.9.0+cu130.

## Inspeção visual e artefatos

O notebook mantém o top-3 automático por conceito, inclusive falsos positivos. Produto isolado e pessoas usando produto não são confirmados integralmente nesses três líderes; isso não demonstra ausência no corpus inteiro. Pessoa, embalagem e texto/logotipo têm exemplos confirmados.

A galeria complementar registra um positivo inspecionado por conceito, sem alterar o ranking: relógio isolado (13/7, score 0,257), jogador usando calçados (18/12, 0,273), texto/logotipo Fiat (2/11, 0,312), embalagem Colgate (4/14, 0,294) e pessoa no Badoo (20/11, 0,313). A presença nesses cinco exemplos não valida todas as contagens semânticas.

Ranking, consultas, ambiente e figuras são gravados em `resultados/v3/`, ignorado pelo Git e ausente deste checkout. A tabela de confirmações é `resultados/v3/figuras/conceitos/confirmacoes_visuais.csv`. Os caminhos descrevem saídas locais da execução; o relatório consolidado incorpora as evidências utilizadas. Reexecutar pode substituir arquivos na v3, sem criar uma nova pasta de versão.
