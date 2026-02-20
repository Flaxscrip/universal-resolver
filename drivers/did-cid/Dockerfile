# Universal Resolver Driver for did:cid
FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY server.js ./

ENV PORT=4250
ENV ARCHON_GATEKEEPER_URL=http://localhost:4224

EXPOSE 4250
HEALTHCHECK --interval=30s --timeout=5s CMD wget -qO- http://localhost:4250/health || exit 1

CMD ["node", "server.js"]
