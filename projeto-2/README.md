# Projeto 2 — Ranking e busca com CLIP

O notebook `A2_clip_ads16.ipynb` usa `openai/clip-vit-base-patch32` em avaliação e sem gradientes. Não há treino ou fine-tuning.

## Executar

Use o ambiente local e as dependências de `requirements.txt`. Execute as células em ordem em um kernel novo, a partir desta pasta. CUDA é usada quando disponível; o notebook também aceita CPU e registra o dispositivo.

O código cobre dados, 20 descrições fixas, embeddings normalizados, comparação de thresholds, ranking, cinco conceitos líderes e oito consultas, com cinco imagens distintas por consulta. A seção adicional de attention maps foi removida por não ser necessária às rubricas de CLIP.

## Dados e interpretação

Fonte: [ADS-16 no Kaggle](https://www.kaggle.com/datasets/groffo/ads16-dataset), associado a [Personality in Computational Advertising: A Benchmark](https://ceur-ws.org/Vol-1680/paper3.pdf). Preserve as condições de uso da fonte. São 20 categorias de 15 anúncios: 300 imagens únicas. O pacote inclui um slot excedente `1/16`; sua exclusão autorizada é conferida e registrada. O corpus diverge do enunciado antigo que citava 16 categorias e 500 imagens; não duplicamos imagens para atingir esse número.

Comparamos thresholds 0,20, 0,25 e 0,30 e mantemos 0,25 para o ranking. Frequência semântica significa quantidade acima do corte; não é contagem validada de objetos. Similaridade não é probabilidade calibrada.

Figuras, ranking, consultas e ambiente atuais ficam em `resultados/v3/`, ignorado pelo Git; a v2 foi preservada como histórico. As análises no notebook devem corresponder às imagens e aos scores da nova execução.


## Resultado histórico da execução v2

Conceitos líderes com threshold 0,25: `isolated_product` (197/300), `people_using_product` (163/300), `text_logo` (114/300), `package` (75/300), `person` (56/300). A frequência mede correspondência semântica, sem anotação manual de objetos.

Hardware: NVIDIA GeForce RTX 3050 6GB Laptop GPU; PyTorch 2.9.0+cu130.


## Entrega padronizada

O notebook `A2_clip_ads16.ipynb` fica nesta pasta. Execute-o a partir desta pasta. O início do notebook informa RAM, VRAM e tempo estimado para o ambiente local.

A seção de conceitos líderes inclui uma tabela de confirmação visual por caminho. Produto isolado e pessoas usando produto não são confirmados integralmente nos três líderes; essa insuficiência é explícita, sem concluir ausência no corpus inteiro. Pessoa, embalagem e texto/logotipo têm exemplos visualmente confirmados.


## Revisão executada em 08/10/2026

A execução atual está em `resultados/v3/` e foi concluída em CPU local com dois threads, preservando o corpus, prompts, consultas e corte de 0,25. A inferência e as figuras levaram cerca de 66 segundos, além da inicialização do Python e das bibliotecas. A ordem dos líderes e os caminhos das oito buscas não mudaram. `people_using_product` contou 164/300, contra 163/300 na execução CUDA v2: uma imagem próxima do corte mudou de lado devido às diferenças numéricas entre dispositivos.

A galeria complementar apresenta um positivo inspecionado por conceito, sem substituir os falsos positivos do top-3 ou alterar o ranking. Todos os cinco ficaram acima do corte: relógio isolado (13/7, score 0,257), jogador usando calçados (18/12, 0,273), texto/logotipo Fiat (2/11, 0,312), embalagem Colgate (4/14, 0,294) e pessoa no Badoo (20/11, 0,313). Imagem, caminho, SHA-256, score e critério visual estão em `resultados/v3/figuras/conceitos/confirmacoes_visuais.csv` e aparecem no notebook.

O arquivo `A2_clip_ads16.ipynb` foi sincronizado com essa execução. A presença visual nesses cinco casos não valida todas as ocorrências semânticas do ranking.

