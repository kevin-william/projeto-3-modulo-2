# Projeto Integrado de Visão Computacional — Módulo 2

Este repositório reúne quatro projetos sobre Vision Transformers, busca multimodal com CLIP, feature extraction com ResNet-50 e geração condicional de radiografias com avaliação pareada de classificadores. O estudo de tráfego urbano é uma análise textual, sem implementação de um modelo de trânsito.

## Navegação e entregas

| Projeto | Documentação | Notebook / documentos | Escopo |
|---|---|---|---|
| 1 | [README do Projeto 1](projeto-1/README.md) | [A1 — Vision Transformers](projeto-1/A1_vision_transformers.ipynb) | ViT do zero versus ViT pré-treinado, em 20 classes do Fruits-360 |
| 2 | [README do Projeto 2](projeto-2/README.md) | [A2 — CLIP e ADS-16](projeto-2/A2_clip_ads16.ipynb) | Ranking de 20 conceitos e oito buscas texto-imagem em 300 anúncios |
| 3 | [README do Projeto 3](projeto-3/README.md) | [A3 — ResNet-50](projeto-3/A3_cnn_kaggle.ipynb) | Treino da cabeça classificadora em sete classes, com backbone congelado |
| 4 | [README do Projeto 4](projeto-4/README.md) | [A4 — cGAN e raio-X](projeto-4/A4_estudo_caso_raio_x.ipynb), [plano de ação 4.1](projeto-4/atividade_4_1_plano_de_acao.docx) e [tráfego urbano 4.2](projeto-4/atividade_4_2_trafego_urbano.docx) | Síntese de COVID-19, classificação pareada e estudos de caso |

O [relatório técnico consolidado](relatorio/Projeto_da_Disciplina.pdf), de 41 páginas, reúne os protocolos, resultados, limitações, referências e apêndices das atividades. A pasta `relatorio/` e esse PDF fazem parte do repositório. Os DOCX do Projeto 4 preservam a análise anterior; o notebook e o relatório consolidado incluem a execução de 08/10/2026 e a ablação da GAN.

## Estrutura do repositório

```text
projeto-3-modulo-2/
├── README.md                                  # Visão geral unificada do repositório
├── .gitignore                                 # Regras de exclusão de caches e artefatos
│
├── projeto-1/                                 # Projeto 1 — Vision Transformers
│   ├── A1_vision_transformers.ipynb           # Notebook principal de execução
│   ├── README.md                              # Documentação e resultados do Projeto 1
│   └── requirements.txt                       # Dependências do Projeto 1
│
├── projeto-2/                                 # Projeto 2 — CLIP e busca multimodal
│   ├── A2_clip_ads16.ipynb                    # Notebook principal de execução
│   ├── README.md                              # Documentação e resultados do Projeto 2
│   └── requirements.txt                       # Dependências do Projeto 2
│
├── projeto-3/                                 # Projeto 3 — CNN e feature extraction
│   ├── A3_cnn_kaggle.ipynb                    # Notebook principal de execução
│   ├── README.md                              # Documentação e resultados do Projeto 3
│   └── requirements.txt                       # Dependências do Projeto 3
│
├── projeto-4/                                 # Projeto 4 — cGAN e estudos de caso
│   ├── A4_estudo_caso_raio_x.ipynb            # Notebook principal de execução
│   ├── atividade_4_1_plano_de_acao.docx       # Relatório clínico e plano de ação (Atividade 4.1)
│   ├── atividade_4_2_trafego_urbano.docx      # Análise textual de tráfego urbano (Atividade 4.2)
│   ├── dados/                                 # Partição congelada e radiografias reais (pasta local)
│   ├── resultados/                            # Evidências experimentais geradas nas execuções locais
│   ├── README.md                              # Documentação e resultados do Projeto 4
│   └── requirements.txt                       # Dependências do Projeto 4
│
└── relatorio/                                 # Relatório técnico consolidado, versionado no Git
    └── Projeto_da_Disciplina.pdf              # Relatório dos quatro projetos, com resultados e apêndices
```

A árvore apresenta as entregas versionadas e as pastas locais de dados e resultados do Projeto 4. Na execução local, o Projeto 4 também exige `projeto-4/dados/particoes.csv` e as radiografias em `dados/Normal/`, `dados/Pneumonia/` e `dados/COVID-19/`. Essa pasta foi restaurada no workspace; sua presença local não implica inclusão no Git. Os notebooks geram pastas `resultados/`; o Projeto 4 também usa `cache/`. Dados, caches e checkpoints não estão incorporados ao PDF.

## Resultados registrados

| Projeto / execução | Resultado | Limite de interpretação |
|---|---|---|
| 1 — v3, 08/10/2026 | Accuracy: 99,6147% no ViT do zero e 99,0919% no pré-treinado; F1 macro: 0,9968 e 0,9908 | Test oficial com 3.634 imagens; captura controlada, sem validação externa |
| 2 — v3, 08/10/2026 | Corte 0,25: produto isolado 197/300, pessoas usando produto 164/300, texto/logotipo 114/300, embalagem 75/300 e pessoa 56/300 | Frequência de similaridade semântica, com falsos positivos; não é contagem anotada de objetos |
| 3 — v2 | Accuracy de 99,6310%, com 270/271 acertos | Uma divisão estratificada fixa; propostas de augmentation não foram aplicadas ao baseline |
| 4 — 20261008_191210 | Recall médio COVID de 80,39% nos dois braços; diferença média de 0 ponto percentual | Teste interno com 17 imagens COVID; não comprova equivalência nem benefício clínico |

No Projeto 1, a restauração estrita dos 200 tensores foi auditada em cada fase do modelo pré-treinado. No Projeto 3, backbone e estatísticas de BatchNorm permanecem congelados. No Projeto 4, a ablação pareada avalia normalização espectral e suavização de rótulos juntas; a melhora modesta nos diagnósticos de diversidade não certifica qualidade anatômica.

## Execução local no Windows

As execuções registradas usam Python 3.12. Cada projeto tem seu próprio `requirements.txt`, com versões e requisitos distintos; prefira um ambiente por projeto ao reproduzir as atividades.

Exemplo para o Projeto 1, a partir da raiz:

```powershell
cd projeto-1
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m jupyter lab
```

Repita a preparação na pasta da atividade desejada e selecione o kernel correspondente. O Projeto 4 inclui `ipykernel`, mas não Jupyter Lab em seu arquivo de dependências; se precisar desse editor, instale-o no ambiente com `python -m pip install jupyterlab` ou use o suporte a notebooks do VS Code.

| Projeto | Diretório de execução | Dispositivo |
|---|---|---|
| 1 | `projeto-1/` | CUDA obrigatória; o notebook interrompe a execução sem GPU CUDA |
| 2 | `projeto-2/` | CUDA quando disponível; aceita CPU |
| 3 | `projeto-3/` | CUDA obrigatória para treino e telemetria |
| 4 | `projeto-4/` ou raiz do repositório, com o manifesto presente | CUDA quando disponível; aceita CPU, com maior tempo de execução |

Use uma instalação de PyTorch com suporte CUDA para os projetos que exigem GPU. O `requirements.txt` do Projeto 4 fixa as versões `torch==2.9.0+cu130` e `torchvision==0.24.0+cu130`, que exigem uma fonte de wheels compatível; instalar as dependências sem essa configuração pode não resolver essas versões. As versões declaradas nos arquivos não garantem, por si só, a disponibilidade de CUDA.

Os Projetos 1, 2 e 3 obtêm os datasets via KaggleHub, com reutilização de cache quando disponível. Modelos pré-treinados também precisam de download ou cache. O Projeto 4 usa a coorte local congelada de 1.200 radiografias e verifica os hashes SHA-256 antes do treino.

Os Projetos 1 e 2 gravam em `resultados/v3/`; o Projeto 3, em `resultados/v2/`. Reexecutar pode substituir arquivos nessas mesmas pastas. O Projeto 4 cria uma pasta com data e hora para cada execução. Consulte o README de cada atividade para os protocolos e pré-requisitos específicos.

## Revisão da documentação — 09/10/2026

Os cinco READMEs foram conferidos contra as células de código e as saídas preservadas dos quatro notebooks, os documentos de estudo de caso e o relatório consolidado. A revisão corrigiu referências a execuções antigas, organização local versus arquivos versionados e comportamento de gravação dos resultados. A coorte restaurada foi conferida: 1.200 arquivos presentes, 1.200 hashes únicos e nenhum SHA-256 divergente do manifesto. Esta revisão não executou novos treinamentos.
