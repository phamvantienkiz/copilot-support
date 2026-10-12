- backend/
  - cmd/
    - embed/
      - main.go

    - server/
      - main.go
      - retriever.go

  - internal/
    - agent/
      - agent.go
      - agent_test.go
      - events.go
      - prompt.go
      - prompt_test.go
      - records.go
      - run_tool.go
      - tools.go

    - assist/
      - testdata/
        - blender-complaint-no-customer.golden
        - blender-complaint-selected-customer.golden
        - blender-model-reply.json

      - cost_test.go
      - finalize.go
      - finalize_profile.go
      - finalize_profile_test.go
      - finalize_test.go
      - format.go
      - gather.go
      - grounding.go
      - helpers_test.go
      - prompt.go
      - prompt_test.go
      - render.go
      - request.go
      - request_test.go
      - service.go
      - types.go

    - catalog/
      - catalog.go
      - catalog_test.go
      - check.go
      - search_documents.go
      - types.go

    - config/
      - config.go
      - config_test.go
      - dotenv.go

    - customerstats/
      - calculator.go
      - customerstats.go
      - customerstats_test.go

    - embeddings/
      - embeddings_test.go
      - store.go
      - sync.go

    - httpserver/
      - agent_handler.go
      - agent_handler_test.go
      - api_handlers.go
      - assist_handler_test.go
      - customer_handlers.go
      - data_handlers_test.go
      - errors.go
      - server.go
      - server_test.go
      - spa_handler.go
      - testenv_test.go

    - llm/
      - client.go
      - cost_test.go
      - embeddings.go
      - json_mode.go
      - json_mode_test.go
      - meter.go
      - tools.go
      - tools_embeddings_test.go

    - retrieval/
      - bm25.go
      - bm25_test.go
      - hybrid.go
      - hybrid_fixtures_test.go
      - hybrid_test.go
      - retriever.go
      - tokenizer.go
      - tokenizer_test.go

    - storage/
      - sqlite/
        - sqlite.go
        - sqlite_test.go
        - vector.go
        - vector_test.go

  - go.mod
  - go.sum

- frontend/
  - public/
    - replay/
    - favicon.svg

  - scripts/
  - src/
    - components/
      - chat/
      - copilot/
      - customer/
      - demo/
      - operator/
      - shell/
      - ui/
      - agent-trace-flow.test.tsx
      - replay-mode-flow.test.tsx
      - theme-toggle.tsx
      - workspace-flow.test.tsx

    - hooks/
    - lib/
    - state/
    - test-fixtures/
    - app.tsx
    - index.css
    - main.tsx
    - vite-env.d.ts

  - index.html
  - package.json
  - pnpm-lock.yaml
  - tsconfig.json
  - vite.config.ts
  - vitest.config.ts

- data/
  - README.md
  - complaint-cases.json
  - customers.json
  - embeddings.sqlite
  - orders.json
  - products.json

- spec/
- docs/
  - build-journal/
  - design/
  - media/
    - screens/

  - phases/
  - build-timeline.json

- .env.example
- .gitignore
- LICENSE
- Makefile
- README.md
