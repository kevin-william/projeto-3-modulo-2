# Projeto 3 — Feature extraction com ResNet-50

[Visão geral do repositório](../README.md) · [Relatório consolidado](../relatorio/Projeto_da_Disciplina.pdf)

O [notebook A3](A3_cnn_kaggle.ipynb) treina somente uma nova cabeça classificadora `fc` sobre a ResNet-50 com pesos `IMAGENET1K_V2`. O backbone permanece congelado, incluindo as estatísticas de BatchNorm durante o treino.

## Executar

Instale as dependências de [requirements.txt](requirements.txt), selecione o kernel Python local e execute de cima para baixo em kernel novo, a partir de `projeto-3/`. Treino e telemetria requerem CUDA; não há exigência de T4/Colab. O cabeçalho informa estimativas de RAM, VRAM e tempo.

## Dados e protocolo

Fonte: [Images Dataset](https://www.kaggle.com/datasets/pavansanagapati/images-dataset), baixado por KaggleHub. A auditoria registrada encontrou 1.803 imagens RGB legíveis em sete classes descobertas dinamicamente: `bike`, `cars`, `cats`, `dogs`, `flowers`, `horses` e `human`.

O split estratificado usa proporções alvo de 70% treino, 15% validação e 15% teste, com semente 42. Após arredondamento, são 1.261 imagens de treino, 271 de validação e 271 de teste, sem caminhos compartilhados.

O baseline usa RGB de 224 × 224, normalização ImageNet e batch 32, sem augmentation. AdamW ajusta somente `fc`, por até 20 épocas, com paciência de cinco épocas sem melhora da accuracy de validação. No Windows, os DataLoaders usam zero workers. O checkpoint de maior accuracy de validação é restaurado antes do teste.

## Resultado da execução v2

Accuracy de teste: **99,6310%**, com **270/271** acertos. Foram realizadas sete épocas; o melhor estado foi selecionado na época 2. A única imagem incorreta pertence à classe `bike` e foi classificada como `cars`.

O hardware registrado foi NVIDIA GeForce RTX 3050 6GB Laptop GPU, com PyTorch 2.9.0+cu130. Curvas, accuracy global e por classe, relatório de classificação e matriz de confusão estão nas saídas do notebook. O relatório textual arredondado não deve ser interpretado como accuracy de 100%.

## Artefatos e limitações

A execução grava curvas, tabelas e resumo em `resultados/v2/`, ignorado pelo Git e ausente deste checkout. Reexecutar pode substituir arquivos nessa pasta; o código não cria automaticamente uma nova versão.

A discussão propõe flip, rotação, alterações de cor e escala e compara feature extraction com fine-tuning por tamanho e domínio dos dados. Essas propostas não foram aplicadas nem avaliadas no baseline. Splits sem caminhos compartilhados não excluem duplicatas visuais, e uma única divisão não demonstra robustez externa.
