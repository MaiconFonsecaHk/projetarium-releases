# Projetarium — Instalação em VPS

Projetarium é uma ferramenta de gestão de projetos e documentação. Este repositório contém os artefatos de instalação para servidores Ubuntu/Debian.

---

## Antes de começar

Siga os passos abaixo **antes** de rodar o instalador. Pular qualquer um deles causa falha na instalação.

### 1. Provisionamento da VPS

| Requisito | Mínimo recomendado |
|---|---|
| Sistema operacional | Ubuntu 22.04 LTS ou Debian 12 |
| CPU | 1 vCPU |
| RAM | 1 GB |
| Disco | 10 GB |

### 2. Abrir as portas no firewall

O instalador precisa que as portas **80** (HTTP) e **443** (HTTPS) estejam acessíveis externamente. Sem elas o Certbot não consegue emitir o certificado TLS e a instalação falha.

Como abrir as portas varia conforme o provedor:

**Oracle Cloud (OCI)**
1. Acesse **Networking → Virtual Cloud Networks → sua VCN → Security Lists**
2. Adicione regras de entrada para TCP porta 80 e TCP porta 443, origem `0.0.0.0/0`
3. Na própria VPS, libere também no firewall do sistema:
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
```

**AWS / Lightsail**
1. Acesse o painel da instância → **Security Groups** (AWS) ou **Firewall** (Lightsail)
2. Adicione regras de entrada para HTTP (80) e HTTPS (443)

**DigitalOcean / Hetzner / Vultr / outros**
- Verifique se há um firewall de nível de nuvem ativo no painel do provedor e libere as portas 80 e 443
- Se usar UFW na VPS:
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
```

### 3. Apontar o domínio para a VPS

1. No painel do seu registrador de domínios (Registro.br, Cloudflare, GoDaddy etc.), crie um registro **A**:
   - **Nome:** subdomínio desejado (ex: `projetarium`)
   - **Tipo:** `A`
   - **Valor:** IP público da VPS
   - **TTL:** 300 (ou o mínimo disponível)

2. Aguarde a propagação do DNS (geralmente 1 a 5 minutos com TTL baixo).

3. Confirme que o domínio resolve antes de instalar:
```bash
ping projetarium.seusite.com
# Deve retornar o IP da sua VPS
```

> O instalador usa o Let's Encrypt para obter o certificado TLS automaticamente.
> O domínio **precisa** resolver para o IP do servidor no momento da instalação.

---

## Instalação

### 1. Baixar os arquivos

Acesse a [release mais recente](../../releases/latest) e baixe os três arquivos:

- `projetarium-backend.tar.gz`
- `projetarium-web.tar.gz`
- `install.sh`

Ou diretamente no terminal da VPS:

```bash
# Substitua X.Y.Z pela versão desejada (ex: 1.0.0)
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

Use o usuário e senha definidos no passo anterior.

---

## O que o instalador configura

| Componente | Detalhe |
|---|---|
| Node.js 22 | Runtime do backend |
| PostgreSQL | Banco de dados |
| Nginx | Proxy reverso |
| Let's Encrypt | Certificado TLS automático |
| PM2 | Gerenciador de processos com autostart no boot |

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
Quando detectar uma instalação existente, escolha **[2] Atualização** para preservar o banco de dados e os arquivos de upload.

---

## Suporte

Abra uma [issue](../../issues) neste repositório.
