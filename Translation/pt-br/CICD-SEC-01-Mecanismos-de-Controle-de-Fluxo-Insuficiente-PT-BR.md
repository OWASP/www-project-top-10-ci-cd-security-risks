---

layout: col-sidebar
title: "CICD-SEC-1: Mecanismos de Controle de Fluxo Insuficientes"
redirect_from:
  - www-project-top-10-ci-cd-security-risks/risks/CICD-SEC-01-Insufficient-Flow-Control-Mechanisms.md

---
## Definição

Mecanismos de controle de fluxo insuficientes referem-se à capacidade de um invasor que obteve permissões de acesso a um sistema dentro do processo de CI/CD (SCM, CI, repositório de artefatos etc.) de enviar manualmente códigos maliciosos ou artefatos pelo pipeline devido à falta de mecanismos que imponham aprovação ou revisão adicional.

## Descrição

Os fluxos CI/CD são projetados para serem velozes. Um novo código pode ser criado na máquina de um desenvolvedor e chegar à produção em minutos, frequentemente com total confiança na automação e no mínimo envolvimento humano. Vendo que os processos de CI/CD são essencialmente o caminho para os ambientes de produção altamente controlados e protegidos, as organizações introduzem continuamente medidas e controles destinados a garantir que nenhuma entidade (humana ou aplicação) possa enviar código ou artefatos através do pipeline sem ser obrigado a passar por um conjunto rigoroso de revisões e aprovações.

## Impacto

Um invasor com acesso ao SCM, CI ou sistemas mais internos do pipeline pode abusar de mecanismos de controle de fluxo insuficientes para implantar ou inserir artefatos maliciosos. Depois de criados, os artefatos são enviados pelo pipeline - potencialmente até a produção - sem qualquer aprovação ou revisão. Por exemplo, um atacante pode:


* Fazer Push de código para um branch, que é implementado automaticamente através do pipeline no ambiente de produção.
* Fazer Push de código para um branch, e em seguida, acionar manualmente um pipeline que envia o código para produção.
* Fazer push de código diretamente para uma lib, que é usada pelo código em execução em um sistema em produção.
* Abusar da regra de auto-merge no CI que faz merge automaticamente de pull requests que estão de acordo com um conjunto predefinido de requisitos, dessa forma, embarcando um código não revisado para a produção.
* Abusar de regras insuficientes de proteção de branch - por exemplo, devido ã exclusão de branches ou usuários específicos para ignorar os controles de segurança da branch e fazer push de código malicioso não revisado.
* Fazer Upload de um artefato para um repositório, como um pacote ou container, disfarçado de um artefato legítimo criado pelo ambiente de build. Nesse cenário, a falta de controles ou verificações pode fazer com que um artefato seja utilizado por um pipeline de deploy e implantado na produção.
* Acesse a produção e altere diretamente o código da aplicação ou infraestrutura (por exemplo, uma função Lambda), sem qualquer aprovação/verificação adicional.


## Recomendações

Defina mecanismos de controle de fluxo da pipeline para garantir que nenhuma entidade (humana/programática) seja capaz de enviar códigos e artefatos sensíveis por meio do pipeline sem verificação ou validação externa. Isso pode ser alcançado através da implementação das seguintes medidas:



* Configure regras de proteção de branch nas branches que hospedam os códigos que são usados em produção e em outros sistemas sensíveis. Sempre que possível, evite a exclusão de contas de usuário ou branches das regras de proteção de branch. Em contas de usuário recebem permissão para fazer push de código não revisado para um repositório, garanta que essas contas não possuam permissão para executar pipelines de implantação conectados ao repositório em questão.
* Limite o uso de regras de auto-merge automática e garanta que, onde quer que estejam em uso - elas sejam aplicáveis a uma quantidade mínima de contextos. Revise o código de todas as regras de auto-merge automáticas cuidadosamente para garantir que elas não possam ser ignoradas para evitar a importação de código de terceiros no processo de auto-merge.
* Onde aplicável, evite que contas acionem pipelines de build e de implantação em produção sem aprovação ou revisão adicional.
* Prefira permitir que os artefatos fluam pelo pipeline apenas na condição de terem sido criados por uma conta de serviço de CI pré-aprovada. Evite que artefatos carregados por outras contas fluam pelo pipeline sem revisão e aprovação secundárias.
* Detecte e evite diferenças e inconsistências entre o código em execução na produção e em sua origem no CI/CD e modifique qualquer recurso que contenha esse desvio para garantir consistência.


## Referências



1. Backdoor planted in the PHP git repository. The attackers pushed malicious unreviewed code directly to the PHP main branch, ultimately resulting in a formal PHP version being spread to all PHP websites.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

2. Bypassing auto-merge rules in Homebrew, by [RyotaK](https://twitter.com/ryotkak). An auto-merge rule used to merge insignificant changes into the main branch was susceptible to bypass, allowing adversaries to merge malicious code into the project.

    [https://brew.sh/2021/04/21/security-incident-disclosure/](https://brew.sh/2021/04/21/security-incident-disclosure/)

3. Bypassing required reviews using GitHub Actions, by [Omer Gil](https://twitter.com/omer_gil). The flaw allowed leveraging GitHub Actions to bypass the required reviews mechanism and push unreviewed code to a protected branch.

    [https://www.cidersecurity.io/blog/research/bypassing-required-reviews-using-github-actions/](https://www.cidersecurity.io/blog/research/bypassing-required-reviews-using-github-actions/)
