# Usa a imagem oficial completa do n8n
FROM n8n:latest

# --- (Opcional, mas recomendado) aumentar heap se for rodar libs pesadas ---
ENV NODE_OPTIONS="--max-old-space-size=2048"

# Torna módulos globais visíveis para o "require()" do Code node
ENV NODE_PATH="/usr/local/lib/node_modules"

# Habilita uso de pacotes externos no Code node (pode controlar via compose também)
ENV N8N_CODE_NODE_FUNCTION_ALLOW_EXTERNAL="true"
  
# Liste os módulos que você quer liberar (vírgula-separado)
ENV N8N_CODE_NODE_FUNCTION_EXTERNAL_MODULES="jsonwebtoken,axios"

# Dependências de build para módulos nativos (funciona em debian/alpine)
# - Instala só se existir o gerenciador correspondente
RUN if command -v apt-get >/dev/null 2>&1; then \
      apt-get update && apt-get install -y --no-install-recommends \
        build-essential python3 ca-certificates && \
      rm -rf /var/lib/apt/lists/*; \
    elif command -v apk >/dev/null 2>&1; then \
      apk add --no-cache \
        build-base python3 ca-certificates; \
    fi

# Instala pacotes globais úteis para Code node (jwt incluso)
# --omit=dev evita instalar dependências de dev desnecessárias
RUN npm config set fund false \
 && npm config set audit false \
 && npm config set update-notifier false \
 && npm i -g --omit=dev \
      jsonwebtoken@^9 \
      axios@^1

# Garante usuário correto (a imagem do n8n já costuma definir)
USER node
