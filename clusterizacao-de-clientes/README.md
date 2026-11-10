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

### Revisão 2
- Troca OTIF → Devolução: cálculo da devolução agrupado em uma só passagem e pré-montado em tempo ocioso quando a aba Nível de Serviço abre (≈420 ms → ≈20 ms).
- Tela de carregamento no mesmo padrão do painel de Metas e Incentivos.
- Gráfico mensal OTIF/In Full/On Time refeito (colunas OTIF, linhas IF e OT, filtro por clique); devolução mensal em colunas; rosca corrigida; listas de ação compactas; filtros alinhados em grade.

### PPT resumido (revisão 3)
Gerado direto dos dados (não da tela), respeitando os filtros de cada aba. Ao clicar em "Gerar PPT resumido" abre uma janela para escolher o que entra: Nível de serviço, Devolução, Fora de rota e Plano de ação. Slides: capa, resumo (KPIs, último mês × mês anterior, destaques automáticos e gráfico mensal), resultado por cluster + "onde agir primeiro", 10 PDVs com menor OTIF (mín. 3 atendimentos válidos), devolução (KPIs e gráficos mensais), destaques de devolução (RN, gerente, motivo), clientes com mais devoluções, fora de rota, plano de ação do mês mais recente e encerramento com a leitura dos indicadores. Fonte Calibri, identidade do painel, rodapé com data, logo e número de página. Se a biblioteca não carregar, aparece um aviso com "Tentar de novo".
