# Projeto Integrado de Visão Computacional — Módulo 2

Este repositório reúne os quatro projetos de Visão Computacional do Módulo 2, cobrindo arquiteturas modernas de Deep Learning: Vision Transformers, modelos fundacionais multimodais (CLIP), Redes Convolucionais Clássicas (ResNet-50) e Redes Adversariais Generativas Condicionais (cGANs com classificação pareada).

---

## Estrutura do Repositório

```text
projeto-3-modulo-2/
├── README.md                                  # Visão geral unificada do repositório
├── .gitignore                                 # Regras de exclusão de caches e artefatos
│
├── projeto-1/                                 # Projeto 1 — Vision Transformers
│   ├── A1_vision_transformers.ipynb           # Notebook principal de execução
│   ├── README.md                              # Documentação e relatório do Projeto 1
│   └── requirements.txt                       # Dependências do Projeto 1
│
├── projeto-2/                                 # Projeto 2 — CLIP & Busca Multimodal
│   ├── A2_clip_ads16.ipynb                    # Notebook principal de execução
│   ├── README.md                              # Documentação e relatório do Projeto 2
│   └── requirements.txt                       # Dependências do Projeto 2
│
├── projeto-3/                                 # Projeto 3 — CNN & Feature Extraction
│   ├── A3_cnn_kaggle.ipynb                    # Notebook principal de execução
│   ├── README.md                              # Documentação e relatório do Projeto 3
│   └── requirements.txt                       # Dependências do Projeto 3
│
└── projeto-4/                                 # Projeto 4 — cGAN & Estudos de Caso
    ├── A4_estudo_caso_raio_x.ipynb            # Notebook principal de execução
    ├── atividade_4_1_plano_de_acao.docx       # Relatório clínico e plano de ação (Atividade 4.1)
    ├── atividade_4_2_trafego_urbano.docx      # Análise textual de tráfego urbano (Atividade 4.2)
    ├── dados/                                 # Partição congelada e radiografias reais
    ├── resultados/                            # Evidências e relatórios experimentais
    ├── README.md                              # Documentação e relatório do Projeto 4
    └── requirements.txt                       # Dependências do Projeto 4
```

---

## Projetos e Notebooks Principais

### 1. Projeto 1 — Classificação de Frutas com Vision Transformers
- **Notebook**: `projeto-1/A1_vision_transformers.ipynb`
- **Dataset**: Fruits-360 (download automatizado via `kagglehub`).
- **Escopo**: Implementação didática de um ViT construído do zero com atenção multi-head e projeções lineares independentes, comparado com fine-tuning de ViT pré-treinado (`google/vit-base-patch16-224-in21k`).
- **Destaques**: Visualização e extração de attention maps com sobreposição nas imagens, auditoria estrita de restauração dos 200 tensores do checkpoint.

### 2. Projeto 2 — Ranking e Busca Semântica em Anúncios com CLIP
- **Notebook**: `projeto-2/A2_clip_ads16.ipynb`
- **Dataset**: ADS-16 (download automatizado via `kagglehub`).
- **Escopo**: Avaliação zero-shot e busca semântica texto-imagem com o modelo multimodal `openai/clip-vit-base-patch32`.
- **Destaques**: Cálculo de embeddings normalizados, análise de sensibilidade e ranking de 20 conceitos líderes com threshold fixo de 0,25, além de busca por consultas textuais com top-5 imagens recuperadas.

### 3. Projeto 3 — Classificação de Objetos com ResNet-50
- **Notebook**: `projeto-3/A3_cnn_kaggle.ipynb`
- **Dataset**: Images Dataset (download automatizado via `kagglehub`).
- **Escopo**: Transfer learning com feature extraction sobre arquitetura convolucional profunda ResNet-50 com pesos `IMAGENET1K_V2`. O backbone e as camadas BatchNorm permanecem congelados, treinando apenas a cabeça linear `fc`.
- **Destaques**: Divisão estratificada disjunta (70/15/15), curvas de treinamento e avaliação com matriz de confusão e métricas por classe.

### 4. Projeto 4 — cGAN, Classificação Pareada e Estudos de Caso
- **Notebook**: `projeto-4/A4_estudo_caso_raio_x.ipynb`
- **Documentos de Estudo de Caso**:
  - `projeto-4/atividade_4_1_plano_de_acao.docx`: Análise clínica de cinco problemas em IA médica e plano de ação integrado.
  - `projeto-4/atividade_4_2_trafego_urbano.docx`: Análise textual e proposição arquitetural para monitoramento de tráfego urbano inteligente.
- **Dataset**: Coorte local de 1.200 radiografias de tórax (Normal, Pneumonia e COVID-19) com partição congelada e hashes SHA-256 validados em `projeto-4/dados/particoes.csv`.
- **Escopo**: Treinamento de cGAN condicional para sintetizar imagens da classe minoritária (COVID-19), seguido de avaliação pareada de classificadores EfficientNet-B0 (braço A: apenas dados originais vs. braço B: aumentados com imagens geradas). Inclui estudo de ablação de estabilidade (normalização espectral e suavização de rótulos).

---

## Instruções de Execução

1. **Ambiente Python**:
   Recomenda-se utilizar um ambiente virtual Python 3.10 ou superior:
   ```bash
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

2. **Instalação de Dependências**:
   Cada pasta de projeto contém seu respectivo arquivo `requirements.txt`:
   ```bash
   pip install -r projeto-1/requirements.txt
   pip install -r projeto-2/requirements.txt
   pip install -r projeto-3/requirements.txt
   pip install -r projeto-4/requirements.txt
   ```

3. **Execução dos Notebooks**:
   Abra os notebooks no Jupyter Lab, VS Code ou Antigravity IDE e selecione o kernel do ambiente virtual:
   - `projeto-1/A1_vision_transformers.ipynb`
   - `projeto-2/A2_clip_ads16.ipynb`
   - `projeto-3/A3_cnn_kaggle.ipynb`
   - `projeto-4/A4_estudo_caso_raio_x.ipynb`

Os notebooks 1, 2 e 3 realizam o download dinâmico e transparente dos dados via `kagglehub`. O notebook 4 consome diretamente a partição congelada presente em `projeto-4/dados/`.
