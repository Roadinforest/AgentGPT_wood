# Use Node.js Alpine
FROM node:19-alpine

ARG NODE_ENV
ENV NODE_ENV=$NODE_ENV

# Install required dependencies
RUN apk add --no-cache \
    netcat-openbsd \
    dos2unix \
    bash \
    gcc \
    g++ \
    make \
    python3 \
    openjdk11

# Working directory
WORKDIR /next

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# wait-for-db script
COPY wait-for-db.sh /usr/local/bin/wait-for-db.sh
RUN chmod +x /usr/local/bin/wait-for-db.sh

# Copy app code
COPY . .
COPY entrypoint.sh /

# Normalize line endings
RUN dos2unix /entrypoint.sh

EXPOSE 3001

ENTRYPOINT ["sh", "/entrypoint.sh"]
CMD ["npm", "run", "dev"]
