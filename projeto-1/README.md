# Projeto 1 — ViT para classificação de frutas

O notebook `A1_vision_transformers.ipynb` compara um ViT didático treinado do zero com `google/vit-base-patch16-224-in21k`. O código usa nomes próprios em português e explicações curtas antes de cada etapa.

## Executar

Use o Python do ambiente local com as dependências de `requirements.txt`. Abra o notebook a partir desta pasta e execute de cima para baixo em um kernel novo. O treinamento requer CUDA. A execução v3 registra o hardware efetivamente usado, sem requisito de T4/Colab.

O ViT didático usa patches 16 × 16, dimensão 192, seis blocos e seis heads com projeções próprias. Treina por até 15 épocas, com early stopping de três épocas pelo F1 macro. O ViT pré-treinado usa batch 1, acumulação de 16 passos e mixed precision: até duas épocas para a cabeça e cinco para fine-tuning.

## Dados e resultados

Fonte: [Fruits-360](https://www.kaggle.com/datasets/moltean/fruits/), de Horea Mureșan e Mihai Oltean, licença [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). O download usa KaggleHub. Preservamos o Test oficial; a validação é 15% do Training, estratificada com semente 42, para o recorte fixo de 20 classes.

As saídas do notebook documentam curvas, tabela comparativa, métricas por classe, erros e heatmap de um head individual. A execução corrigida salva checkpoints e tabelas auxiliares em `resultados/v3/`, ignorado pelo Git. Artefatos anteriores não são sobrescritos.

A comparação não inclui uma CNN. Attention maps não são provas causais. O fundo uniforme e a captura controlada limitam a generalização para cenas reais.


## Resultado da execução v3 — 08/10/2026

| Modelo | Accuracy no Test | F1 macro |
|---|---:|---:|
| ViT do zero | 99.6147% | 0.9968 |
| ViT pré-treinado | 99.0919% | 0.9908 |

Cada fase restaurou os 200 tensores do checkpoint selecionado pela validação, sem chaves ausentes, inesperadas ou incompatíveis. O F1 macro recalculado coincidiu com o registrado (cabeca: 0.982092, fine_tuning: 1.000000). Os resultados atuais estão em `resultados/v3/`; os v2 permanecem como histórico.

## Resultado histórico da execução v2

Estes valores são registros anteriores à correção da restauração incompatível. Eles não comprovam a avaliação do melhor checkpoint e não substituem a comparação validada da execução v3 acima.

| Modelo | Accuracy no Test | F1 macro |
|---|---:|---:|
| ViT do zero | 99.6147% | 0.9968 |
| ViT pré-treinado | 100.0000% | 1.0000 |

Hardware: NVIDIA GeForce RTX 3050 6GB Laptop GPU; PyTorch 2.9.0+cu130.


## Entrega padronizada

O notebook `A1_vision_transformers.ipynb` fica nesta pasta. Execute-o a partir desta pasta. O início do notebook informa RAM, VRAM e tempo estimado para o ambiente local.

A restauração corrigida usa a conversão oficial do Transformers, rejeita chaves ausentes ou incompatíveis e confere todos os tensores após `load_state_dict(strict=True)`. Os resultados v2 apresentados são históricos; o notebook atualizado apresenta a nova execução.

