# Projeto 1 — ViT para classificação de frutas

[Visão geral do repositório](../README.md) · [Relatório consolidado](../relatorio/Projeto_da_Disciplina.pdf)

O [notebook A1](A1_vision_transformers.ipynb) compara um ViT didático treinado do zero com `google/vit-base-patch16-224-in21k`, em um recorte fixo de 20 classes do Fruits-360, que inclui frutas e pimentões. Não há comparação experimental com CNN.

## Executar

Instale as dependências de [requirements.txt](requirements.txt), selecione o kernel local e execute as células em ordem, em um kernel novo, a partir de `projeto-1/`. O notebook exige CUDA e interrompe a execução se ela estiver ausente. A referência registrada é Windows, Python 3.12 e RTX 3050 Laptop de 6 GB; não há exigência de T4/Colab. O cabeçalho informa estimativas de RAM, VRAM e tempo, sem medições consolidadas de pico.

O ViT didático usa patches 16 × 16, dimensão 192, seis blocos e seis heads com projeções próprias. Usa batch 16, AdamW, mixed precision e até 15 épocas; o early stopping tem paciência de três épocas pelo F1 macro de validação. O ViT pré-treinado usa batch 1, acumulação de 16 passos, mixed precision e gradient checkpointing: até duas épocas para a cabeça e cinco para fine-tuning do backbone, com paciência de duas épocas.

## Dados e protocolo

Fonte: [Fruits-360](https://www.kaggle.com/datasets/moltean/fruits/), de Horea Mureșan e Mihai Oltean, licença [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). O notebook reutiliza a versão 107 quando completa no cache oficial do KaggleHub; caso contrário, solicita o download. Seleciona a coleção de imagens 100 × 100 e redimensiona para 224 × 224.

Preserva o Test oficial com 3.634 imagens. O Training tem 10.856 imagens, dividido em 9.227 de treino e 1.629 de validação, com estratificação e semente 42. Augmentation fica restrita ao treino. O maior F1 macro de validação seleciona os checkpoints; o teste não orienta a seleção.

## Resultado da execução v3 — 08/10/2026

| Modelo | Accuracy no Test | F1 macro |
|---|---:|---:|
| ViT do zero | 99,6147% | 0,9968 |
| ViT pré-treinado | 99,0919% | 0,9908 |

Cada fase do **ViT pré-treinado** restaurou os 200 tensores do checkpoint selecionado, sem chaves ausentes, inesperadas ou incompatíveis. A conversão usa `from_pretrained`, seguida de `load_state_dict(strict=True)` e conferência de igualdade dos tensores. O F1 macro recalculado na validação coincidiu com o registrado: cabeça 0,982092 e fine-tuning 1,000000.

O notebook registra curvas, métricas por classe, comparação, erros e attention maps de uma head individual. Parte das figuras aparece nas saídas apenas como objeto de visualização; sua presença não garante que os pixels estejam incorporados ao notebook. Métricas, checkpoints e tabelas auxiliares são gravados em `resultados/v3/`, ignorado pelo Git e ausente deste checkout. Reexecutar pode substituir arquivos da v3; o caminho não cria uma nova versão automaticamente.

## Histórico e limitações

A execução v2 registrou 99,6147% para o ViT do zero e 100% para o pré-treinado. Esses valores precedem a correção da restauração incompatível e não comprovam a avaliação do melhor checkpoint; a comparação válida é a v3 acima. O hardware registrado foi NVIDIA GeForce RTX 3050 6GB Laptop GPU, com PyTorch 2.9.0+cu130.

Attention maps não são provas causais nem segmentações. O fundo uniforme e a captura controlada limitam a generalização para cenas reais. O resultado de uma divisão não comprova superioridade geral de uma arquitetura.
