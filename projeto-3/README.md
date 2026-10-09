# Projeto 3 — Feature extraction com ResNet-50

O notebook `A3_cnn_kaggle.ipynb` treina somente uma nova cabeça classificadora sobre a ResNet-50 com pesos `IMAGENET1K_V2`. O backbone fica congelado, incluindo as estatísticas de BatchNorm durante o treino.

## Executar

Abra o notebook nesta pasta com o ambiente Python local e as dependências de `requirements.txt`. Execute de cima para baixo em kernel novo. Treino e telemetria requerem CUDA local; não há exigência de T4/Colab.

Fonte: [Images Dataset](https://www.kaggle.com/datasets/pavansanagapati/images-dataset), baixado por KaggleHub. Auditamos 1.803 imagens e descobrimos as classes dinamicamente. O split estratificado usa 70% treino, 15% validação e 15% teste, com semente 42.

Usamos RGB de 224 × 224, normalização ImageNet, batch 32 e nenhuma augmentation no baseline. AdamW ajusta somente `fc`; o melhor estado é selecionado pela accuracy de validação, com paciência de cinco épocas.

## Evidências

O notebook registra curvas, accuracy global e por classe, relatório e matriz de confusão. A discussão propõe quatro estratégias de augmentation e compara feature extraction com fine-tuning por tamanho e domínio dos dados.

A antiga tabela hardcoded era conteúdo editorial e foi substituída por Markdown. Valores como normalização e tamanho da imagem continuam explícitos por fazerem parte do protocolo.

Resultados locais da nova execução ficam em `resultados/v2/`, sem sobrescrever os anteriores. Splits sem caminhos compartilhados não excluem duplicatas visuais, e uma única divisão não demonstra robustez externa.


## Resultado da execução v2

Accuracy de teste: **99.6310%**, com **270/271** acertos. Métricas por classe e curvas estão nas saídas do notebook.

Hardware: NVIDIA GeForce RTX 3050 6GB Laptop GPU; PyTorch 2.9.0+cu130.


## Entrega padronizada

O notebook `A3_cnn_kaggle.ipynb` fica nesta pasta. Execute-o a partir desta pasta. O início do notebook informa RAM, VRAM e tempo estimado para o ambiente local.

