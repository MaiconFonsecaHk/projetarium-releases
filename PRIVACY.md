# Política de Privacidade do Projetarium

Última atualização: 24 de setembro de 2026.

Esta política descreve como o aplicativo Projetarium, distribuído pela
Vermillion Technologies, trata dados nos modos Local e Self-hosted. Ela se
aplica aos binários e instaladores oficiais publicados por este repositório e
pela Microsoft Store.

## Responsabilidade pelo tratamento

A Vermillion Technologies é responsável pelos canais de distribuição e
suporte que opera. Em uma instalação Self-hosted administrada por outra pessoa
ou organização, o operador dessa instalação controla o servidor e é
responsável por definir acesso, retenção, backups e atendimento às pessoas que
utilizam aquele ambiente.

## Dados tratados pelo aplicativo

Conforme os recursos utilizados, o Projetarium pode armazenar nomes e
identificadores de contas, hashes de senha, projetos, blocos, tarefas, artigos,
comentários, históricos, anexos, configurações e registros de auditoria. Esses
dados são fornecidos ou criados pelos próprios usuários e equipes.

O Projetarium não inclui publicidade, telemetria de produto, rastreamento de
comportamento ou venda de dados pessoais.

## Modo Local

No modo Local, o banco SQLite, os anexos, as exportações e os backups são
gravados no computador e nas pastas escolhidas pelo usuário. O Projetarium não
envia automaticamente esse conteúdo para uma infraestrutura operada pela
Vermillion Technologies.

Quando uma pasta sincronizada é escolhida para backup externo, o Projetarium
grava nela um pacote criptografado. A sincronização é executada pelo programa do
provedor instalado no computador, como OneDrive, Google Drive ou serviço
equivalente. O Projetarium não recebe as credenciais desses provedores e não
confirma que o arquivo foi enviado aos servidores deles.

## Modo Self-hosted

No modo Self-hosted, o aplicativo transmite autenticação, conteúdo dos projetos
e arquivos para o endereço de servidor configurado pelo usuário ou pela equipe.
O operador dessa instalação controla armazenamento, acesso, retenção, backups e
logs do servidor. A Vermillion Technologies não recebe automaticamente esses
dados quando não opera a instalação informada.

## Credenciais e segurança

Senhas são armazenadas como hashes nos bancos aplicáveis. Tokens de sessão e
segredos locais compatíveis são mantidos pelo mecanismo seguro disponível no
sistema operacional. Backups externos do modo Local usam criptografia e uma
senha de recuperação definida pelo usuário.

Nenhum mecanismo elimina todos os riscos. Usuários devem manter o sistema
atualizado, proteger credenciais, limitar acessos ao servidor Self-hosted e
conservar backups independentes testados.

## Compartilhamento, suporte e serviços de terceiros

O Projetarium não compartilha automaticamente o conteúdo dos projetos com a
Vermillion Technologies. Ao abrir voluntariamente uma solicitação de suporte no
GitHub, o usuário escolhe quais informações envia e também fica sujeito às
políticas do GitHub. Bancos, dumps, tokens, senhas, caminhos privados e anexos
reais não devem ser publicados em issues.

A Microsoft pode tratar informações relacionadas à aquisição, instalação e
atualização do aplicativo pela Microsoft Store de acordo com as próprias
políticas. Esse tratamento ocorre fora do Projetarium.

## Retenção, acesso, correção e exclusão

No modo Local, o usuário controla os arquivos mantidos no computador e suas
cópias de backup. No modo Self-hosted, solicitações de acesso, correção ou
exclusão devem ser dirigidas ao administrador da instalação. A exclusão de uma
conta ou do aplicativo não apaga necessariamente exportações e backups já
criados, que precisam ser administrados separadamente.

## Crianças e adolescentes

O Projetarium é uma ferramenta de produtividade para organização de projetos e
não é direcionado especificamente a crianças. Organizações que permitam seu uso
por menores devem aplicar as autorizações e os controles exigidos pela
legislação e por suas próprias políticas.

## Alterações e contato

Mudanças materiais nesta política serão publicadas com nova data de
atualização. Para dúvidas objetivas sobre privacidade ou funcionamento do
aplicativo, consulte o [guia de suporte](SUPPORT.md). Vulnerabilidades ou
possíveis exposições de dados devem ser relatadas pelo canal privado descrito na
[política de segurança](SECURITY.md), nunca em uma issue pública.
