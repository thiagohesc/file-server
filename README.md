# File Server (NGINX) + Traefik - Docker Compose Setup

Este projeto configura um servidor de arquivos estáticos usando NGINX e proxy reverso seguro com Traefik, ideal para distribuição de arquivos HTML, PDFs, imagens, instaladores e outros conteúdos web.

## 📦 Serviço: File Server (NGINX)

### ⚙️ Configuração:

- **Imagem:** `nginx:alpine`
- **Volume:**
  - `${FILES_PATH}:/usr/share/nginx/html:ro` → caminho onde ficam os arquivos estáticos.
  - `./nginx_conf/default.conf:/etc/nginx/conf.d/default.conf:ro` → configuração customizada do NGINX.
- **Rede:** `rpg-net` (externa)
- **Restart policy:** `always`

### 🌐 Acesso via Traefik:

- **Labels:**
  - `traefik.enable=true` → habilita o roteamento via Traefik
  - `Host(\`${DOMAIN}\`)` → domínio de acesso ao file server
  - `websecure` com HTTPS e certificado automático (`certresolver=le`)
  - Redirecionamento da porta interna `80`

---

## ✅ Como usar

1. Crie o arquivo `.env`:

```env
DOMAIN=arquivos.seudominio.com
FILES_PATH=/caminho/para/seus/arquivos
```

2. Estrutura de pastas recomendada:

```
.
├── docker-compose.yml
├── .env
├── nginx_conf/
│   └── default.conf           # Configuração NGINX personalizada
└── arquivos/                  # Arquivos que serão servidos
```

3. Suba o container:

```bash
docker compose up -d
```

4. Acesse via navegador:

```
https://${DOMAIN}
```

---

## 🛠️ Requisitos

- Docker
- Docker Compose
- Traefik configurado e rodando na rede `rpg-net`
