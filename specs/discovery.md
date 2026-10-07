# Discovery — Aplicação de previsão do tempo

## Resumo Executivo

- O produto proposto é uma aplicação web responsiva, em pt-BR, para consultar o tempo em cidades escolhidas pela pessoa usuária.
- A v1 prevê busca e seleção de cidade, condições atuais, previsão de hoje mais quatro dias e alternância entre Celsius e Fahrenheit.
- Open-Meteo é a fonte de dados escolhida para o treinamento, sem exigência de API key.
- Os principais riscos são resultados de cidades ambíguas, indisponibilidade ou lentidão da fonte e conexão instável em dispositivos móveis.
- Campos meteorológicos, regras de data, cobertura e metas operacionais ainda precisam de validação antes de aprovar o escopo final.

## Contexto

A empresa precisa de uma aplicação de previsão do tempo para que usuários consultem as condições de uma cidade e planejem os próximos dias. O briefing define busca por cidade, consulta do clima atual, previsão de cinco dias, alternância entre Celsius e Fahrenheit e uso em dispositivos móveis. A experiência deve priorizar acesso rápido e leitura clara das informações, especialmente em telas pequenas.

## Personas Propostas

Estas personas são hipóteses de trabalho derivadas do briefing, não resultados de pesquisa com usuários. Devem ser validadas antes de orientar decisões finais de produto.

| Persona | Objetivo principal | Contexto de uso | Métrica de sucesso pessoal |
| --- | --- | --- | --- |
| Pessoa em rotina diária | Consultar o clima para decidir como se vestir ou se preparar para sair. | Principalmente no celular, antes de sair ou durante deslocamentos; eventualmente no desktop. | Encontra a cidade e identifica as condições atuais sem confundir o local ou a unidade. |
| Pessoa que planeja os próximos dias | Consultar a previsão para escolher quando realizar atividades ou viajar. | Compara informações no desktop ao planejar e pode voltar a consultar pelo celular. | Consegue consultar os cinco dias da cidade desejada e identificar o período relevante. |
| Pessoa habituada a Fahrenheit | Consultar temperaturas na unidade que interpreta com mais facilidade. | Pode usar celular ou desktop; alterna a unidade durante a consulta. | Vê todas as temperaturas na unidade escolhida, com rótulos claros e consistentes. |

## Requisitos Funcionais

- **RF1 — Buscar cidades:** permitir que o usuário pesquise uma cidade pelo nome e selecione o local desejado.
- **RF2 — Consultar clima atual:** exibir as condições atuais da cidade selecionada.
- **RF3 — Consultar previsão:** exibir a previsão para cinco dias da cidade selecionada.
- **RF4 — Alternar unidades:** permitir alternar entre Celsius e Fahrenheit e refletir a unidade escolhida nos dados de temperatura exibidos.
- **RF5 — Informar estados da consulta:** comunicar carregamento, ausência de resultados e falhas ao buscar dados; quando houver falha recuperável, oferecer uma ação para tentar novamente.

## Requisitos Não-Funcionais

- **RNF1 — Responsividade:** suportar larguras de viewport de 320 px a 1440 px nos fluxos principais, sem rolagem horizontal nem perda de conteúdo ou funcionalidade.
- **RNF2 — Usabilidade:** apresentar cidade, condições atuais e previsão de forma legível e fácil de consultar em uma interação rápida.
- **RNF3 — Acessibilidade:** atender ao nível AA da WCAG 2.2 nos fluxos principais, incluindo operação por teclado, nomes acessíveis para controles e contraste mínimo conforme o padrão.
- **RNF4 — Resiliência:** em falhas de rede ou indisponibilidade da fonte meteorológica, manter a interface utilizável e apresentar um estado compreensível, sem deixar a consulta indefinida.
- **RNF5 — Desempenho:** em condições de rede de referência a definir, 95% das consultas devem exibir os dados em até 3 segundos quando a fonte externa responder normalmente; medir separadamente o tempo de resposta da fonte.
- **RNF6 — Disponibilidade:** manter disponibilidade mensal de pelo menos 99,5% para a aplicação e seus fluxos principais, excluindo falhas da fonte meteorológica externa; monitorar a disponibilidade dessa dependência separadamente e informar sua indisponibilidade ao usuário conforme RF5.

> As metas numéricas e a rede de referência são propostas iniciais e precisam ser validadas pelo negócio e pela equipe técnica antes de se tornarem critérios de aceite.

## Riscos

| Risco | Probabilidade | Impacto | Mitigação inicial |
| --- | --- | --- | --- |
| Nomes de cidades repetidos ou ambíguos | Alta | Médio | Exibir informações de contexto, como região e país, para apoiar a seleção. |
| Indisponibilidade, lentidão ou limites da fonte meteorológica | Média | Alto | Tratar erros, permitir nova tentativa e avaliar limites e disponibilidade antes da escolha final da integração. |
| Conexão instável em dispositivos móveis | Média | Médio | Mostrar estados de carregamento e erro claros e evitar bloquear a navegação sem feedback. |
| Conversão ou rotulagem incorreta de unidades | Baixa | Alto | Manter a unidade visível e validar conversões com testes. |
| Previsão de cinco dias interpretada de maneiras diferentes | Média | Médio | Definir explicitamente se o período inclui o dia atual e como as datas serão apresentadas. |

## Autocrítica do Discovery

### O que ainda está vago

- O público prioritário e a tarefa principal não foram validados; as personas acima são apenas hipóteses.
- Ainda não estão definidos os campos do clima atual e de cada dia da previsão, nem se haverá detalhamento horário.
- Fluxo e qualidade da busca, cobertura geográfica, idiomas aceitos e contexto para distinguir cidades homônimas seguem em aberto.
- O fuso horário, formatos de data, atualização dos dados e tratamento de respostas parciais precisam de decisões explícitas.
- As metas de desempenho e disponibilidade são propostas sem rede de referência, método de medição ou aprovação operacional.

### O que pode gerar retrabalho

- Confirmar Open-Meteo sem validar cobertura, precisão, termos, atribuição e limites de uso pode exigir a troca da fonte ou mudanças de produto.
- Escolher campos meteorológicos depois de desenhar a experiência pode alterar a hierarquia e o espaço necessário na interface.
- Adiar definições de busca, localidades suportadas e dados de desambiguação pode levar a refazer a seleção de cidade.
- Não definir fuso, atualização, cache e comportamento em falhas pode produzir datas inesperadas ou apresentar dados desatualizados como atuais.
- As suposições sobre web responsiva, ausência de localização automática e dados não persistidos precisam de confirmação dos responsáveis pelo produto.

### O que falta para avançar com segurança

É possível iniciar uma especificação preliminar, mantendo as hipóteses e perguntas abertas visíveis. Antes de aprová-la como contrato para desenvolvimento, produto e equipe técnica devem decidir ou aceitar explicitamente: público prioritário; campos e granularidade dos dados; fuso e formatação; busca e cobertura; persistência local; adequação e termos da fonte; metas mensuráveis de qualidade; e escopo, critérios de aceite e responsáveis pela operação.

## Perguntas em Aberto

As perguntas abaixo partem exclusivamente do briefing. As respostas podem alterar escopo, arquitetura, custos e critérios de aceite; nenhuma alternativa é considerada decisão até ser validada.

| Pergunta em aberto | Impacto de seguir sem resposta |
| --- | --- |
| Quem são os usuários prioritários e qual tarefa principal o app deve resolver: consulta rápida do dia, planejamento de viagem ou ambas? | A hierarquia das informações, o fluxo principal e as prioridades de produto podem atender mal ao público real. |
| “Aplicação” significa web responsiva, PWA, aplicativos nativos ou uma combinação? | Afeta stack, distribuição, permissões, custo, cronograma e critérios de uso em dispositivos móveis. |
| Quais sistemas operacionais, navegadores e tamanhos de tela precisam ser suportados? | Sem uma matriz de suporte, não é possível definir cobertura de testes nem garantir compatibilidade. |
| A busca deve aceitar texto livre, sugestões enquanto o usuário digita, busca após envio ou uma combinação? Deve tolerar erros de digitação e nomes alternativos? | A experiência, a acessibilidade, a latência percebida e os requisitos da integração de geocoding ficam indefinidos. |
| Como o usuário escolhe entre cidades homônimas: quais dados de contexto devem aparecer (país, estado/região, coordenadas)? | O usuário pode selecionar o local errado e receber uma previsão irrelevante. |
| Quais países, territórios e localidades devem estar cobertos? Há suporte a nomes em idiomas ou alfabetos diferentes? | A cobertura geográfica e linguística pode ser insuficiente para os usuários esperados e limitar a escolha do provedor. |
| Quais campos definem “clima atual” (por exemplo, temperatura, sensação térmica, condição, umidade, vento e precipitação)? Deve haver horário da observação? | O produto pode exibir informação insuficiente, excessiva ou sem contexto temporal, além de afetar layout e custo de dados. |
| Quais campos e nível de detalhe compõem a previsão de cada dia? A previsão é somente diária ou também horária? | Não é possível dimensionar a interface, avaliar a adequação do provedor nem estabelecer critérios de aceite da previsão. |
| Em qual fuso horário os dias da previsão começam e terminam? | Sem uma regra de fuso, datas podem divergir da expectativa local, sobretudo perto da meia-noite e em cidades de outros fusos. |
| A cobertura, precisão, frequência de atualização, licença e os termos da Open-Meteo são adequados ao produto? | A escolha da fonte está definida, mas sua adequação pode não atender aos locais ou à qualidade esperados, ou gerar restrições de uso. |
| Com que frequência os dados devem ser atualizados e qual idade máxima de dado ainda é aceitável? Deve ser exibido quando ocorreu a última atualização? | Dados desatualizados podem ser apresentados como atuais, prejudicando decisões e confiança. |
| A alternância Celsius/Fahrenheit afeta apenas temperatura ou também outras grandezas? Qual unidade usar para vento, pressão e precipitação? | A interface pode misturar convenções ou mostrar conversões inconsistentes para usuários de diferentes regiões. |
| A escolha entre Celsius e Fahrenheit deve persistir localmente entre visitas? Como arredondar os valores convertidos? | A unidade inicial está definida, mas sem regra de persistência e arredondamento a experiência pode variar entre sessões ou exibir valores inesperados. |
| A cidade selecionada deve permanecer no dispositivo ao recarregar ou retornar ao app? São necessários histórico, favoritos ou várias cidades salvas? | A decisão exclui persistência no servidor, mas não define armazenamento local nem funcionalidades de cidades salvas. |
| O app deve pedir localização do dispositivo ou a cidade será sempre informada pela busca? A localização, se usada, é opcional? | Afeta permissões, privacidade, fluxo inicial e comportamento quando o usuário nega acesso ou o dispositivo não fornece localização. |
| Como devem funcionar ausência de resultados, falha da API, limite de uso e perda de conexão? Deve haver nova tentativa, cache ou indicação de dados anteriores? | Os estados de erro e recuperação ficam indefinidos, podendo deixar o usuário sem orientação ou mostrar dados antigos sem aviso. |
| Além da interface em pt-BR, quais formatos de data/hora, calendário e convenções regionais devem ser usados? | O idioma está definido, mas datas, horários e outras convenções podem continuar inadequados ou inconsistentes. |
| Qual padrão e nível de conformidade de acessibilidade são exigidos, e quais tecnologias assistivas fazem parte da validação? | Não há critério objetivo para avaliar acessibilidade; barreiras podem impedir o uso por pessoas com deficiência. |
| Quais rede e dispositivos representam o cenário de desempenho? As metas propostas (95% das consultas em até 3 segundos) são aceitáveis e como medir o tempo da fonte externa? | A meta pode ser inviável ou não representar a experiência real; resultados de testes podem ser incomparáveis ou responsabilizar incorretamente o app pela API. |
| A meta proposta de disponibilidade de 99,5% ao mês é aceitável? Ela cobre somente a aplicação ou também a disponibilidade dos dados meteorológicos? Quais janelas de manutenção são excluídas? | O SLA pode ser interpretado de formas diferentes, e a aplicação pode parecer disponível mesmo sem entregar dados úteis. |
| Quais dados podem ser armazenados localmente (cidade, preferências) e haverá analytics, cookies ou requisitos de consentimento e retenção? | Sem armazenamento no servidor, dados ainda podem ser persistidos no dispositivo ou coletados por analytics, com implicações de privacidade e conformidade. |
| Quais proteções contra abuso da busca são necessárias e quais políticas de segurança se aplicam ao cliente? | A ausência de chave da API reduz o risco de exposição de credenciais, mas não impede abuso do serviço nem define controles de segurança do app. |
| Como o sucesso do produto será medido (por exemplo, consultas concluídas, retorno de usuários, satisfação ou tempo para encontrar uma cidade)? | Sem métricas, não é possível confirmar se a solução atende à necessidade nem orientar prioridades posteriores. |
| Alertas meteorológicos, notificações, mapas, widgets ou compartilhamento fazem parte do escopo inicial ou são explicitamente adiados? | O time pode expandir o escopo sem autorização ou omitir funções que stakeholders consideram essenciais. |
| Existem diretrizes de marca, conteúdo, tom das mensagens ou requisitos de identidade visual? | A interface pode não corresponder à identidade da empresa ou exigir retrabalho de design e conteúdo. |
| Qual é o prazo, orçamento, responsável operacional e processo esperado para incidentes ou suporte? | É difícil escolher uma solução compatível com as restrições, planejar a entrega e definir quem responde por falhas em produção. |
| Quais critérios de aceite definem a entrega de v1 e quem aprova o produto? | O time pode considerar a entrega concluída enquanto stakeholders esperam comportamentos ou qualidade diferentes. |

## Decisões

| Decisão | Justificativa | Perguntas em aberto que resolve |
| --- | --- | --- |
| **Fonte de dados: Open-Meteo, sem API key.** | Evita exigir uma credencial de API para a integração prevista e simplifica a configuração inicial. A cobertura, os termos e a adequação dos dados ainda precisam ser validados. | Define qual fonte será integrada, eliminando a escolha do provedor. Permanecem em aberto a adequação da cobertura, precisão, atualização e termos de uso. |
| **“5 dias” = hoje + os quatro dias seguintes.** | Estabelece um intervalo previsível que inclui a consulta do dia atual e os próximos quatro dias. | Resolve se o dia atual faz parte do período da previsão. Não define o fuso horário usado para delimitar cada dia. |
| **Unidade padrão: Celsius.** | Define uma apresentação inicial única e adequada ao padrão esperado para a interface em pt-BR. | Resolve qual unidade mostrar na primeira consulta. Não define persistência da preferência nem arredondamento das conversões. |
| **Sem autenticação e sem persistência de dados no servidor.** | Mantém a v1 focada em consulta, sem contas ou sincronização entre usuários/dispositivos. | Exclui login/contas e armazenamento de dados do usuário no servidor. Não decide se cidade ou preferências serão salvas localmente. |
| **Idioma da interface: pt-BR.** | Alinha textos e experiência ao público inicial definido para o produto. | Resolve o idioma da interface. Não define formatos de data/hora ou outras convenções regionais. |

## Suposições

- O produto será uma aplicação acessível por navegador, com interface adaptável a dispositivos móveis.
- A consulta depende de conexão com a internet e de uma fonte externa de dados meteorológicos.
- O escopo inicial é de consulta, sem autenticação nem persistência de dados no servidor; armazenamento local continua pendente de definição.
- Não haverá localização automática: o usuário informará a cidade pela busca, salvo decisão posterior.
- Open-Meteo é a fonte de dados decidida; sua cobertura, seus termos e sua adequação ainda precisam ser validados.
- Permanecem pendentes detalhes de conteúdo meteorológico, fuso horário, persistência local das preferências e convenções de data/hora.
