# Projetarium 0.1.0-beta.1

Status: beta em preparação; consulte a página de Releases para saber se o
pré-lançamento já foi promovido.

## Escopo da beta

Esta será a primeira beta pública do Projetarium Local para Windows 10/11 x64.
Ela reúne Board, Canvas, Backlog, Timeline, tarefas, artigos, usuários locais,
backup e recuperação em um aplicativo desktop com SQLite.

O número inicial do build é `1`. A nota será fechada somente quando o
instalador, o executável, os hashes e a tag forem derivados do mesmo commit.

## Limitações conhecidas

- Não existe atualização automática nesta beta.
- Self-hosted e Web ainda não são suportados publicamente.
- A CLI autenticada permanece experimental e somente leitura.
- Agentes de IA não podem realizar mutações.
- A interface completa do Registro de Auditoria não está concluída.
- UML e modelagem de sistemas continuam no roadmap.
- Linux desktop ainda não possui aplicativo ou pacote suportado.

## Segurança dos dados

- Mantenha o banco ativo em pasta local, fora de pastas sincronizadas.
- Use OneDrive, Google Drive ou serviço equivalente apenas como destino de
  backups concluídos.
- Preserve uma cópia independente antes de atualizar ou restaurar.
- Não publique bancos, dumps, credenciais, caminhos completos ou conteúdo de
  projetos em GitHub Issues.

## Gate obrigatório da publicação

- gerar e assinar o instalador Windows;
- verificar instalação e primeira execução em máquina limpa;
- atualizar sobre uma instalação anterior sem perder dados;
- desinstalar preservando banco, anexos, preferências e backups;
- conferir versão e publicador no aplicativo e nas propriedades do binário;
- publicar SHA-256, termos e avisos de terceiros junto da tag.
