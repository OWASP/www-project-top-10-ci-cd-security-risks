---

layout: col-sidebar
title: "CICD-SEC-10: Visibilidade e Logs Insuficientes"

---
## Definição

Logs insuficientes e riscos de visibilidade permitem que um adversário realize atividades maliciosas no ambiente de CI/CD sem ser detectado durante qualquer fase do attack kill chain, incluindo a identificação dos TTPs (Técnicas, Táticas e Procedimentos) do invasor como parte de qualquer pós-incidente investigação.

## Descrição

A existência de recursos de logs bem detalhados e visibilidade é essencial para a capacidade de uma organização de se preparar, detectar e investigar um incidente relacionado à segurança.

Embora estações de trabalho, servidores, dispositivos de rede e aplicações do negócio ou de tecnologia s sejam normalmente abordados em profundidade nos programas de registro e visibilidade de uma organização, geralmente não é o caso de sistemas e processos em ambientes de desenvolvimento.

Dada a quantidade de potenciais vetores de ataque que utilizam ambientes e processos de desenvolvimento, é imperativo que as equipes de segurança consttruam recursos apropriados para detectar esses ataques assim que eles acontecerem. Como muitos desses vetores envolvem alavancar o acesso programático em relação aos diferentes sistemas, um aspecto fundamental para enfrentar esse desafio é criar fortes níveis de visibilidade em torno do acesso humano e programático.

Dada a natureza sofisticada dos vetores de ataque CI/CD, há um nível igual de importância para ambos os logs de auditoria dos sistemas - por exemplo acesso do usuário, criação do usuário, modificação da permissão e os logs da aplicação - por exemplo evento push para um repositório, execução de compilações, upload de artefatos.


## Impacto

Com os adversários gradualmente mudando seu foco para ambientes de desenvolvimento como um meio de atingir seus objetivos, as organizações que não garantem os controles apropriados de logs e visibilidade em torno desses ambientes, podem não detectar uma violação e enfrentar grandes dificuldades na mitigação/remediação devido ao mínimo capacidades investigativas.

Tempo e dados são as commodities mais valiosas para uma organização sob ataque. A existência de todas as fontes de dados relevantes em um local centralizado pode ser a diferença entre um resultado bem-sucedido e um resultado devastador em um cenário de resposta a incidentes.



## Recomendações

Existem vários elementos para obter registro e visibilidade suficientes:



* **Mapeamento do ambiente** - Boas capacidades de visibilidade não podem ser alcançadas sem um nível íntimo de familiaridade com todos os diferentes sistemas envolvidos em ameaças potenciais. Uma possível violação pode envolver qualquer um dos sistemas que fazem parte dos processos de CI/CD, incluindo o SCM, CI, repositórios de artefatos, software de gerenciamento de pacotes, registros de contêineres, CD e mecanismos de orquestração (por exemplo, K8s).

     Identifique e crie um inventário de todos os sistemas em uso na organização, contendo todas as instâncias desses sistemas (especificamente relevantes para sistemas autogerenciados, por exemplo, Jenkins).

* **Identificando e habilitando as origens de log apropriadas** - Depois que todos os sistemas relevantes forem identificados, a próxima etapa é garantir que todos os logs relevantes sejam habilitados, pois esse não é o estado padrão nos diferentes sistemas. A visibilidade deve ser otimizada tanto no acesso humano quanto no acesso programático por meio de todas as várias medidas permitidas. É importante colocar um nível igual de ênfase na identificação de todas as fontes de log de auditoria relevantes, bem como as fontes de log de aplicações.
* **Envio de logs para um local centralizado** (por exemplo, SIEM), para oferecer suporte à agregação e correlação de logs entre diferentes sistemas para detecção e investigação.
* **Criação de alertas para detecção de anomalias e potenciais atividades maliciosas**, tanto em cada sistema isoladamente quanto anomalias no processo de envio de código, que envolve vários sistemas e requer conhecimento mais profundo nos processos internos de build e deploys.


## Referências

Os recursos de logs e visibilidade são essenciais e relevantes para poder detectar e investigar qualquer incidente, independentemente do risco que foi explorado no incidente. Qualquer incidente de segurança nos últimos anos envolvendo sistemas CI/CD exigiu que a organização vítima tivesse forte visibilidade para poder investigar adequadamente e entender a extensão dos danos do ataque em questão.