# Projeto 4 — cGAN e estudos de caso

[Visão geral do repositório](../README.md) · [Relatório consolidado](../relatorio/Projeto_da_Disciplina.pdf)

## Entregas e organização

- [A4 — Estudo de caso de raio-X](A4_estudo_caso_raio_x.ipynb): implementação completa da cGAN, classificadores EfficientNet-B0 com originais versus originais e sintéticos, comparação de Recall e ablação pareada.
- [Atividade 4.1 — Plano de ação](atividade_4_1_plano_de_acao.docx): cinco problemas, impactos clínicos, plano integrado, protocolo e resultados da análise anterior.
- [Atividade 4.2 — Tráfego urbano](atividade_4_2_trafego_urbano.docx): quatro problemas, impactos, soluções e plano de avaliação. É uma análise textual; não há implementação de trânsito.

Esses arquivos, `README.md` e `requirements.txt` são as entregas versionadas nesta pasta. O relatório consolidado fica em `../relatorio/Projeto_da_Disciplina.pdf` e inclui a execução de 08/10/2026 e a ablação; os DOCX preservam a análise anterior.

Localmente, `dados/` contém a coorte e o manifesto; `cache/` guarda pesos e arquivos temporários; `resultados/` recebe registros, métricas, previsões, imagens e checkpoints. A pasta `dados/` foi restaurada no workspace em 09/10/2026, mas está fora do conjunto de arquivos versionados nesta revisão. As pastas das execuções históricas citadas abaixo não estão presentes neste checkout. O notebook contém toda a implementação, sem exigir scripts externos, e grava registros estruturados em `.txt` e tabelas em CSV.

## Executar e pré-requisitos

Instale as dependências de [requirements.txt](requirements.txt), selecione o kernel local e execute as células em ordem. A referência registrada usa Python 3.12 e PyTorch 2.9.0+cu130. O arquivo fixa wheels `+cu130` de PyTorch e torchvision; a instalação precisa de uma fonte compatível com essas versões. Jupyter Lab não consta nesse arquivo: use o suporte a notebooks do VS Code ou instale o editor separadamente.

Execute a partir de `projeto-4/` ou da raiz do repositório. A primeira célula de código exige `projeto-4/dados/particoes.csv` (ou `dados/particoes.csv`, quando executada nesta pasta). Embora exista uma busca posterior por manifestos históricos, ela não substitui esse requisito inicial.

A coorte local contém 1.200 radiografias: 840 Normal, 240 Pneumonia e 120 COVID-19, em `dados/Normal/`, `dados/Pneumonia/` e `dados/COVID-19/`. A conferência da restauração encontrou todos os arquivos, 1.200 hashes SHA-256 únicos e nenhuma divergência com o manifesto. O notebook também verifica legibilidade e separação de grupos e hashes perceptuais entre partições.

| Classe | Treino | Validação | Teste |
|---|---:|---:|---:|
| Normal | 600 | 120 | 120 |
| Pneumonia | 171 | 34 | 35 |
| COVID-19 | 86 | 17 | 17 |
| Total | 857 | 171 | 172 |

O manifesto congelado não é redefinido a partir dos resultados. Os registros de origem e política disponíveis ficam em `dados/politica_coorte_v2.txt`; a restauração não resolve as pendências de direitos, desidentificação ou independência por paciente.

CUDA é utilizada quando disponível; o código aceita CPU com dois threads. Os tempos relatados são da GPU RTX 3050 de 6 GB. O cabeçalho apresenta estimativas de recursos. Os padrões de `EPOCAS_GAN` e `EPOCAS_CLASSIFICADOR` são 60 e 12; podem ser definidos no ambiente antes da execução. A GAN usa resolução 128 e o classificador 160 pixels.

O notebook cria `resultados/AAAAMMDD_HHMMSS/` e não sobrescreve uma pasta existente. Os pesos ImageNet de EfficientNet-B0 são reutilizados do cache ou baixados; se isso falhar, o notebook solicita o checkpoint em `cache/torch/hub/checkpoints/efficientnet_b0_rwightman-7f5810bc.pth`. A GAN e os classificadores recebem treinamento novo. Após importar Matplotlib, o notebook remove o índice de fontes JSON do cache local, mantendo-o em memória.

## Execução atual — 08/10/2026

As saídas preservadas do notebook correspondem a `resultados/20261008_191210`. A execução treinou a GAN principal por 60 épocas, gerou 86 imagens condicionadas em COVID-19 e treinou seis classificadores por 12 épocas e 324 atualizações cada.

| Semente | Recall com originais | Recall com sintéticos | Diferença em pontos percentuais |
|---|---:|---:|---:|
| 2026 | 82,35% | 76,47% | -5,88 |
| 2027 | 82,35% | 82,35% | 0,00 |
| 2028 | 76,47% | 82,35% | +5,88 |
| Média | 80,39% | 80,39% | 0,00 |

A única intervenção intencional entre os braços é a inclusão de sintéticos no treino. Partições, pesos iniciais por semente, orçamento de atualizações, pesos de classe, augmentation e critério de seleção são iguais. O primeiro maior F1 macro de validação define o checkpoint; o teste não seleciona modelos ou candidatos.

Os sintéticos não trouxeram ganho médio de Recall no teste interno. O intervalo bootstrap pareado de 95% da diferença média vai de -9,80 a +7,84 pontos percentuais; igualdade das médias não comprova equivalência. O teste contém apenas 17 imagens COVID. O bootstrap é condicionado aos modelos e à mesma GAN, sem nova coleta ou treinamento de novas GANs.

## Ablação pareada — rubrica 5.3

A referência sem normalização espectral nem suavização é comparada ao pacote com ambas as estratégias, em três sementes. Cada configuração recebeu 60 épocas e 26 passos por época, com estados iniciais e partições iguais em cada par. A janela das dez últimas épocas foi definida antes dos resultados.

| Diagnóstico médio na janela final | Referência | Mitigação |
|---|---:|---:|
| Posto efetivo | 8,6336 | 9,0151 |
| Saturação | 2,1354% | 0% |
| Distância RMS entre pares | 0,1908 | 0,1880 |

O posto efetivo aumentou 4,42%, com aumento nos três pares. A distância RMS não melhorou uniformemente. Não houve divergência não finita nem pares quase idênticos pelo corte RMS de 0,02 na janela final. As grades mostram repetição estrutural e artefatos, compatíveis com diversidade reduzida ou colapso parcial, sem comprovar colapso completo. A referência exploratória de 64 imagens COVID reais de treino tem posto efetivo 16,3471.

A execução principal e a ablação levaram cerca de 23,1 minutos na RTX 3050; a referência real e a figura de grades foram executadas depois, sem novos treinos. Curvas, protocolos, tabelas e grades são gravados em `resultados/20261008_191210/ablacao/`, aparecem nas saídas do notebook e estão discutidos no relatório consolidado. Esses caminhos identificam a execução histórica, não pastas disponíveis neste checkout.

## Histórico e limitações

A execução de 05/10/2026, identificada como `resultados/20261005_234446`, registrou os mesmos recalls por semente e a mesma média. Seus registros de verificações e revisão visual dos DOCX são históricos; não foram repetidos nesta revisão de documentação.

Os grupos são de similaridade perceptual, sem independência por paciente comprovada. Os sintéticos apresentam artefatos de textura e anatomia pouco definida, sem revisão clínica; condicionar o gerador em COVID não certifica patologia. A melhora da ablação é modesta, restrita aos diagnósticos de treinamento, e avalia as duas estratégias juntas. O resultado não demonstra qualidade anatômica, utilidade clínica ou generalização externa.

## Conversão dos documentos no Windows

Para novas conversões, use `C:\Program Files\LibreOffice\program\soffice.exe` (ou `soffice.com` para saída de console), conferindo o caminho absoluto. Utilize um perfil temporário isolado dentro da pasta de trabalho para preservar o perfil pessoal e os documentos abertos. A instalação local foi explicitamente autorizada pelo usuário.
