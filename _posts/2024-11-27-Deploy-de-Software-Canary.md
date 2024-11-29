---
layout: post
author: Leandro Pessoa
tag: ConteudoTecnico GerenciaDeConfiguração Deploy DevOps Canary
---

# Deploy de Software: Três Estratégias para Alta Disponibilidade e Segurança

Dando continuidade à nossa série sobre estratégias de deploy de software, vamos explorar uma nova abordagem que garante disponibilidade e confiabilidade em sistemas críticos.

Se você chegou agora, nos primeiros dois posts discutimos as técnicas de [Blue-Green Deployment](https://leandropsouza1.github.io/devaneios/Deploy-de-Software-Blue-Green) e [Feature Flag](https://leandropsouza1.github.io/devaneios/Deploy-de-Software-Feature-Flag). Mas não se preocupe, se você não leu os primeiros posts, você pode acessá-los para mais detalhes ou acompanhar este diretamente, já que apresentaremos novos conceitos complementares.

A maneira como novas funcionalidades são integradas ao sistema não apenas impacta a experiência dos usuários, mas também influencia a reputação do software no mercado. Por isso, neste terceiro post, vamos falar sobre Canary Release e aprofundar o tema, explorando estratégias ainda mais granulares e seguras para a liberação de funcionalidades.

Acompanhe e descubra como levar sua entrega de software ao próximo nível! 🚀

---

## Canary Release

A técnica Canary Release é uma abordagem poderosa para disponibilizar novas versões de software em produção, minimizando os riscos e o impacto de potenciais problemas. O nome, inspirado na prática de mineiros que usavam canários para detectar gases tóxicos em minas de carvão, reflete a essência da técnica: expor um pequeno grupo de usuários à nova versão, atuando como "canários", antes de liberá-la para todos.

## Como funciona o Canary Release?

**1. Implantação em Subconjunto da Infraestrutura**: A nova versão do software é implantada em uma parte limitada da infraestrutura de produção, sem que nenhum usuário seja direcionado para ela inicialmente.

![Canary Release](https://martinfowler.com/bliki/images/canaryRelease/canary-release-1.png)

**2. Liberação para o Grupo de "Canários"**: Um pequeno grupo de usuários é selecionado para utilizar a nova versão. A escolha dos "canários" pode ser feita de diversas maneiras:

- **Amostragem Aleatória**: Usuários escolhidos aleatoriamente.

- **Usuários Internos**: Colaboradores da empresa testam a nova versão antes da liberação externa (A Meta utilizava essa técnica para lançar novas funcionalidades para o aplicativo do Facebook).

- **Segmentação por Perfil**: Usuários selecionados com base em seus perfis e dados demográficos.

![Canary Release](https://martinfowler.com/bliki/images/canaryRelease/canary-release-2.png)

**3. Monitoramento Contínuo e Coleta de Métricas**: O comportamento da nova versão é monitorado de perto, coletando métricas cruciais, como:

**- Performance da aplicação**: Tempos de resposta, uso de recursos, etc.
**- Ocorrência de erros e exceções**: Verificar com ajuda de softwares de monitoramento (Dynatrace, Datadog, Sentry, etc) a existencia de novos erros e/ou exceções.

**- Feedback dos usuários "canários"**: Conversar com os usuários e colher o feedback sobre as novas features que foram implementadas, desempenho do sistema, etc.

**4. Expansão Gradual da Liberação**: Se a nova versão se comportar como esperado, a liberação é expandida gradualmente para um número maior de usuários. A expansão pode ser feita em etapas controladas, sempre acompanhando as métricas e o feedback.

**5. Liberação Total para Todos os Usuários**: Quando a equipe estiver confiante na estabilidade da nova versão, com base nos dados coletados e na ausência de problemas significativos, a liberação é finalmente disponibilizada para todos os usuários.

![Canary Release](https://martinfowler.com/bliki/images/canaryRelease/canary-release-3.png)

## Vantagens

**- Redução de Riscos**: A exposição gradual e controlada da nova versão permite detectar problemas precocemente, antes que afetem um grande número de usuários.

**- Testes em Ambiente Real**: Permite realizar testes de capacidade e performance da nova versão em um ambiente de produção real, com tráfego real de usuários.

**- Feedback Valioso dos Usuários**: O feedback dos usuários "canários" é extremamente valioso para identificar problemas de usabilidade, bugs e áreas de melhoria antes da liberação geral.

**- Rollback Simplificado**: Se forem detectados problemas durante a fase de liberação gradual, o rollback é simples e rápido: basta redirecionar os usuários afetados de volta para a versão anterior do software. Essa capacidade de reverter a mudança rapidamente minimiza o impacto para os usuários e garante a estabilidade do sistema.

**- Implementação de Testes A/B**: A estrutura do Canary Release pode ser utilizada para implementar testes A/B, comparando o desempenho de diferentes versões do software.

**Atenção:** Apesar de os lançamentos canários e os testes A/B compartilharem semelhanças na implementação técnica, principalmente no que diz respeito ao direcionamento controlado de tráfego, é crucial evitar confundi-los. Os lançamentos canários são projetados para detectar problemas e regressões em uma nova versão de software, funcionando como um mecanismo de segurança durante a implantação gradual. Já os testes A/B servem para testar uma hipótese, comparando diferentes implementações de uma funcionalidade para determinar qual gera melhores resultados.

Utilizar um lançamento canário para realizar testes A/B pode interferir nos resultados, pois a análise das métricas de negócio, chave para os testes A/B, seria prejudicada por eventuais problemas de desempenho ou instabilidades da nova versão, que são o foco do monitoramento em um lançamento canário.

Além disso, a diferença nos tempos de execução também torna a combinação dessas abordagens inadequada. Enquanto um lançamento canário busca ser concluído em minutos ou horas para garantir agilidade na implantação, a coleta de dados para testes A/B, visando significância estatística, pode levar dias.

## Desvantagens

**- Complexidade do Gerenciamento de Versões**: A necessidade de gerenciar múltiplas versões do software simultaneamente aumenta a complexidade da implantação e exige organização.

**- Dificuldade em Aplicações Desktop/Mobile**: Em softwares distribuídos para desktops ou dispositivos móveis, o controle sobre a atualização para a nova versão é menor, o que pode dificultar a implementação do Canary Release.

**- Atenção às Mudanças no Banco de Dados**: Mudanças no banco de dados devem ser gerenciadas com cuidado, garantindo a compatibilidade entre as diferentes versões do software durante a liberação gradual. É recomendável utilizar técnicas como [ParallelChange](https://martinfowler.com/bliki/ParallelChange.html) para que o banco de dados suporte ambas as versões durante a transição.

**- Monitoramento e Métricas Eficazes**: A implementação de um sistema de monitoramento robusto e a definição de métricas relevantes são essenciais para o sucesso do Canary Release. A equipe precisa acompanhar de perto os indicadores chave para detectar problemas rapidamente e tomar decisões informadas sobre a progressão da liberação.

## Cuidados Essenciais

**- Planeje bem a segmentação**: Escolha o subconjunto de usuários com base em critérios representativos do comportamento geral.

**- Tenha automação e monitoramento**: Use ferramentas que detectem erros e monitorem métricas de desempenho em tempo real.

**- Planeje reversões**: Estruture o processo para que o rollback, se necessário, seja rápido e seguro.

**- Comunique-se com os usuários**: Informe os usuários do grupo inicial, especialmente se houver risco de interrupções ou mudanças perceptíveis.

## Considerações Finais

O Canary Release é uma técnica valiosa para equipes que buscam implantar novas versões de software de forma segura, controlada e com o mínimo de interrupção para os usuários. A técnica permite um processo de liberação gradual, com feedback constante e a possibilidade de rollback rápido, contribuindo para um ambiente de produção mais estável e confiável.

**Fonte**: [Canary Release](https://martinfowler.com/bliki/CanaryRelease.html) do site [Martin Fowler](https://martinfowler.com/).

#DevOps #CanaryRelease #GestãoDeRisco #EntregaContínua #Tecnologia #Inovação
