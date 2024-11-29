---
layout: post
author: Leandro Pessoa
---

# Deploy de Software: Três Estratégias para Alta Disponibilidade e Segurança

Dando continuidade à nossa série sobre estratégias de deploy de software, vamos explorar uma nova abordagem que garante disponibilidade e confiabilidade em sistemas críticos.

Se você chegou agora, no primeiro post discutimos as técnicas de [Blue-Green Deployment](https://leandropsouza1.github.io/devaneios/Deploy-de-Software-Blue-Green). Mas não se preocupe, se você não leu o [primeiro post](https://leandropsouza1.github.io/devaneios/Deploy-de-Software-Blue-Green), você pode acessar o [post anterior](https://leandropsouza1.github.io/devaneios/Deploy-de-Software-Blue-Green) para mais detalhes ou acompanhar este diretamente, já que apresentaremos novos conceitos complementares.

A maneira como novas funcionalidades são integradas ao sistema não apenas impacta a experiência dos usuários, mas também influencia a reputação do software no mercado. Por isso, neste segundo post, vamos falar sobre Feature Flags e aprofundar o tema, explorando estratégias ainda mais granulares e seguras para a liberação de funcionalidades.

Acompanhe e descubra como levar sua entrega de software ao próximo nível! 🚀

---

## Feature Flag

A técnica Feature Flag Deployment (ou feature toggle) oferece uma abordagem poderosa para gerenciar a disponibilização de novas funcionalidades em software, permitindo que você as desative ou ative dinamicamente, sem a necessidade de novas implantações. Essa técnica proporciona grande flexibilidade e controle sobre o processo de implantação.

## Como Funcionam os Feature Flag?

**1. Pontos de Ativação (Toggle Points)**: Pontos específicos no código-fonte onde a funcionalidade é ativada ou desativada por um Feature Flag.

**2. Roteador de Ativação (Toggle Router)**: Componente responsável por tomar a decisão de ativar ou desativar a funcionalidade, com base em uma configuração.

**3. Configuração de Ativação (Toggle Configuration)**: Define o estado (ativo ou inativo) de cada Feature Flag.

![Feature Toggle](https://martinfowler.com/bliki/images/featureToggle/featureToggle.png)

**Exemplo Prático**

Imagine que você está desenvolvendo uma nova funcionalidade de descontos para um site de e-commerce, que deve ser liberada exatamente no dia 24/12, às 23h59. Com o uso de Feature Flags, você pode implantar essa funcionalidade em produção antecipadamente, mantendo-a desativada até o momento exato da ativação. Dessa forma, evita-se a necessidade de realizar uma nova publicação do site na data e horário especificados.

**Implementação**

No código-fonte, você define um Toggle Point onde a funcionalidade de recomendações é executada:

```
if (featureDecisions.recomendacoesAtivadas()) {

  // Exibir recomendações personalizadas

} else {

  // Exibir conteúdo padrão

}
```

O Toggle Router, por sua vez, consulta a Toggle Configuration para determinar se a funcionalidade deve ser ativada.

## Vantagens

**Implantações Contínuas e Trunk-Based Development**: Permite que equipes de desenvolvimento trabalhem em novas funcionalidades em branches de desenvolvimento principais, sem impactar o ambiente de produção.

**Teste A/B e Experimentação**: Facilita a realização de testes A/B, permitindo comparar o desempenho de diferentes versões de uma funcionalidade com grupos de usuários distintos.

**Controle Operacional**: Fornece aos operadores a capacidade de ativar ou desativar funcionalidades em tempo real, em resposta a problemas de performance ou outras situações.

## Desvantagens

**- Complexidade Adicional no Código**: A introdução de Feature Flag adiciona complexidade ao código-fonte, exigindo atenção para manter a legibilidade e a manutenibilidade.

**- Sobrecarga de Testes**: É crucial testar todas as combinações possíveis de Feature Flag ativos e inativos, o que pode aumentar significativamente o esforço de testes.

**- Gerenciamento de Feature Flag**: Um grande número de Feature Flag em produção pode se tornar difícil de gerenciar. É importante ter uma estratégia para desativar e remover Feature Flag obsoletos.

**- Liberação Gradual e Canary Releases**: Permite lançar novas funcionalidades para um subconjunto de usuários, como em um Canary Release, para testar a aceitação e o impacto antes da liberação geral.

⚠️ **Feature Flag devem ser a última opção** ⚠️

A Feature Flag é uma técnica útil e amplamente utilizada por muitas equipes. No entanto, elas devem ser sua última escolha ao colocar funcionalidades em produção.

Sua primeira opção deve ser dividir a funcionalidade em partes menores, de modo que você possa introduzir essas partes no produto de forma segura. Os benefícios dessa abordagem são os mesmos de qualquer estratégia baseada em lançamentos pequenos e frequentes: você reduz o risco de problemas e obtém feedback valioso sobre como os usuários realmente utilizam a funcionalidade, o que melhora os ajustes e aprimoramentos futuros.

Se for absolutamente necessário ocultar uma funcionalidade parcialmente desenvolvida, a melhor abordagem é usar [Keystone Interface](https://martinfowler.com/bliki/KeystoneInterface.html): construa toda a funcionalidade, exceto o ponto de entrada na interface do usuário (UI), e adicione essa parte final em um único ciclo de lançamento. Dessa forma, o código não relacionado à interface já estará totalmente integrado ao restante do sistema, mas nada será visível ou usado até que o último elemento seja adicionado no final.

## Cuidados Essenciais

**- Implementação com Padrões de Design**: Utilizar padrões de design, como Strategy Pattern, para organizar o código e facilitar a manutenção.

**- Documente suas flags**: Mantenha um registro claro de todas as Feature Flag, quem as utiliza e quando devem ser removidas.

**- Automatize a limpeza**: Estabeleça processos para remover flags desnecessárias após o lançamento completo de uma funcionalidade.

**- Monitore continuamente**: Acompanhe métricas de uso, performance e impacto das funcionalidades ativadas.

**- Defina critérios claros de uso**: Utilize Feature Flag apenas quando necessário, para evitar acúmulo de complexidade no código.

Somente se você não puder realizar lançamentos pequenos ou não puder utilizar a abordagem de [Keystone Interface](https://martinfowler.com/bliki/KeystoneInterface.html), deve considerar o uso de flags de lançamento.

## Conclusão

O Feature Flag Deployment é uma técnica valiosa para equipes que buscam maior flexibilidade e controle no processo de implantação de software. A técnica permite lançar novas funcionalidades com segurança, realizar testes A/B, controlar a exposição de funcionalidades e responder rapidamente a problemas em produção. No entanto, é importante estar ciente das desvantagens e adotar práticas para mitigar a complexidade adicional e a sobrecarga de testes, garantindo que a implementação de Feature Flag seja eficaz e sustentável.

Fonte: [Feature Flag](https://martinfowler.com/bliki/FeatureFlag.html) do site [Martin Fowler](https://martinfowler.com/).

#FeatureFlags #DesenvolvimentoÁgil #DevOps #Inovação #GestãoDeRiscos #Tecnologia
