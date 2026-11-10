# Clusterização de clientes — Disman

Painel autocontido para importação local de bases de PDVs, clusterização, análise de entregas, nível de serviço (OT/IF/OTIF), rating e plano de ação.

## Uso

Abra o painel e selecione a pasta com os arquivos de dados. O processamento ocorre localmente no navegador.

## Publicação

Projeto estático preparado para publicação na Vercel.

## Desempenho e visual (revisão geral)

- **Gravação**: mudar status do plano de ação, metas de entrega ou ano grava só os parâmetros (milissegundos) e sincroniza com o banco logo depois, sem travar a tela. Importar ou limpar bases continua gravando tudo. O texto pesado das bases é reaproveitado entre sincronizações.
- **Abertura**: o painel guarda no navegador a marca (data + tamanho) da última versão sincronizada e só baixa as bases do banco quando outra pessoa as alterou.
- **Importação**: leitura de CSV ~4x mais rápida e a lista de arquivos é redesenhada sem excesso durante a importação.
- **Cálculo**: datas, janelas de pedido e períodos são memorizados; os resultados são idênticos aos anteriores (conferido tela a tela).
- **Visual**: camada final `ds2` no `index.html` (faixa de título compacta, seletores de visão claros, filtros e tabelas mais densos, sem texto cortado, barra de processamento vazia quando ocioso).
