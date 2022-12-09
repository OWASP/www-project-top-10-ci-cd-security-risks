---

layout: col-sidebar
title: "CICD-SEC-2: Gerenciamento de Identidade e Acessos inadequado"

---
## Definição


Os riscos de gerenciamento de identidades e controle de acesso inadequados decorrem das dificuldades em gerenciar a grande quantidade de identidades espalhadas pelos diferentes sistemas no ecossistema de desenvolvimento, desde o controle do código até a implantação. A existência de identidades mal geridas - tanto as contas humanas quanto as programáticas - aumenta o potencial e a extensão dos danos quando comprometidas.


## Descrição

Os processos de entrega de software consistem em vários sistemas conectados entre si com o objetivo de mover o código e os artefatos do desenvolvimento para a produção. Cada sistema fornece multiplos métodos de acesso e de integração (nome de usuário e senha, token de acesso, aplicações de marketplace, aplicativos oauth, plugins, chaves SSH). Os diferentes tipos de contas e métodos de acesso podem ter seu próprio método de provisionamento exclusivo, conjunto de políticas de segurança e modelo de autorização. Essa complexidade cria desafios no gerenciamento de diferentes identidades ao longo de todo o ciclo de vida da identidade e na garantia de que suas permissões estejam alinhadas com o princípio do menor privilégio (Least privilege).

Além disso, em um ambiente típico, a maioria das contas de usuário de um SCM ou CI é altamente permissiva, pois esses sistemas não são tradicionalmente uma área de foco importante para as equipes de segurança. Essas identidades são usadas principalmente por engenheiros que precisam de flexibilidade para criar grandes mudanças no código e na infraestrutura.

Algumas das principais preocupações e desafios relacionados ao gerenciamento de identidade e acesso no ecossistema CI/CD incluem:



* **Identidades excessivamente permissivas** - Manter o princípio do menor privilégio para contas humanas e de aplicacões. Por exemplo, em SCMs - Garantir que cada identidade humana e de aplicação tenha apenas as permissões necessárias somente nos repositórios que ela realmente precisa não é algo normalmente adotado.
* **Identidades obsoletas** - Funcionários/sistemas que não estão ativos e/ou não necessitam mais do acesso, mas não tiveram sua conta humana e/ou programática em todos os sistemas CI/CD revogados.
* **Identidades locais** - Sistemas que não possuem acesso federado com um IDP centralizado, criando identidades que são gerenciadas localmente dentro do sistema em questão. As contas locais criam desafios na aplicação de políticas de segurança consistentes (por exemplo, política de senha, política de bloqueio, MFA), bem como na revogação adequada do acesso em todos os sistemas (por exemplo, quando um funcionário sai da organização).
* **Identidades Externas** - 
    * <span style="text-decoration:underline;">Colaboradores cadastrados com endereço de email de um domínio que não pertence ou não é gerenciado pela organização</span> - Nesse cenário, a segurança dessas contas é altamente dependente da segurança fornecida na solução ao qual as contas pertencem. Uma vez que essas contas não são gerenciadas pela empresa, elas não necessáriamente estão em conformidade com a política de segurança da empresa. 
    * <span style="text-decoration:underline;">Colaboradores Externos</span> - Uma vez que o acesso é concedido a colaboradores externos a um sistema, o nível de segurança do sistema é derivado do nível do ambiente de trabalho do colaborador externo, fora do controle da organização.
* **Identidades autorregistradas** - Em sistemas onde o autorregistro é permitido, muitas vezes um endereço de domínio válido é o único pré-requisito para o autorregistro e acesso a sistemas de CI/CD. O uso do grupo de permissões padrão/base que seja diferente de "nada" expande significativamente a superfície de ataque.
* **Identidades Compartilhadas** - Identidades compartilhadas entre usuários humanos / aplicações / tanto humanos quanto aplicações diminuem a rastreabilidade das atividades, bem como criam desafios relacionados à responsabilidade no caso de uma possível investigação.


## Impacto

A existência de centenas (ou às vezes milhares) de identidades - tanto humanas quanto programáticas - em todo o ecossistema de CI/CD, combinada com a falta de identidade forte, práticas de gerenciamento de acesso e uso comum de contas excessivamente permissivas, leva a um estado onde o comprometimento de  qualquer conta de usuário em qualquer sistema, possa dar acesso a  recursos poderosos no ambiente e pode servir como um salto para o ambiente de produção.

## Recomendações



* Realizar análise e mapeamento contínuos de todas as identidades em todos os sistemas dentro do ecossistema de desenvolvimento. Para cada identidade, mapeie o provedor de identidade, o nível de permissões concedidas e o nível de permissões realmente usado. Certifique-se de que todos os métodos de acesso programático sejam cobertos na análise.
* Remover as permissões desnecessárias para o trabalho contínuo de cada identidade nos diferentes sistemas do ambiente.
* Determine um período aceitável para desabilitar/remover contas desatualizadas e desabilitar/remover qualquer identidade que tenha ultrapassado o período predeterminado de inatividade.
* Evitar criar contas de usuários locais. Em vez disso, crie e gerencie identidades usando um provedor de identidade centralizado (IdP). Sempre que contas de usuários locais estiverem em uso, certifique-se de que as contas que não deveriam ter mais acesso sejam desativadas/removidas e que as políticas de segurança de todas as contas existentes correspondam às políticas da organização.
* Mapear continuamente todos os colaboradores externos e garantir que suas identidades estejam alinhadas com o princípio do menor privilégio. Sempre que possível, conceda permissões com uma data de expiração predeterminada - para contas humanas e programáticas - e desative a conta assim que o trabalho estiver concluído.
* Impedir que os funcionários usem seus endereços de e-mail pessoais, ou qualquer endereço que pertença a um domínio não pertencente e/ou não gerenciado pela organização no SCM, CI ou qualquer outra plataforma de CI/CD. Monitore continuamente os endereços fora do domínio nos diferentes sistemas e remova os usuários não compatíveis.
* Evite permitir que os usuários se autorregistrem nos sistemas e conceda permissão conforme necessário.
* Evite conceder permissões base (Ex.: Operador, Administrador) em um sistema a todos os usuários e a grandes grupos aos quais as contas de usuário são atribuídas automaticamente.
* Evite usar contas compartilhadas. Crie contas nomeadas para cada contexto específico e conceda o conjunto exato de permissões necessárias para o contexto em questão.


## Referencias



1. The Stack Overflow TeamCity build server compromise - The attacker was able to escalate their privileges in the environment due to the fact the newly registered accounts were assigned administrative privileges upon access to the system. 

    [https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/](https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/)

2. Mercedes Benz source code leaked after a self-maintained internet-facing GitLab server was available for access by self-registration.

    [https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/](https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/)

3. A self-managed GitLab server of the New York state government was exposed to the internet, allowing anyone to self-register and log in to the system, which stored sensitive secrets.

    [https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/](https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/)

4. Malware added to the Gentoo Linux distribution source code, after the GitHub account password of a project maintainer was compromised.

    [https://wiki.gentoo.org/wiki/Project:Infrastructure/Incident_Reports/2018-06-28_Github](https://wiki.gentoo.org/wiki/Project:Infrastructure/Incident_Reports/2018-06-28_Github)
