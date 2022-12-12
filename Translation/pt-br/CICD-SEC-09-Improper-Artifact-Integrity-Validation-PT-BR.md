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

A validação inadequada da integridade do artefato pode ser abusada por um adversário com uma posição dentro do processo de entrega de software para enviar um artefato malicioso através do pipeline, resultando na execução de código malicioso - em sistemas dentro do processo de CI/CD ou pior - na produção .


## Recomendações

The prevention of improper artifact integrity validation risks requires a collection of measures, across different systems and stages within the software delivery chain. Consider the following controls: 



* Implement processes and technologies to validate the integrity of resources all the way from development to production. When a resource is generated, the process will include signing that resource using an external resource signing infrastructure. Prior to consuming the resource in subsequent steps down the pipeline, the resource’s integrity should be validated against the signing authority. Some prevalent measures to consider in this context:
    * **Code signing** - SCM solutions provide the ability to sign commits using a unique key for each contributor. This measure can then be leveraged to prevent unsigned commits from flowing down the pipeline.
    * **Artifact verification software** - Usage of tools for signing and verification of code and artifacts provide a way to prevent unverified software from being delivered down the pipeline. An example for such a project is [Sigstore](https://www.sigstore.dev/), created by the Linux Foundation.
    * **Configuration drift detection** - Measures aimed at detecting configuration drifts (e.g. resources in cloud environments which aren’t managed using a signed IAC template), potentially indicative of resources that were deployed by an untrusted source or process.
* 3rd party resources fetched from build/deploy pipelines (such as scripts imported and executed as part of the build process) should follow a similar logic - prior to using 3rd party resources, the hash of the resource should be calculated and cross referenced against the official published hash of the resource provider. 


## Referências



1. The hack of the SolarWinds build system, used to spread malware through SolarWinds to 18,000 organizations. The code of the Orion software was changed in the build system during the build process, leaving no trace in the codebase.

	[https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm](https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm)



2. Codecov, a popular code coverage tool used in the CI, is compromised to steal environment variables from builds. Attackers gained access to the GCP (Google Cloud Platform) account hosting the Codecov script, and modified it to contain malicious code. The attack was identified by a customer comparing the hash of the script stored on GitHub with the script downloaded from the GCP account.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

3. Backdoor planted in the PHP git repository, ultimately resulting in a formal PHP version being spread to all PHP users. The attackers push malicious unreviewed code directly to the PHP main branch, committing the code as if it were made by known PHP contributors.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

4. Attackers compromise the Webmin build server, and add a backdoor to one of the application’s scripts. The backdoor continued to persist even after the compromised build server was decommissioned due to the fact that code was restored from a local backup, rather than the source control system. Webmin users were susceptible to RCE through a supply chain attack for a duration of over 15 months, until the backdoor was removed.

    [https://www.webmin.com/exploit.html](https://www.webmin.com/exploit.html)
