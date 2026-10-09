# Projeto 4 — cGAN e estudos de caso

Implementação das atividades 4.1 e 4.2, com os arquivos organizados nesta pasta.

## Entregas

- `A4_estudo_caso_raio_x.ipynb`: notebook executado com código completo da cGAN, bloco A de treinamento com originais, bloco B com originais e sintéticos, comparação de Recall e ablação pareada.
- `atividade_4_1_plano_de_acao.docx`: cinco problemas, impactos clínicos, plano integrado, protocolo e resultados atuais.
- `atividade_4_2_trafego_urbano.docx`: análise textual dos quatro problemas de tráfego, impactos, soluções e plano de avaliação. Não há implementação de trânsito.

## Resultados históricos da execução de 05/10/2026

A execução `resultados/20261005_234446` utilizou NVIDIA GeForce RTX 3050 6GB Laptop GPU e PyTorch 2.9.0+cu130. Foram treinadas 60 épocas da cGAN, geradas 86 imagens condicionadas em COVID e executados seis classificadores EfficientNet B0, com 12 épocas e 324 atualizações cada.

| Semente | Recall com originais | Recall com sintéticos | Diferença em pontos percentuais |
|---|---:|---:|---:|
| 2026 | 82,35% | 76,47% | -5,88 |
| 2027 | 82,35% | 82,35% | 0,00 |
| 2028 | 76,47% | 82,35% | +5,88 |
| Média | 80,39% | 80,39% | 0,00 |

Os sintéticos não trouxeram ganho médio de Recall neste teste interno. O intervalo bootstrap pareado de 95% da diferença média vai de -9,80 a +7,84 pontos percentuais; a igualdade da média não comprova equivalência. O teste contém 17 imagens COVID. Os modelos são condicionados à mesma GAN e o bootstrap não incorpora nova coleta nem treinamento de novas GANs.

O split tem 857 reais no treino, 171 na validação e 172 no teste. Usamos o split congelado anterior, sem redefini-lo com base nos resultados. A única intervenção intencional entre os braços é a inclusão de sintéticos; partições, pesos iniciais por semente, orçamento, pesos de classe, augmentation e critério de seleção são os mesmos. A primeira época com maior F1 macro de validação define o checkpoint. O teste não seleciona modelos ou candidatos.

## Limites observados

Os grupos são de similaridade perceptual, sem IDs clínicos. A independência por paciente não está comprovada. Persistem as pendências de origem, direitos e desidentificação descritas nos registros da coorte anterior, preservados na pasta de execução, com os registros estruturados convertidos para texto legível. Os sintéticos apresentam artefatos de textura e anatomia pouco definida; não receberam revisão clínica. Condicionar o gerador em COVID não certifica a patologia. O resultado não autoriza uso clínico nem demonstra generalização externa.

A nova execução de 08/10/2026 inclui ablação pareada de normalização espectral e suavização, com evidência quantitativa descrita abaixo. Os DOCX preservam a análise anterior; o notebook reúne as evidências atuais da rubrica 5.3.

## Organização da pasta

Na raiz são mantidos somente estes arquivos:

- `README.md`
- `requirements.txt`
- `A4_estudo_caso_raio_x.ipynb`
- `atividade_4_1_plano_de_acao.docx`
- `atividade_4_2_trafego_urbano.docx`

As pastas `resultados`, `cache` e `dados` são mantidas. Não há arquivos `.json` ou scripts `.py`, inclusive dentro dessas pastas. O notebook contém toda a implementação e grava novos registros em `.txt`. Os CSVs, imagens e checkpoints permanecem como evidências do experimento.

## Executar o notebook

Abra `A4_estudo_caso_raio_x.ipynb` na pasta `projeto-4`, selecione o kernel do ambiente local `.venv` e execute as células em ordem. As dependências utilizadas estão em `requirements.txt`. Não são necessários scripts externos.

Antes de executar, os valores `EPOCAS_GAN` e `EPOCAS_CLASSIFICADOR` podem ser configurados no ambiente; os padrões são 60 e 12. O notebook cria uma nova subpasta em `resultados`, registra o dispositivo usado e imprime o caminho. Ele não sobrescreve a execução apresentada neste relatório. Após importar o Matplotlib, remove o índice de fontes em JSON do cache; o índice continua disponível em memória.

As imagens estão organizadas em `dados/`. Para manter a partição congelada, o notebook lê o manifesto em `dados/particoes.csv` (com cópia preservada dos registros da coorte anterior em `.txt`). Nenhum JSON é copiado para esta pasta. Somente os pesos ImageNet locais foram reaproveitados; a GAN e os classificadores recebem treinamento novo.

## Resultados e verificações

A execução concluída está em `resultados/20261005_234446`. Seus metadados e resumo estão em arquivos `.txt`; métricas e previsões continuam em CSV. A conversão preservou os valores dos registros e não executou novos treinos. As saídas do notebook correspondem à execução concluída em 05/10/2026. Referências a nomes de registros anteriores nos documentos ou nas saídas históricas descrevem a organização usada naquela execução; os registros atuais estão em texto.

As conferências de métricas, partições, inicializações, orçamento e checkpoints estão preservadas em `resultados/20261005_234446/verificacoes`. A primeira tentativa interrompida permanece identificada em sua própria pasta, sem métricas experimentais.

Os dois DOCX foram corrigidos, renderizados com LibreOffice 26.8.0.3 e inspecionados em todas as dez páginas: seis da atividade 4.1 e quatro da atividade 4.2. A revisão visual foi aprovada, sem cortes ou sobreposições, e as auditorias de acessibilidade não encontraram ocorrências. Os PDFs, PNGs e o registro da revisão estão em `resultados/20261005_234446/verificacao_visual`, como evidências internas. Os DOCX preservados na raiz são os documentos finais.

## LibreOffice local

A instalação fica em `C:\Program Files\LibreOffice`; o executável é `C:\Program Files\LibreOffice\program\soffice.exe`. O caminho foi adicionado ao PATH do usuário e registrado nas instruções globais em `C:\Users\lord_\.codex\AGENTS.md`. A descoberta pelo caminho absoluto foi testada também com PATH vazio. Essa configuração persistente continua válida após a remoção dos scripts auxiliares desta entrega. Para novas conversões, usar o executável absoluto e um perfil temporário isolado, preservando o perfil pessoal.


## Execução atual e rubrica 5.3 — 08/10/2026

O notebook foi executado novamente, incluindo a GAN principal e os seis classificadores. A execução atual está em `resultados/20261008_191210`; o Recall médio voltou a ser 80,39% em ambos os braços. A execução principal e a ablação demoraram 23,1 minutos na RTX 3050 de 6 GB; a referência real e a figura de grades foram executadas em seguida, sem novos treinos.

A ablação compara referência sem normalização espectral/sem suavização com o pacote de mitigação, em três sementes. Cada configuração recebeu 60 épocas e 26 passos por época; os hashes dos estados iniciais e partições são iguais dentro de cada par. A janela de análise (dez últimas épocas) foi definida antes do resultado.

O posto efetivo médio aumentou de 8,6336 para 9,0151 (+4,42%), com aumento nos três pares. A saturação média caiu de 2,1354% para 0%. A distância RMS média entre pares não melhorou uniformemente: 0,1908 na referência e 0,1880 na mitigada. Não houve divergência não finita nem pares quase idênticos pelo corte RMS de 0,02 na janela final. As grades têm repetição estrutural e artefatos, sinais compatíveis com diversidade reduzida/colapso parcial, sem comprovar colapso completo. A referência exploratória com 64 imagens COVID reais de treino tem posto efetivo 16,3471.

A melhora demonstrada é modesta e restrita aos diagnósticos de treinamento; não confirma qualidade anatômica ou ganho clínico. A comparação avalia as duas estratégias juntas. Curvas, tabelas, protocolos e grades estão em `resultados/20261008_191210/ablacao/` e aparecem no notebook.

O notebook `A4_estudo_caso_raio_x.ipynb` executa a partir desta pasta ou da raiz do repositório. Seu cabeçalho informa RAM, VRAM e tempo estimado. O PDF único ficou fora desta revisão por solicitação explícita do usuário.
