---

layout: col-sidebar
title: "CICD-SEC-3: Abuso de cadeia de dependência"

---
## Definição


Os riscos de abuso da cadeia de dependência referem-se à capacidade de um invasor de abusar de falhas relacionadas a como as estações de trabalho de devenvolvedores e ambientes de build buscam dependências de código. O abuso da cadeia de dependência resulta em um pacote malicioso inadvertidamente obtido e executado localmente quando extraído.


## Descrição


O gerenciamento de dependências e pacotes externos usados pelo código está se tornando cada vez mais complexo devido ao grande número de sistemas envolvidos no processo em todos os contextos de desenvolvimento em uma organização. Muitas vezes, os pacotes são obtidos usando um cliente dedicado por linguagem de programação, geralmente a partir de uma combinação de repositórios de pacotes autogerenciados (por exemplo, Jfrog Artifactory) e repositórios SaaS específicos da linguagem (por exemplo - Node.js tem o npm e o registro npm, o pip do Python usa PyPI , e as gems do Ruby usam RubyGems).

Muitas organizações se esforçam ao máximo para detectar o uso de pacotes com vulnerabilidades conhecidas e realizar análises estáticas de códigos escritos por eles mesmos e de terceiros. No entanto, no contexto do uso de dependências, há um conjunto igualmente importante de controles necessários para proteger o ecossistema de dependências - envolvendo a segurança do processo que define como as dependências são obtidas. Configurações inadequadas podem fazer com que um desenvolvedor desavisado, ou pior - o sistema de compilação, baixe um pacote malicioso em vez do pacote que deveria ser baixado. Em muitos casos, o pacote não é apenas baixado, mas também executado imediatamente após o download, devido a scripts de pré-instalação e processos semelhantes que são projetados para executar o código de um pacote imediatamente após a extração do pacote.

Os principais vetores de ataque neste contexto são:


* <span style="text-decoration:underline;">Confusão de dependências</span> - Publicação de pacotes maliciosos em repositórios públicos com o mesmo nome de pacotes internos, em uma tentativa de induzir os gerenciadores de pacotes a baixar o pacote malicioso em vez de pacote privado.
* <span style="text-decoration:underline;">Sequestro de dependência</span> - Obtenção do controle da conta de um mantenedor de pacote no repositório público, a fim de carregar uma nova versão maliciosa de um pacote amplamente utilizado, com a intenção de comprometer clientes desavisados que baixam a versão mais recente do pacote.
* <span style="text-decoration:underline;">Engano por digitação incorreta (Typosquatting)</span> - Publicação de pacotes maliciosos com nomes semelhantes aos de pacotes populares na esperança de que um desenvolvedor digite incorretamente o nome de um pacote e busque acidentalmente o pacote com erro de escrita.
* <span style="text-decoration:underline;">Sequestro de identidade/Marca online (Brandjacking)</span> - Publicação de pacotes maliciosos de maneira consistente com a convenção de nomenclatura ou outras características do pacote de uma marca específica, na tentativa de obter desenvolvedores desavisados buscar esses pacotes por associá-los falsamente à marca confiável.


## Impacto

O objetivo dos atacantes que enviam pacotes para repositórios de pacotes públicos usando uma das técnicas mencionadas acima é executar código malicioso em um host que baixar o pacote. Isso pode ser a estação de trabalho de um desenvolvedor ou um servidor de compilação puxando o pacote.

Depois que o código malicioso está em execução, ele pode ser aproveitado para roubo de credenciais e movimentação lateral dentro do ambiente em que é executado.

Outro cenário potencial é o código mal-intencionado do invasor chegar aos ambientes de produção a partir do servidor de compilação. Em muitos casos, o pacote malicioso continuaria a manter também a funcionalidade original e segura que o usuário esperava, resultando em uma menor probabilidade de descoberta.


## Recomendações

Há uma ampla gama de métodos de mitigação que são específicos para a configuração dos diferentes gerenciadores de pacotes de cada linguagem de programação e a maneira como os proxies internos e os repositórios de pacotes externos são usados.

Dito isso, todos os controles recomendados compartilham os mesmos princípios -



* Nenhum gerenciador pacotes deve ter permissão para baixar pacotes diretamente da Internet ou de fontes não confiáveis. Em vez disso, os seguintes controles devem ser implementados:
     * Sempre que pacotes de terceiros forem baixados de um repositório externo, certifique-se de que todos os pacotes sejam obtidos por meio de um proxy interno, e não diretamente da Internet. Isso permite a utilização de controles de segurança adicionais na camada de proxy, além de fornecer recursos investigativos em torno de pacotes baixados - no caso de um incidente de segurança.
     * Quando aplicável, não permita o download de pacotes diretamente de repositórios externos. Configure todos os gerenciadores de pacotes para baixar pacotes de repositórios internos, contendo pacotes pré-vetados, e estabeleça um mecanismo para verificar e aplicar essa configuração em todos os gerenciadores.
* Ativar verificação de checksum e verificação de assinatura para pacotes baixados.
* Evite configurar o gerenciadores de pacotes para baixar a versão mais recente do pacote (Latest). Prefira configurar uma versão pré-aprovada ou intervalos de versões. Use as técnicas específicas da estrutura para “bloquear” continuamente a versão do pacote necessária em sua organização para uma versão estável e segura.
* Escopos:
     * Certifique-se de que todos os pacotes privados sejam registrados no escopo da organização.
     * Certifique-se de que todos os códigos que fazem referência a um pacote privado usem o escopo do pacote.
     * Certifique-se de que os clientes sejam forçados a buscar pacotes que estão sob o escopo de sua organização exclusivamente de seu repositório interno.
* Quando os scripts de instalação estiverem sendo executados como parte da instalação do pacote, certifique-se de que exista um contexto separado para esses scripts, que não tenha acesso a segredos e outros recursos confidenciais disponíveis em outros estágios do processo de build.
* Certifique-se de que os projetos internos sempre contenham arquivos de configuração de gerenciadores de pacotes (por exemplo .npmrc no NPM) no repositório de código do projeto, para substituir qualquer configuração insegura potencialmente existente em um cliente que busca o pacote.
* Evite publicar nomes de projetos internos em repositórios públicos.
* Como regra geral, dada a quantidade de gerenciadores de pacotes e configurações em uso simultaneamente, a prevenção completa do abuso da cadeia de terceiros está longe de ser trivial. Portanto, é recomendável garantir que um nível apropriado de foco seja colocado em torno da detecção, monitoramento e mitigação para garantir que, em caso de incidente, ele seja identificado o mais rápido possível e tenha o mínimo de danos potenciais. Nesse contexto, todos os sistemas relevantes devem ser protegidos adequadamente de acordo com as diretrizes sob o risco "CICD-SEC-7: Configuração Inseguras de Sistemas".


## Referências



1. Dependency Confusion, by [Alex Birsan](https://twitter.com/alxbrsn). An attack vector that tricks package managers and proxies into fetching a malicious package from a public repository instead of the intended package of the same name from an internal repository.

    [https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)

2. Amazon, Zillow, Lyft, and Slack NodeJS apps targeted by threat actors using the Dependency Confusion vulnerability.

    [https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/](https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/)

3. The _ua-parser-js_ NPM library, with 9 million downloads a week, was hijacked to launch cryptominers and steal credentials.

    [https://github.com/advisories/GHSA-pjwm-rvh2-c87w](https://github.com/advisories/GHSA-pjwm-rvh2-c87w)

4. The _coa_ NPM library, with 9 million downloads a week, was hijacked to steal credentials.

    [https://github.com/advisories/GHSA-73qr-pfmq-6rp8](https://github.com/advisories/GHSA-73qr-pfmq-6rp8)

5. The _rc_ NPM library, with 14 million downloads a week, was hijacked to steal credentials.

    [https://github.com/advisories/GHSA-g2q5-5433-rhrf](https://github.com/advisories/GHSA-g2q5-5433-rhrf)
