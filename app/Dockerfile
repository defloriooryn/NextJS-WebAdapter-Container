FROM public.ecr.aws/docker/library/node:16.17.0-slim as builder
WORKDIR /app

COPY . .

RUN npm ci && npm run build


FROM public.ecr.aws/docker/library/node:16.17.0-slim as runner
WORKDIR /app

COPY --from=public.ecr.aws/awsguru/aws-lambda-adapter:0.5.0 /lambda-adapter /opt/extensions/lambda-adapter
ENV PORT=3000 
ENV NODE_ENV=production

COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

CMD ["node", "server.js"]
