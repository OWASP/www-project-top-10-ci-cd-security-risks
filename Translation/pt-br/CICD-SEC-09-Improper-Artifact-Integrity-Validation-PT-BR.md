---

layout: col-sidebar
title: "CICD-SEC-9: Validação de integridade de artefatos impróprias"

---
## Definição


Os riscos de validação de integridade de artefatos impróprios permitem que um invasor com acesso a um dos sistemas no processo de CI/CD envie códigos ou artefatos maliciosos (embora aparentemente benignos) pelo pipeline, devido a mecanismos insuficientes para garantir a validação de códigos e artefatos.


## Descrição

Os processos de CI/CD consistem em várias etapas, responsáveis por levar o código desde a estação de trabalho de um desenvolvedor até a produção. Existem vários recursos sendo alimentados em cada etapa - combinando recursos internos, artefatos com pacotes de terceiros e artefatos obtidos de locais remotos. O fato de o recurso final depender de várias fontes espalhadas pelas diferentes etapas, fornecidas por vários colaboradores, cria vários pontos de entrada por meio dos quais esse recurso final pode ser adulterado.

Se um recurso adulterado conseguiu se infiltrar com sucesso no processo de entrega, sem levantar qualquer suspeita ou encontrar qualquer verficação de segurança - provavelmente continuará fluindo pelo pipeline - até a produção - sob o disfarce de um recurso legítimo.


## Impacto

A validação inadequada da integridade do artefato pode ser abusada por um adversário com uma posição dentro do processo de entrega de software para enviar um artefato malicioso através do pipeline, resultando na execução de código malicioso - em sistemas dentro do processo de CI/CD ou pior - na produção.


## Recomendações

A prevenção de riscos de Validação de integridade de artefatos impróprias requer uma coleção de medidas, em diferentes sistemas e estágios dentro da cadeia de entrega de software. Considere os seguintes controles:



* Implementar processos e tecnologias para validar a integridade dos recursos desde o desenvolvimento até a produção. Quando um recurso é gerado, o processo incluirá a assinatura desse recurso usando uma infraestrutura de assinatura de recurso externo. Antes de consumir o recurso nas etapas subsequentes do pipeline, a integridade do recurso deve ser validada em relação à autoridade de assinatura. Algumas medidas prevalentes a serem consideradas neste contexto:
     * **Assinatura de código** - As soluções SCM fornecem a capacidade de assinar confirmações usando uma chave exclusiva para cada contribuidor. Essa medida pode então ser aproveitada para evitar que commits não assinados fluam pelo pipeline.
     * **Software de verificação de artefato** - O uso de ferramentas para assinatura e verificação de código e artefatos fornece uma maneira de impedir que software não verificado seja entregue no pipeline. Um exemplo desse projeto é o [Sigstore](https://www.sigstore.dev/), criado pela Linux Foundation.
     * **Detecção de desvio de configuração** - Medidas destinadas a detectar desvios de configuração (por exemplo, recursos em ambientes de nuvem que não são gerenciados usando um modelo IAC assinado), potencialmente indicativos de recursos que foram implantados por uma fonte ou processo não confiável.
* Recursos de terceiros obtidos de pipelines de build/deploy (como scripts importados e executados como parte do processo de build) devem seguir uma lógica semelhante - antes de usar recursos de terceiros, o hash do recurso deve ser calculado e referenciado em relação ao hash oficial publicado do provedor de recursos.


## Referências



1. The hack of the SolarWinds build system, used to spread malware through SolarWinds to 18,000 organizations. The code of the Orion software was changed in the build system during the build process, leaving no trace in the codebase.

	[https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm](https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm)



2. Codecov, a popular code coverage tool used in the CI, is compromised to steal environment variables from builds. Attackers gained access to the GCP (Google Cloud Platform) account hosting the Codecov script, and modified it to contain malicious code. The attack was identified by a customer comparing the hash of the script stored on GitHub with the script downloaded from the GCP account.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

3. Backdoor planted in the PHP git repository, ultimately resulting in a formal PHP version being spread to all PHP users. The attackers push malicious unreviewed code directly to the PHP main branch, committing the code as if it were made by known PHP contributors.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

4. Attackers compromise the Webmin build server, and add a backdoor to one of the application’s scripts. The backdoor continued to persist even after the compromised build server was decommissioned due to the fact that code was restored from a local backup, rather than the source control system. Webmin users were susceptible to RCE through a supply chain attack for a duration of over 15 months, until the backdoor was removed.

    [https://www.webmin.com/exploit.html](https://www.webmin.com/exploit.html)
