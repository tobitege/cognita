FROM node:18.2.0 AS build-step
WORKDIR /build
COPY . .
RUN yarn install
RUN test -e .env || cp .env.example .env
ARG VITE_USE_RELATIVE_BASE_URL VITE_DOCS_QA_STANDALONE_PATH VITE_QA_FOUNDRY_URL
ENV VITE_USE_RELATIVE_BASE_URL=${VITE_USE_RELATIVE_BASE_URL} \
    VITE_DOCS_QA_STANDALONE_PATH=${VITE_DOCS_QA_STANDALONE_PATH}
RUN if [ -n "$VITE_QA_FOUNDRY_URL" ]; then \
        export VITE_QA_FOUNDRY_URL=${VITE_QA_FOUNDRY_URL}; \
    fi
RUN yarn build

FROM node:18.2.0 AS production-stage
RUN npm install -g serve
WORKDIR /app
COPY --from=build-step /build/dist /app/dist
EXPOSE 5000
CMD ["serve", "-s", "dist", "-l", "5000"]
