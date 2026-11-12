# analyze-youtube-comments

Один фронтенд (`frontend/`) и несколько реализаций backend с одинаковым API (`backend-*/`).

## Общая инфраструктура

`docker-compose.yml` в этой папке поднимает Redis и Postgres для всех реализаций
в сети `ayc-infra`:

```bash
docker compose up -d      # из этой папки, до запуска любого backend
```

Снаружи доступны `localhost:6379` и `localhost:5432` (пользователь и пароль `app`).
Если на этих портах уже работают локальные Redis или Postgres (например, из Homebrew),
их нужно остановить: `brew services stop redis`.

Каждый backend подключается к сети как к внешней и сам создаёт свои базы (рабочую и тестовую):

| Папка | Redis DB | База Postgres | Тестовая база |
|---|---|---|---|
| `backend-python` | 0 | `analysis_python` | `analysis_python_test` |
| `backend-go` | 1 | `analysis_go` | `analysis_go_test` |
| `backend-node` | 2 | `analysis_node` | `analysis_node_test` |
| `backend-node-fastify` | 10 | `analysis_node_fastify` | `analysis_node_fastify_test` |
| `backend-rust` | 3 | `analysis_rust` | `analysis_rust_test` |
| `backend-java` | 4 | `analysis_java` | `analysis_java_test` |
| `backend-kotlin` | 5 | `analysis_kotlin` | `analysis_kotlin_test` |
| `backend-net` | 6 | `analysis_dotnet` | `analysis_dotnet_test` |
| `backend-sinatra` | 7 | `analysis_sinatra` | `analysis_sinatra_test` |
| `backend-roda` | 8 | `analysis_roda` | `analysis_roda_test` |
| `backend-hanami` | 11 | `analysis_hanami` | `analysis_hanami_test` |
| `backend-elixir` | 9 (если Redis используется) | `analysis_elixir` | `analysis_elixir_test` |

Номера баз Redis изолируют ключи, но Pub/Sub-каналы, `FLUSHALL` и лимиты памяти у них общие.
