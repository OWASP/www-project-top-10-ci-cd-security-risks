---

layout: col-sidebar
title: "CICD-SEC-5: Insufficient PBAC (Pipeline-Based Access Controls)"

---
## Definição

Os Hosts que executam os pipelines têm acesso a vários recursos e sistemas dentro e fora do ambiente de execução. Ao executar código malicioso em um pipeline, os adversários aproveitam os riscos insuficientes de PBAC (controles de acesso baseados em pipeline) para abusar da permissão concedida ao pipeline para se mover lateralmente dentro ou fora do sistema CI/CD.

## Descrição

Os pipelines são o coração do CI/CD. Os nós que executam pipelines executam os comandos especificados na configuração do pipeline e, ao fazê-lo, conduzem uma ampla gama de atividades confidenciais:



* Acesso ao código fonte, construção e teste.
* Obtenção de segredos de vários locais, como variáveis de ambiente, cofres, serviços de identidade baseados em nuvem dedicados (como o serviço de metadados da AWS) e outros locais.
* Criar, modificar e implantar artefatos.

PBAC é um termo que se refere ao contexto em que cada pipeline - e cada etapa dentro desse pipeline - está sendo executado. Dada a natureza altamente sensível e crítica de cada pipeline, é imperativo limitar cada pipeline ao conjunto exato de dados e recursos aos quais ele precisa acessar. Idealmente, cada pipeline e cada etapa devem ser restringidos de forma a garantir que, caso um adversário seja capaz de executar código malicioso no contexto do pipeline, a extensão do dano potencial seja mínima.

O PBAC inclui controles relacionados a vários elementos relacionados ao ambiente de execução do pipeline:



* Acesso dentro do ambiente de execução do pipeline: ao código, segredos, variáveis de ambiente e outros pipelines.
* Permissões para o host subjacente e outros nós de pipeline.
* Filtros de entrada e saída para a internet.

## Impacto

Um pedaço de código malicioso que pode ser executado no contexto do nó de execução do pipeline tem todas as permissões do estágio do pipeline em que ele está sendo executado. Ele pode acessar segredos, acessar o host subjacente e conectar-se a qualquer um dos sistemas do pipeline em questão tem acesso. Isso pode levar à exposição de dados confidenciais, movimento lateral dentro do ambiente de CI - potencialmente acessando servidores e sistemas fora do ambiente de CI e implantação de artefatos maliciosos no pipeline, inclusive na produção.

A extensão do dano potencial de um cenário no qual um adversário é capaz de comprometer os nós de execução do pipeline ou injetar código malicioso no processo de Build é determinada pela granularidade do PBAC no ambiente.


## Recomendações



* Não use um nó compartilhado para pipelines com diferentes níveis de sensibilidade/que exijam acesso a diferentes recursos. Os nós compartilhados devem ser usados apenas para pipelines com níveis idênticos de confidencialidade.
* Certifique-se de que os segredos usados em sistemas de CI/CD tenham escopo definido de maneira a permitir que cada pipeline e etapa tenham acesso apenas aos segredos necessários.
* Reverta o nó de execução para seu estado original após cada execução de pipeline.
* Certifique-se de que o usuário do sistema operacional que executa o job do pipeline tenha recebido permissões do sistema operacional no nó de execução de acordo com o princípio do privilégio mínimo.
* Os jobs de pipeline de CI e CD devem ter permissões limitadas no controller node. Quando necessário, execute os jobs de pipeline em um nó separado e dedicado.
* Certifique-se de que o nó de execução esteja devidamente atualizado e com os patches de correção aplicados.
* Certifique-se de que a segmentação de rede no ambiente em que o Job está sendo executado esteja configurada para permitir que o nó de execução acesse apenas os recursos necessários dentro da rede. Sempre que possível, evite conceder acesso ilimitado à Internet a esses nós.
* Quando os scripts de instalação estiverem sendo executados como parte da instalação do pacote, certifique-se de que exista um contexto separado para esses scripts, que não tenha acesso a segredos e outros recursos confidenciais disponíveis em outros estágios do processo de compilação.

## Referências



1. Codecov, a popular code coverage tool used in the CI, was compromised and used to steal environment variables from builds.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

2. Amazon, Zillow, Lyft, and Slack NodeJS apps targeted by threat actors using the Dependency Confusion vulnerability. Organizations that were victims of Dependency Confusion attacks had malicious code executed on CI nodes, allowing the adversary to move laterally within the environment and abuse insufficient PBAC.

    [https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/](https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/)

3. A vulnerability found in Teleport’s CI implementation, allowed attackers from the internet to execute a Direct-3PE attack to run a privileged container and escalate to root privilege on the node itself - leading to secret exfiltration, release of malicious artifacts, and access to sensitive systems.

    [https://goteleport.com/blog/hack-via-pull-request/](https://goteleport.com/blog/hack-via-pull-request/)
