# Relatorio de Atividade: Introducao ao AWS IAM


## Visao Geral do Laboratorio

O trabalho consistiu em configurar e testar o acesso de três colaboradores fictícios a uma infraestrutura AWS. O foco principal foi garantir que cada pessoa tivesse exatamente as permissões necessárias para sua função de trabalho, aplicando o conceito de privilégio mínimo.

Durante a atividade, explorei a interface do IAM, analisei políticas de segurança escritas em JSON e validei na prática o que acontece quando um usuário tenta realizar uma ação para a qual não possui autorização.

## Etapas de Configuracao

### 1. Analise de Usuarios e Grupos
O ambiente de laboratório forneceu três usuários (user-1, user-2 e user-3) e três grupos com permissões distintas. Analisei as políticas vinculadas a esses grupos para entender o que cada perfil de acesso permitia:

* O grupo **EC2-Support** utilizava uma política gerenciada pela AWS que permite apenas visualizar informações do Amazon EC2.
* O grupo **S3-Support** permitia a listagem e visualização de arquivos no Amazon S3.
* O grupo **EC2-Admin** possuía uma política em linha (inline) específica para permitir o controle operacional de instâncias, como ligar e desligar servidores.

### 2. Organizacao de Acessos
Seguindo o cenário proposto, realizei a inclusão dos usuários nos grupos corretos. Esta é uma boa prática de administração: em vez de dar permissões diretamente ao usuário, nós as damos ao grupo. Quando o usuário entra no grupo, ele herda automaticamente as regras de acesso.

* O **user-1** foi designado ao suporte de armazenamento (S3-Support).
* O **user-2** foi designado ao suporte de computação (EC2-Support).
* O **user-3** recebeu as credenciais de administrador de instâncias (EC2-Admin).

## Resultados dos Testes Praticos

Para garantir que a configuração estava correta, realizei logins individuais com cada usuário através de uma URL de acesso específica:

* **Teste com user-1:** Confirmei que era possível ver os buckets do S3, mas o sistema bloqueou qualquer tentativa de visualizar instâncias do EC2, exibindo mensagens de falta de autorização.
* **Teste com user-2:** Consegui visualizar os servidores ativos (como o LabHost), mas ao tentar desligar a instância, a AWS negou a operação. Isso provou que a conta era de "apenas leitura".
* **Teste com user-3:** O teste de desligamento da instância LabHost funcionou perfeitamente, confirmando que as permissões de administração estavam ativas apenas para este perfil.

## Conclusao e Aprendizados

A principal lição deste laboratório é que o IAM funciona como o porteiro da nuvem. Ele não apenas identifica quem é o usuário, mas controla rigorosamente o que ele pode fazer. 

Entendi a diferença prática entre uma Política Gerenciada (que a AWS cria e mantém) e uma Política Inline (criada para uma necessidade específica de um grupo). O uso de grupos facilita muito a gestão: se um novo funcionário for contratado para o suporte, basta adicioná-lo ao grupo correto para que ele tenha as permissões prontas para o trabalho, sem risco de acesso indevido a outras áreas da conta.


