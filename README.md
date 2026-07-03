# Automatizando o reporte de pendências contábeis de um produto de crédito de alto volume

**Contexto:** produto de crédito consignado em uma instituição financeira de grande porte

**Meu papel:** condução ponta a ponta, da modelagem dos dados até a entrega do produto analítico final

**Stack:** SQL · Python (pandas, matplotlib) · Databricks · HTML · automação de distribuição por e-mail

> **Nota sobre os dados:** por confidencialidade, generalizei o nome da instituição, os nomes de sistemas e scripts internos, e os valores absolutos apresentados aqui. A estrutura do problema, a arquitetura da solução e a ordem de grandeza dos resultados refletem fielmente o projeto real.

## O produto e o problema de negócio

Pendência Contábil é uma das principais métricas de áreas que prestam serviços contábeis. Caracterizada por lançamentos em aberto, diferenças de conciliação entre sistemas e erros operacionais diversos, a criticidade de uma pendência cresce conforme o seu tempo em aberto (aging). Quando não regularizados, esses saldos passam a integrar as contas do balanço patrimonial, distorcendo o ativo e o passivo da instituição. Esse cenário gera riscos regulatórios que afetam a integridade das demonstrações financeiras, riscos financeiros por possíveis ajustes de receita ou despesa, riscos operacionais que sinalizam falhas estruturais nos processos, e riscos de imagem e perdas financeiras diretamente no relacionamento com os clientes.

Crédito consignado é uma modalidade de empréstimo em que a parcela é descontada direto na folha de pagamento do tomador, mediante convênio entre a instituição financeira e o empregador. É um produto de menor risco justamente por causa desse desconto automático, e por isso vem crescendo de forma consistente nos últimos anos.

O problema é que cada empregador conveniado tem seu próprio sistema de folha, seu próprio layout de arquivo e suas próprias regras de repasse. Isso significa que a complexidade de conciliar o que foi contratado com o que foi efetivamente repassado cresce junto com a base de convênios, às vezes até mais rápido do que a carteira em si. Quando os valores não fecham, nasce uma **pendência contábil**: um lançamento em aberto ou uma diferença entre sistemas que precisa ser investigada, e que, se não for tratada, distorce o balanço e carrega risco regulatório e operacional.

## Como o problema aparecia no dia a dia

Antes deste projeto, a análise dessas pendências era fragmentada e reativa:

- As informações estavam espalhadas entre contabilidade, sistema de conciliação, portal do produto e bases de crédito
- As unidades de negócio dependiam de analistas para qualquer leitura executiva: prints, planilhas manuais e e-mails pontuais
- Não havia separação clara entre problema recente (fluxo) e problema estrutural acumulado (estoque)
- O conhecimento sobre causas e padrões ficava concentrado em quem produzia a análise, não documentado nem comparável ao longo do tempo

O desafio central não era falta de dado. Era falta de uma estrutura analítica que transformasse volume operacional em direcionamento de decisão.

## A solução: um pipeline de quatro etapas

Desenhei um fluxo único e automatizado, pensado pra simplicidade operacional e reprodutibilidade:

1. **Carga e modelagem (SQL)** — um script consolida e enriquece dados contábeis, operacionais e de crédito numa base analítica única, classificando cada pendência por aging, origem, tipo de diferença e forma de repasse.
2. **Exploração e cálculo de indicadores (Python)** — um notebook gera as séries temporais, os comparativos mês a mês e as visualizações, usando pandas e matplotlib.
3. **Montagem do reporte (HTML)** — um template estrutura o conteúdo em leitura progressiva: do resumo executivo até o detalhe por causa e por parceiro conveniado, com gráficos incorporados como imagem.
4. **Distribuição automatizada** — o reporte é enviado por e-mail, segmentado por unidade de negócio, com controle de execução pra evitar reprocessamento e log de status.

Toda a base analítica roda em Databricks, o que garante escala e permite reaproveitar a mesma estrutura pra novos cortes no futuro.

## Os indicadores que estruturam a análise

Em vez de simplesmente listar pendências, estruturei a leitura em cinco eixos que respondem às perguntas reais de quem toma decisão:

- **Tempo (aging)** — separar pendências recentes (até 30 dias, sinal de problema operacional pontual) de estoque antigo (acima de 30 dias, sinal de acúmulo estrutural)
- **Causa** — quais tipos de diferença mais concentram volume
- **Responsabilidade** — quais parceiros conveniados mais geram pendência, cruzando com o tipo de diferença
- **Evolução** — variação mês a mês, destacando quem mais contribuiu pra alta ou queda
- **Automação** — quanto da carteira já usa o canal digital de repasse versus processo manual

## Resultados

- As pendências do produto com mais de 30 dias em aberto caíram por volta de **70%** em cerca de seis meses após a implantação do reporte
- A participação do produto no total de pendências da instituição caiu de algo próximo de **40% para menos de 20%**, deixando de ser o principal ofensor
- A satisfação das unidades atendidas, medida por pesquisa interna, saiu de uma faixa de **40 pontos para acima de 60**, superando a meta estabelecida

*(valores aproximados, preservando a ordem de grandeza dos resultados reais)*

## O que as unidades atendidas relataram

Os retornos qualitativos mostraram que o material passou de simples insumo informativo pra instrumento de gestão em três níveis:

- **Operacional** — o reporte deu visibilidade a uma desorganização antes difusa, permitindo redistribuir responsabilidades dentro do time e aumentar a recorrência da análise
- **Tático** — em contextos onde já havia conhecimento prático do processo, o reporte funcionou como validador analítico, trazendo segurança pra definir metas mais consistentes de redução
- **Estratégico** — a clareza dos dados viabilizou ação direta na causa raiz, incluindo tratativas com parceiros externos pra ajuste de layout de arquivo, e motivou até uma revisão de como a atividade de conciliação era enquadrada organizacionalmente

## Stack técnica

`SQL` `Python (pandas, matplotlib)` `Databricks` `HTML` `Automação de e-mail`
