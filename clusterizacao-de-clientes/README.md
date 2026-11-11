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

### Status da importação
Ao fim de cada importação aparece, no cartão "Processamento", um resumo: concluída sem erros (verde), com alertas (arquivo não reconhecido ou mês não identificado, em amarelo) ou com erros (arquivo ilegível ou falha ao salvar, em vermelho). Mostra arquivos importados, registros, horário, lista dos arquivos com problema e se os dados foram sincronizados com o banco.

## Revisão 4 — devoluções por datas distintas, textos normalizados e PPT redesenhado

- **Devolução = datas distintas por cliente.** Em todo o painel e no PPT, uma devolução é um par (PDV, data); várias linhas ou notas no mesmo dia contam uma vez. Substitui "Linhas de devolução".
- **Devolução PDV (%)** = devoluções (PDV/dia) ÷ entregas (PDV/dia) no filtro. PDVs reincidentes/críticos = clientes com devolução em mais de uma data.
- **Textos de motivo e responsabilidade normalizados**: abreviações expandidas (ex.: "ciente canc." → "Cliente cancelou"), variações com erro de digitação unidas ao rótulo mais frequente e mapa opcional em `state.params.returnLabelMap` (`{"texto em minúsculas":"Rótulo final"}`).
- **PPT redesenhado**: grade única, cartões de KPI, rankings em barras horizontais no lugar de tabelas densas, tabelas com linhas alternadas e novos slides de motivos/responsabilidade e RNs/gerentes.

## Revisão 5 — indicadores por cidade

- **Cidades normalizadas.** O nome da cidade vem do cadastro (`Cidade`) e do arquivo de pedidos (`Nome Município`). Variações de escrita são unidas por aproximação (maiúsculas/minúsculas, acentos, sufixo de UF como "- PE", "S." por "São", conectores "de/da/do" e um erro de digitação em uma palavra) e o nome final sai com a grafia correta ("SAO JOSE DO EGITO" → "São José do Egito"). Para forçar um nome, use o mapa opcional `state.params.cityMap` (`{"texto em minúsculas":"Nome final"}`), no mesmo padrão do `returnLabelMap`. Duas grafias muito frequentes e parecidas (mais de 30 ocorrências cada) não são unidas automaticamente, por segurança.
- **OTIF por cidade** (aba Nível de serviço → OTIF): filtro "Cidade", ranking da pior para a melhor, gráfico OTIF/In Full/On Time das 12 principais cidades, mapa de calor cidade × mês e tabela completa (PDVs, atendimentos, OT, IF, OTIF, Rating). Clicar numa cidade filtra o painel.
- **Devolução por cidade** (aba Devolução): filtro "Cidade", barras de Devolução PDV por cidade, tabela e mapa de calor cidade × mês.
- **PPT resumido**: novos slides "Nível de serviço por cidade" e "Devolução por cidade", respeitando os filtros.
