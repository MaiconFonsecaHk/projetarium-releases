# Projetarium

[![Plataformas](https://img.shields.io/badge/plataformas-Windows%20%7C%20Web-0078D4)](../../releases)
[![Edições](https://img.shields.io/badge/edições-Local%20%7C%20Self--hosted-5B5BD6)](../../releases)
[![Distribuição](https://img.shields.io/badge/distribuição-freemium-7C3AED)](../../releases)
[![Código-fonte](https://img.shields.io/badge/código--fonte-proprietário-B91C1C)](#licença-e-distribuição)

O **Projetarium** é uma plataforma de gestão, documentação e modelagem de projetos de tecnologia. Em um único ambiente, ele reúne planejamento visual, listas de tarefas, documentação técnica, colaboração por projeto, automação por linha de comando, agentes de IA com permissões restritas, auditoria e diagramas de sistemas.

Este é o repositório público oficial de distribuição. Aqui são publicados os aplicativos compilados, instaladores, manifestos assinados, notas de versão e instruções destinadas aos usuários. O desenvolvimento e o código-fonte permanecem em um repositório privado.

> Ainda não existe uma versão pública estável. Quando a primeira versão for aprovada nos gates de segurança, migração, desempenho e smoke test, ela aparecerá em [Releases](../../releases).

## Edições

| Característica | Projetarium Local | Projetarium Self-hosted |
|---|---|---|
| Aplicativos | Windows | Windows e Web |
| Armazenamento | SQLite no computador | PostgreSQL no servidor do cliente |
| Funcionamento offline | Sim | O servidor precisa estar acessível |
| Colaboração simultânea | Uso local | Equipes e múltiplos dispositivos |
| Infraestrutura externa obrigatória | Nenhuma | VPS própria com domínio e HTTPS |
| Privacidade | Dados mantidos na máquina | Dados mantidos na infraestrutura do cliente |
| Melhor aplicação | Uso pessoal, estudo e projetos individuais | Equipes, empresas e acesso remoto |

O **Projetarium Local** é a edição gratuita e offline. O **Projetarium Self-hosted** utiliza a mesma experiência de produto, mas conecta os clientes Windows e Web a uma API instalada na VPS da própria organização. Não existe dependência obrigatória de Firebase ou de um banco controlado pelo Projetarium.

## Funcionalidades

### Projetos, Board e Backlog

- Board com colunas de status, cartões reposicionáveis e visão rápida do andamento.
- Backlog hierárquico com busca e filtros por projeto, status, modalidade, urgência, responsável e prazo.
- Referências humanas sequenciais por projeto, como `#44F` para feature e `#45B` para bug, sem substituir os identificadores técnicos internos.
- Timeline do projeto com alterações, eventos e navegação até o conteúdo relacionado.
- Insights com distribuição de status, itens bloqueados, trabalho parado, burndown e sugestões de paralelismo.
- Importação, exportação, clone e backup preservando o grafo completo do projeto e validando relações antes de modificar a base ativa.

### Canvas visual

- Área ampla com pan, zoom, minimapa, seleção múltipla e movimentação em grupo.
- Blocos para features, bugs, melhorias, refactors, documentação e outras modalidades, cada um com status, urgência, estimativa e story points.
- Hierarquia pai/filho, subtarefas aninhadas, comentários, anexos e histórico de mudanças.
- Conexões direcionais entre blocos com handles magnéticos, cancelamento seguro de ligações incompletas e exclusão confirmada.
- Contêineres visuais para agrupar blocos e setas livres para explicar fluxos sem criar dependências falsas.
- Cartões que adaptam descrição e dimensões ao conteúdo sem comprometer a navegação do Canvas.
- Cópia de um dossiê saneado do bloco em Markdown, incluindo os dados úteis e anexos compatíveis, sem comentários, histórico integral, caminhos locais ou segredos.

### Listas de tarefas

- Área dedicada a tarefas inspirada na praticidade de aplicativos como Microsoft To Do, sem exigir a criação de um bloco no Canvas.
- Listas pessoais e listas vinculadas a projetos, com ordenação manual, prioridade, vencimento, lembretes e recorrência.
- Responsáveis individuais ou múltiplos conforme o acesso ao projeto.
- Conclusão individual ou em lote, filtros de pendências e atrasos e sincronização incremental entre clientes.
- Controle de concorrência para impedir que uma edição silenciosamente sobrescreva uma alteração mais recente.
- Notificações e lembretes deduplicados, com retenção limitada para evitar crescimento indefinido do banco.

### Usuários, equipes e acesso por projeto

- Papéis de sistema separados das permissões de cada projeto.
- Papéis de projeto `owner`, `editor`, `commentator` e `viewer`, permitindo que uma pessoa edite somente os projetos aos quais recebeu acesso.
- Transferência protegida de propriedade e prevenção contra projetos sem owner.
- Revogação de acesso aplicada às próximas requisições, inclusive em sessões já abertas.
- Consultas protegidas contra descoberta de projetos, blocos, tarefas, anexos ou membros por identificadores de outro escopo.
- Opção de recordar o último nome de usuário com consentimento, sem armazenar a senha como texto ou placeholder inseguro.

### CLI e agentes de IA

O Projetarium inclui uma CLI autenticada para pessoas, scripts e agentes de IA consultarem e organizarem projetos sem acessar diretamente SQLite ou PostgreSQL.

- Consulta de projetos, backlog, bloqueios, tarefas, membros e detalhes de blocos por referência ou título.
- Saída humana, Markdown ou JSON versionado, com paginação, limites e separação previsível entre `stdout` e `stderr`.
- Criação e edição segura de blocos, descrições, comentários, responsáveis, prioridades, status e tarefas com revisão otimista, idempotência e `dry-run`.
- Perfis Local e Self-hosted com credenciais guardadas no cofre do sistema operacional; senhas e tokens não são aceitos em argumentos.
- Agentes de IA são identidades próprias, identificáveis e revogáveis, nunca contas humanas disfarçadas.
- Cada agente recebe apenas `reader` ou `editor` no sistema e em projetos explicitamente concedidos.
- Um agente editor pode criar e atualizar conteúdo, comentar, atribuir e concluir tarefas e reorganizar trabalho, mas **jamais pode excluir dados, administrar identidades, receber owner, transferir propriedade ou elevar privilégios**.
- Operações mutáveis são vinculadas a uma ordem auditada; se a auditoria não estiver disponível, a escrita do agente falha de forma fechada.

Exemplos de uso:

```powershell
# A autenticação solicita o segredo sem exibi-lo no terminal
projetarium auth login --server https://projetarium.exemplo.com --agent "Sol CLI" --profile sol

# Pesquisa por título sem depender de caixa ou acentuação
projetarium blocks search --project ID_DO_PROJETO --title "registro de auditoria" --output json

# Descobre os recursos suportados pela versão instalada
projetarium capabilities --output json
```

### Registro de Auditoria

- Registro canônico das ordens recebidas e das mutações realizadas pela interface Windows, Web, CLI, API ou processos internos.
- Identificação separada do usuário solicitante, do usuário ou agente executor, da origem, do resultado e das entidades afetadas.
- Correlação entre uma instrução e todas as alterações produzidas por ela, sem armazenar segredos ou argumentos sensíveis.
- Eventos encadeados e verificação de lacunas ou adulteração acidental.
- Busca paginada por período, projeto, ator, origem, ação, entidade e resultado.
- Exportação saneada em JSON ou CSV acompanhada de manifesto e hashes.
- Desfazer por operação compensatória: o sistema cria uma nova alteração auditada e preserva o fato de que a operação anterior aconteceu.
- Eventos de auditoria não podem ser editados ou excluídos individualmente pela interface comum.

### Documentação e conhecimento

- Editor rich text para instruções, troubleshooting, referências, changelogs, perguntas frequentes e outros artigos.
- Tags, controle de publicação e vínculo com projetos.
- Referências entre artigos, blocos, tarefas e diagramas sem conceder acesso indevido por associação.
- Exportação de conteúdo adequada para documentação técnica e compartilhamento com equipes.

### UML e modelagem de sistemas

O editor de modelagem utiliza um domínio próprio, separado do Canvas de planejamento. Os elementos representam um modelo consultável e não apenas formas posicionadas na tela.

- Suporte aos 14 tipos formais de diagramas UML 2.5.1: classes, componentes, estrutura composta, implantação, objetos, pacotes, perfis, atividades, casos de uso, máquinas de estados, sequência, comunicação, visão geral de interação e timing.
- Modelagens complementares para C4, entidade-relacionamento, BPMN e fluxogramas, cada uma com semântica e validações próprias.
- Paletas contextuais, drag and drop, seleção múltipla, camadas, alinhamento, distribuição, grid, guides, conectores, minimapa e navegação por teclado.
- Reutilização de elementos semânticos em diferentes diagramas e navegação entre referências.
- Validação incremental com diagnóstico por elemento e distinção entre erro semântico e recomendação visual.
- Undo/redo por ação lógica, autosave, revisão otimista e recuperação de falhas.
- Importação e exportação em formato nativo versionado, SVG, PNG, PDF multipágina e XMI/UMLDI.
- Integração com projetos, blocos, tarefas, artigos, auditoria e CLI, inclusive com representação textual limitada para automações por IA.

## Segurança, privacidade e recuperação

- Senhas exigem no mínimo 10 caracteres, uma letra maiúscula, um número e um caractere especial, além de rejeitarem combinações previsíveis semelhantes ao nome ou usuário.
- Segredos, hashes, tokens, caminhos internos e credenciais não são incluídos em exportações ou saídas de automação.
- Releases Self-hosted utilizam manifesto versionado, tamanhos e hashes obrigatórios e assinatura RSA-SHA256 antes da instalação.
- Atualizações da VPS geram backup do PostgreSQL e preservam configurações e uploads antes de aplicar migrations.
- Backups locais podem ser programados com frequência diária, semanal ou mensal, retenção configurável e destino escolhido pelo usuário.
- Uma pasta sincronizada pelo OneDrive, Google Drive ou serviço equivalente pode ser usada como **destino dos backups**.

> Não coloque o arquivo `projetarium.db` ativo diretamente em uma pasta sincronizada. SQLite utiliza arquivos auxiliares e transações que podem ser interrompidos ou replicados fora de ordem pelo cliente de nuvem. Mantenha o banco ativo em disco local e selecione a pasta sincronizada apenas para receber cópias de backup concluídas.

## Instalação do Projetarium Local

1. Abra a [página de Releases](../../releases) e escolha uma versão estável.
2. Baixe o pacote Windows indicado nos assets da versão.
3. Confira as notas da release, o hash publicado e a assinatura quando fornecida.
4. Instale ou extraia o pacote completo e execute `projetarium.exe`; não mova somente o executável para fora da pasta distribuída.
5. Escolha **Local**, mantenha o banco em uma pasta local e crie a primeira conta administrativa.
6. Em **Configurações → Backup**, escolha opcionalmente uma pasta do OneDrive, Google Drive ou outro serviço sincronizado para receber os backups automáticos.

Windows 10 ou Windows 11 x64 são as plataformas desktop suportadas. Os requisitos exatos e eventuais dependências acompanham as notas de cada versão.

## Instalação Self-hosted em VPS

### Requisitos recomendados

| Requisito | Mínimo inicial |
|---|---|
| Sistema operacional | Ubuntu 22.04 LTS ou Debian 12 |
| CPU | 1 vCPU |
| Memória | 1 GB de RAM |
| Armazenamento | 10 GB, ampliado conforme anexos e backups |
| Rede | Portas TCP 80 e 443 públicas |
| DNS | Registro A ou AAAA apontado para a VPS |

O instalador configura Node.js 22, PostgreSQL, Nginx, HTTPS com Let's Encrypt, PM2, usuário de serviço e diretórios protegidos. Em produção, dimensione CPU, memória, disco e retenção a partir do número de usuários, projetos e anexos da sua organização.

### Preparar domínio e firewall

Crie o registro DNS do domínio ou subdomínio antes da instalação e libere HTTP/HTTPS tanto no firewall do provedor quanto no sistema operacional:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
```

Confirme que o domínio resolve para o IP público correto. O Let's Encrypt não conseguirá emitir o certificado se DNS ou portas ainda estiverem indisponíveis.

### Instalar uma release assinada

Baixe `install.sh` e `release-public-key.pem` diretamente da tag escolhida. Compare a impressão digital da chave pública com o valor divulgado pelo projeto por um canal independente antes de confiar na primeira instalação.

```bash
VERSION="vX.Y.Z"
BASE="https://github.com/MaiconFonsecaHk/projetarium-releases/releases/download/${VERSION}"

curl --proto '=https' --tlsv1.2 --fail --location --show-error \
  "$BASE/install.sh" -o /tmp/projetarium-install.sh

curl --proto '=https' --tlsv1.2 --fail --location --show-error \
  "$BASE/release-public-key.pem" -o /tmp/projetarium-release-public.pem

openssl pkey -pubin -in /tmp/projetarium-release-public.pem -outform DER \
  | openssl dgst -sha256

sudo bash /tmp/projetarium-install.sh \
  --release "$VERSION" \
  --public-key /tmp/projetarium-release-public.pem
```

O instalador baixa os bundles da mesma tag, valida assinatura, manifesto, hash, tamanho e conteúdo dos arquivos antes de promover a versão. Nunca execute um instalador obtido de mirror, link encurtado ou comando remoto enviado por terceiros.

Durante a instalação serão solicitados:

- domínio público do Projetarium;
- usuário e nome de exibição do primeiro administrador;
- senha do administrador conforme a política exibida;
- senha exclusiva do PostgreSQL.

Depois da conclusão, acesse `https://SEU_DOMINIO` e entre com a conta criada.

### Atualizar

Baixe o `install.sh` da nova tag e execute-o informando a versão. A chave confiável guardada na primeira instalação será reutilizada; o próprio instalador também será verificado pelo manifesto assinado.

```bash
VERSION="vX.Y.Z"
curl --proto '=https' --tlsv1.2 --fail --location --show-error \
  "https://github.com/MaiconFonsecaHk/projetarium-releases/releases/download/${VERSION}/install.sh" \
  -o /tmp/projetarium-install.sh

sudo bash /tmp/projetarium-install.sh --release "$VERSION"
```

Escolha a opção de atualização quando solicitada. O procedimento preserva banco, `.env` e uploads, executa backup e migrations e reinicia o serviço somente após validar os artefatos.

### Operação e diagnóstico

```bash
# Estado dos processos
sudo -u projetarium pm2 status

# Logs em tempo real
sudo -u projetarium pm2 logs projetarium

# Reiniciar a aplicação
sudo -u projetarium pm2 restart projetarium

# Parar a aplicação
sudo -u projetarium pm2 stop projetarium
```

| Caminho | Conteúdo |
|---|---|
| `/opt/projetarium/backend/` | Backend instalado |
| `/opt/projetarium/web/` | Aplicação Web |
| `/var/lib/projetarium/` | Uploads, anexos e dados operacionais |
| `/var/log/projetarium/` | Logs da aplicação |
| `/opt/projetarium/backend/.env` | Configuração protegida do servidor |
| `/etc/projetarium/release-public-key.pem` | Chave pública confiável para atualizações |

Não publique `.env`, dumps, logs completos, tokens, chaves ou anexos ao solicitar suporte.

## Artefatos e integridade das releases

Uma publicação Self-hosted contém, no mínimo:

- `projetarium-backend.tar.gz`;
- `projetarium-web.tar.gz`;
- `install.sh`;
- `release-public-key.pem`;
- `release-manifest.json`;
- `release-manifest.sig`.

Os pacotes Windows e os executáveis da CLI são distribuídos nos assets indicados por cada versão. Consulte sempre as notas da tag para confirmar plataformas, alterações de banco, compatibilidade da API e instruções de atualização.

## Suporte e relato de problemas

Abra uma [issue](../../issues) e informe:

- versão exata do Projetarium;
- edição Local ou Self-hosted;
- Windows e navegador, ou distribuição da VPS;
- passos mínimos para reproduzir;
- resultado esperado e resultado observado;
- capturas de tela ou trechos de log já removidos de dados pessoais e segredos.

Antes de relatar uma falha de dados, preserve uma cópia independente do banco ou dump e evite repetir operações destrutivas.

## Licença e distribuição

O Projetarium é distribuído no modelo **freemium**, mas seu código-fonte é proprietário e fechado. Os binários, instaladores e documentação de cada versão são regidos pelos termos de uso e licença que acompanham aquela release.

É proibido redistribuir, descompilar, sublicenciar ou publicar os artefatos fora das permissões expressamente concedidas. Baixe o Projetarium apenas deste repositório oficial e verifique a integridade dos arquivos antes da instalação.
