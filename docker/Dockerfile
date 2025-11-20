# Usa a imagem oficial completa do n8n
# FROM n8nio/n8n:1.110.0
# FROM n8nio/n8n:latest
FROM n8nio/n8n:1.120.3

USER root

# Modo produção e mais memória para libs JS pesadas
ENV NODE_ENV=production
ENV NODE_OPTIONS="--max-old-space-size=2048"

# Torna módulos globais visíveis para o require() no Code node
ENV NODE_PATH="/usr/local/lib/node_modules"

# Dependências de build (para módulos nativos) + git (opcional, para npm de repositórios)
RUN if command -v apt-get >/dev/null 2>&1; then \
      apt-get update && apt-get install -y --no-install-recommends \
        build-essential python3 ca-certificates git && \
      rm -rf /var/lib/apt/lists/*; \
    elif command -v apk >/dev/null 2>&1; then \
      apk add --no-cache \
        build-base python3 ca-certificates git; \
    fi

# Instala pacotes globais úteis ao Code node (JWT e HTTP client)
# --omit=dev evita dependências desnecessárias em produção
RUN npm config set fund false \
 && npm config set audit false \
 && npm config set update-notifier false \
 && npm i -g --omit=dev jsonwebtoken@^9 axios@^1



# Healthcheck simples (verifica /rest/health)
HEALTHCHECK --interval=30s --timeout=5s --start-period=40s --retries=5 \
  CMD node -e "http=require('http');http.get('http://127.0.0.1:5678/rest/health',r=>{process.exit(r.statusCode===200?0:1)}).on('error',()=>process.exit(1))"

# A imagem do n8n já usa 'node', garantimos aqui
USER node
