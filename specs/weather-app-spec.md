# Especificação do Weather App

## Overview

Aplicação web responsiva, em pt-BR, para consultar o clima atual e a previsão
de cinco dias de uma cidade. A pessoa pesquisa e seleciona uma localidade,
consulta as condições e pode alternar as temperaturas entre Celsius e
Fahrenheit. A fonte de dados definida no discovery é Open-Meteo, sem exigir uma
chave de API. A versão inicial não requer autenticação nem persistência de
dados no servidor.

O objetivo é permitir uma consulta rápida e legível, inclusive em dispositivos
móveis, deixando claros os estados de carregamento, ausência de resultados e
falha recuperável.

## Functional Requirements

### RF1 — Buscar e selecionar cidade

O sistema deve permitir pesquisar uma cidade pelo nome e selecionar a
localidade desejada. Quando houver mais de uma correspondência, deve apresentar
contexto suficiente para distinguir os resultados. O conjunto exato de campos
de contexto é uma pergunta aberta. A consulta meteorológica só começa após uma
localidade ser selecionada.

### RF2 — Consultar clima atual

Depois da seleção de uma localidade, o sistema deve exibir as condições atuais
associadas a ela. Os campos meteorológicos obrigatórios ainda precisam ser
definidos. A interface deve identificar a localidade e não pode apresentar como
atuais dados de outra localidade.

### RF3 — Consultar previsão de cinco dias

Para a localidade selecionada, o sistema deve exibir cinco dias de previsão:
hoje e os quatro dias seguintes. O fuso horário que delimita os dias e os
campos de cada dia ainda precisam ser confirmados. A regra proposta é usar o
fuso horário local da localidade selecionada; essa regra precisa de aprovação.

### RF4 — Alternar unidade de temperatura

O sistema deve permitir alternar entre Celsius e Fahrenheit e atualizar todos
os valores de temperatura exibidos. A unidade inicial deve ser Celsius. Regras
de arredondamento e persistência da preferência ainda precisam ser definidas.
A alternância afeta temperaturas, não define conversão de outras grandezas, e
deve manter a mesma localidade e os mesmos dados meteorológicos subjacentes.

### RF5 — Comunicar estados da consulta

O sistema deve comunicar carregamento, ausência de resultados e falha ao
consultar dados. Para falhas recuperáveis, deve oferecer uma ação de nova
tentativa e não deixar a consulta sem feedback. Ausência de resultados não deve
ser apresentada como falha da fonte de dados.

## User Stories

- **US1 — Busca:** Como pessoa que consulta o tempo, quero buscar e selecionar
  uma cidade para ver as condições do local correto. (RF1, RF2)
- **US2 — Planejamento:** Como pessoa que planeja atividades para os próximos
  dias, quero consultar a previsão de cinco dias para escolher quando realizá-las.
  (RF3)
- **US3 — Unidade:** Como pessoa habituada a Fahrenheit, quero alternar a
  unidade das temperaturas para interpretá-las no formato que conheço. (RF4)
- **US4 — Uso móvel:** Como pessoa que consulta o tempo pelo celular, quero ler
  as condições e a previsão sem perder conteúdo para planejar em qualquer lugar.
  (RF2, RF3; RNF1, RNF2)
- **US5 — Recuperação:** Como pessoa cuja consulta falhou, quero entender o
  problema e tentar novamente para obter os dados sem reiniciar o fluxo. (RF5)

As personas são descrições de trabalho derivadas do briefing e das necessidades
citadas no discovery; não foram validadas com usuários. A prioridade e as
características dos segmentos continuam em aberto.

## Acceptance Criteria

- **AC-RF1.1 — Busca e seleção**
  - Given que a pessoa informa um nome de cidade válido
  - When a busca termina com correspondências
  - Then o sistema apresenta as localidades encontradas com contexto e permite
    selecionar uma delas.
- **AC-RF1.2 — Desambiguação**
  - Given que a busca retorna localidades com o mesmo nome
  - When os resultados são apresentados
  - Then cada resultado inclui informação que permite distingui-lo dos demais.
- **AC-RF1.3 — Busca sem resultados**
  - Given que a pessoa envia um termo de busca e o geocoding não encontra
    correspondências
  - When a busca termina
  - Then o sistema informa que nenhum local foi encontrado, permite editar o
    termo e não inicia uma consulta meteorológica.
- **AC-RF1.4 — Entrada vazia**
  - Given que o campo de busca está vazio ou contém somente espaços
  - When a pessoa tenta iniciar a busca
  - Then o sistema não envia a consulta e informa que é necessário digitar uma
    cidade.
- **AC-RF2 — Condições atuais**
  - Given que uma localidade foi selecionada e a fonte retorna dados atuais
  - When a consulta termina com sucesso
  - Then o sistema apresenta as condições atuais associadas à localidade
    selecionada e identifica essa localidade.
- **AC-RF2.1 — Dados parciais**
  - Given que a fonte retorna uma resposta válida com alguns campos ausentes
  - When as condições são apresentadas
  - Then os campos disponíveis são exibidos e os ausentes são identificados
    como indisponíveis, sem substituição por zero.
- **AC-RF3 — Cinco dias**
  - Given que uma localidade foi selecionada e há dados de previsão disponíveis
  - When a previsão é apresentada
  - Then são exibidos exatamente cinco dias consecutivos, começando por hoje e
    incluindo os quatro dias seguintes no fuso horário local da localidade,
    associados à localidade selecionada.
- **AC-RF4.1 — Unidade inicial**
  - Given que a pessoa abre uma consulta sem uma preferência de unidade definida
  - When as temperaturas são apresentadas
  - Then elas são identificadas em Celsius.
- **AC-RF4.2 — Alternância de unidade**
  - Given que há temperaturas apresentadas em uma unidade
  - When a pessoa seleciona a outra unidade
  - Then todas as temperaturas visíveis passam a ser apresentadas na unidade
    selecionada, com rótulos coerentes, sem alterar a localidade consultada.
- **AC-RF5.1 — Carregamento e falha**
  - Given que uma consulta de cidade ou dados meteorológicos está em andamento
  - When a resposta ainda não foi recebida
  - Then o sistema informa que a consulta está carregando.
- **AC-RF5.2 — Recuperação**
  - Given que uma consulta falha por uma condição recuperável
  - When o sistema informa a falha
  - Then ele apresenta uma ação de nova tentativa e, ao acioná-la, inicia nova
    consulta para a mesma localidade ou termo de busca.
- **AC-RF5.3 — Falha não recuperável**
  - Given que uma consulta falha por uma condição não recuperável
  - When o sistema informa a falha
  - Then o carregamento termina e a mensagem não oferece uma nova tentativa
    que não possa ser executada.

## Non-Functional Requirements

- **RNF1 — Responsividade:** suportar viewports de 320 px a 1440 px nos fluxos
  principais, sem rolagem horizontal nem perda de conteúdo ou funcionalidade.
- **RNF2 — Usabilidade:** identificar a localidade selecionada e organizar
  condições atuais e previsão em uma hierarquia legível para consulta rápida.
- **RNF3 — Acessibilidade:** atender WCAG 2.2 nível AA em todos os fluxos
  principais definidos, incluindo operação por teclado, nomes acessíveis para
  controles e contraste conforme os critérios do padrão.
- **RNF4 — Resiliência:** em falha de rede ou indisponibilidade da fonte,
  apresentar um estado compreensível e manter a interface utilizável, sem
  consulta indefinida.
- **RNF5 — Desempenho (proposta a validar):** em uma rede de referência ainda
  a definir, pelo menos 95% das consultas devem exibir os dados após o envio da
  busca em até 3 segundos quando a fonte externa responder normalmente. Registrar
  separadamente a latência da fonte externa para atribuir corretamente atrasos.
- **RNF6 — Disponibilidade (proposta a validar):** disponibilidade mensal de
  pelo menos 99,5% para a aplicação e fluxos principais, excluindo falhas da
  fonte externa; monitorar a dependência separadamente.

## Edge Cases

- **EC1 — Entrada vazia:** não enviar uma consulta; informar que é necessário
  digitar o nome de uma cidade e manter o campo disponível para correção.
- **EC2 — Caracteres especiais:** tratar a entrada como texto, sem quebrar a
  interface; se não houver correspondência, informar ausência de resultados e
  permitir editar a busca.
- **EC3 — Cidade inexistente:** quando não houver correspondência, informar que
  nenhum local foi encontrado e permitir alterar o termo de busca.
- **EC4 — Geocoding sem resultados:** apresentar o mesmo estado de ausência de
  resultados de EC3; não iniciar uma consulta meteorológica sem localidade
  selecionada.
- **EC5 — Cidades homônimas:** mostrar contexto diferenciador em cada resultado
  e aguardar a seleção explícita antes de carregar o clima.
- **EC6 — Falha da API ou da rede:** interromper o estado de carregamento,
  explicar que os dados não puderam ser carregados e oferecer nova tentativa
  quando a falha for recuperável. Não apresentar uma falha como ausência de
  resultados.
- **EC7 — Timeout:** ao exceder o limite de espera configurado, encerrar o
  carregamento, comunicar a falha como recuperável e oferecer nova tentativa.
  O limite de espera ainda precisa ser decidido.
- **EC8 — Resposta parcial:** apresentar apenas dados válidos recebidos e
  identificar campos ausentes como indisponíveis; não substituir dados ausentes
  por zero nem descartar dados válidos. Os campos obrigatórios ainda precisam
  ser definidos.

## Assumptions

- A v1 é uma aplicação web responsiva, acessada por navegador e dependente de
  conexão com a internet.
- A interface é apresentada em pt-BR.
- Open-Meteo é a fonte de dados escolhida; sua adequação, cobertura e termos
  ainda precisam ser validados.
- Cinco dias significa hoje e os quatro dias seguintes.
- Celsius é a unidade inicial.
- A v1 não tem autenticação nem persistência de dados no servidor.
- Não há localização automática definida; a pessoa informa a cidade pela busca.

## Risks

| Risco | Impacto | Mitigação |
| --- | --- | --- |
| Cidades com nomes iguais ou ambíguos | Seleção da localidade errada | Exibir contexto que diferencie os resultados. |
| Indisponibilidade, lentidão ou limite da fonte | Consulta incompleta ou indisponível | Comunicar falhas, permitir nova tentativa e validar a dependência. |
| Conexão instável em dispositivo móvel | Falta de feedback ou consulta interrompida | Exibir estados de carregamento e erro compreensíveis. |
| Conversão ou rotulagem incorreta de temperatura | Interpretação errada | Manter a unidade visível e verificar critérios de conversão. |
| Interpretação inconsistente dos cinco dias | Datas diferentes das esperadas | Definir hoje mais quatro dias; ainda definir o fuso horário. |

## Out of Scope

O escopo abaixo é uma proposta para a v1 e depende da validação das perguntas
abertas correspondentes.

- Autenticação, contas de usuário e persistência de dados no servidor.
- Localização automática do dispositivo.
- Funcionalidades não estabelecidas no briefing, como alertas, notificações,
  mapas, widgets, compartilhamento e histórico de cidades.
- Aplicativos nativos. A entrega definida provisoriamente é uma aplicação web.

## Open Questions

### Prontidão para desenvolvimento (P3.7)

**Reavaliação: a spec ainda não está pronta para desenvolvimento sem novas
perguntas.** O discovery atualizado propõe três personas, mas elas não foram
validadas com usuários nem priorizadas. Também continuam sem decisão detalhes
que alteram os comportamentos e os critérios de aceite.

Para aprovar a spec como contrato de desenvolvimento, ainda é necessário:

- Validar e priorizar o público e as personas; confirmar plataforma e localidades
  suportadas e definir o fluxo de busca e desambiguação.
- Definir os campos obrigatórios do clima atual e da previsão, o fuso e formato
  das datas, a atualização e idade máxima dos dados e a regra para respostas
  parciais.
- Aprovar o comportamento de unidades, arredondamento e persistência local;
  confirmar cobertura, termos, atribuição e limites da Open-Meteo.
- Aprovar critérios mensuráveis de desempenho e disponibilidade, matriz de
  dispositivos e navegadores, tratamento de falhas e requisitos de segurança,
  privacidade e operação.
- Aprovar o escopo da v1, as exclusões de **Out of Scope**, os critérios de
  aceite e os responsáveis por produto e operação.

As perguntas abertas abaixo detalham esses itens. **Out of Scope** está
preenchido como proposta, mas não pode ser considerado final até sua aprovação.

- Quem são os usuários prioritários? As descrições das stories são personas
  provisórias e precisam ser validadas.
- Quais campos compõem clima atual e previsão diária, e como tratar campos
  indisponíveis em uma resposta parcial?
- Em qual fuso horário começa cada dia da previsão?
- Quais informações de contexto devem distinguir cidades homônimas?
- Quais localidades, países, idiomas de busca e alfabetos devem ser cobertos?
- A busca ocorre após envio ou sugere resultados enquanto a pessoa digita? Deve
  tolerar erros de digitação e nomes alternativos?
- A escolha de unidade e cidade persiste localmente entre visitas? Como
  arredondar temperaturas convertidas?
- Quais formatos de data e hora devem ser usados?
- Qual é o limite de timeout, política de nova tentativa, idade máxima dos
  dados e comportamento de cache?
- Os termos, a cobertura, a precisão e a disponibilidade da Open-Meteo atendem
  ao produto? Há exigência de atribuição da fonte?
- Quais navegadores, sistemas operacionais e dispositivos devem ser suportados?
- As metas propostas de desempenho e disponibilidade são aprovadas? Qual é a
  rede de referência e como medir cada meta?
- Quais dados podem ser armazenados no dispositivo? Haverá analytics, cookies,
  consentimento ou requisitos de retenção e privacidade?
- Quais proteções contra abuso, requisitos de segurança, monitoramento e
  processo de incidentes são necessários?
- Existem diretrizes de marca, conteúdo e tom para a interface?
- Quais são prazo, orçamento, responsável operacional, critérios de aceite da
  v1, aprovador e métricas de sucesso?
- Alertas, notificações, mapas, widgets e compartilhamento estão explicitamente
  adiados ou algum deles é requisito da v1?

## Traceability

| User Story | Acceptance Criteria | Requisitos não funcionais relevantes |
| --- | --- | --- |
| US1 — Busca | AC-RF1.1, AC-RF1.2, AC-RF1.3, AC-RF1.4, AC-RF2, AC-RF2.1 | RNF1, RNF2, RNF3, RNF4, RNF5, RNF6 |
| US2 — Planejamento | AC-RF3 | RNF1, RNF2, RNF3, RNF4, RNF5, RNF6 |
| US3 — Unidade | AC-RF4.1, AC-RF4.2 | RNF1, RNF2, RNF3 |
| US4 — Uso móvel | AC-RF2, AC-RF2.1, AC-RF3 | RNF1, RNF2, RNF3, RNF4, RNF5, RNF6 |
| US5 — Recuperação | AC-RF5.1, AC-RF5.2, AC-RF5.3 | RNF3, RNF4, RNF5, RNF6 |

Verificação P3.8: todas as cinco User Stories têm critérios de aceite e
requisitos não funcionais associados; os critérios AC-RF1.1 a AC-RF5.3 estão
representados na tabela conforme os requisitos funcionais relacionados.