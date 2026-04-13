# Projetarium — Instalação em VPS

Projetarium é uma ferramenta de gestão de projetos e documentação. Este repositório contém os artefatos de instalação para servidores Ubuntu/Debian.

---

## Requisitos

| Item | Mínimo recomendado |
|---|---|
| Sistema operacional | Ubuntu 22.04 LTS ou Debian 12 |
| CPU | 1 vCPU |
| RAM | 1 GB |
| Disco | 10 GB |
| Portas abertas | 80 (HTTP), 443 (HTTPS) |
| Domínio | Registro A apontando para o IP da VPS **antes** de rodar o instalador |

> O instalador obtém certificado TLS automaticamente via Let's Encrypt (Certbot).  
> O domínio **precisa** resolver para o IP do servidor no momento da instalação.

---

## Instalação

### 1. Baixar o instalador

Acesse a [release mais recente](../../releases/latest) e baixe os três arquivos:

- `projetarium-backend.tar.gz`
- `projetarium-web.tar.gz`
- `install.sh`

Ou via terminal na própria VPS:

```bash
# Substitua X.Y.Z pela versão desejada
VERSION="X.Y.Z"
BASE="https://github.com/MaiconFonsecaHk/projetarium-releases/releases/download/v${VERSION}"

wget "$BASE/projetarium-backend.tar.gz" -P /tmp/
wget "$BASE/projetarium-web.tar.gz"     -P /tmp/
wget "$BASE/install.sh"                 -P /tmp/
```

### 2. Rodar o instalador

```bash
sudo bash /tmp/install.sh \
    --bundle     /tmp/projetarium-backend.tar.gz \
    --web-bundle /tmp/projetarium-web.tar.gz
```

O script é interativo e pedirá:

- **Domínio** — ex: `projetarium.seusite.com`
- **Usuário admin** — nome de usuário para o primeiro acesso
- **Nome de exibição** — nome que aparece no app
- **Senha do admin** — mínimo 8 caracteres
- **Senha do banco PostgreSQL** — gerada por você, nunca exposta publicamente

Confirme os dados e aguarde. A instalação completa leva cerca de **2 a 5 minutos**.

### 3. Acessar

Após a conclusão, abra no navegador:

```
https://SEU_DOMINIO
```

Use o usuário e senha que você definiu no passo anterior.

---

## O que o instalador configura

| Componente | Detalhe |
|---|---|
| Node.js 22 | Runtime do backend |
| PostgreSQL | Banco de dados |
| Nginx | Proxy reverso |
| Let's Encrypt | Certificado TLS automático |
| PM2 | Gerenciador de processos com autostart |

---

## Comandos úteis pós-instalação

```bash
# Status do serviço
sudo -u projetarium pm2 status

# Logs em tempo real
sudo -u projetarium pm2 logs projetarium

# Reiniciar
sudo -u projetarium pm2 restart projetarium

# Parar
sudo -u projetarium pm2 stop projetarium
```

Arquivos importantes:

| Caminho | Conteúdo |
|---|---|
| `/opt/projetarium/backend/` | Código do backend |
| `/opt/projetarium/web/` | App Flutter Web |
| `/var/lib/projetarium/` | Uploads e anexos |
| `/var/log/projetarium/` | Logs da aplicação |
| `/opt/projetarium/backend/.env` | Configurações (somente root) |

---

## Atualização

Baixe os novos artefatos da release mais recente e rode o instalador novamente.  
O script é idempotente: banco, usuário de sistema e Nginx já existentes são reaproveitados.  
**O usuário admin existente não é recriado** — apenas o schema é atualizado.

---

## Suporte

Abra uma [issue](../../issues) neste repositório.
