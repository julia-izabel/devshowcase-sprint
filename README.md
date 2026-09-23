# DevShowcase API — Spring Boot

API REST em Java 17 + Spring Boot 3, com arquitetura em camadas (Controller → Service → Repository), DTOs, tratamento global de exceções e documentação Swagger/OpenAPI.

## Endpoints

- `POST /api/projects/{id}/feedbacks` — cadastra nota (1–5) e comentário; recalcula a média do projeto.
- `PUT /api/projects/{id}/upvote` — incrementa o contador de upvotes.
- `GET /api/projects?technology=Java&page=0&size=10` — lista projetos com filtro e paginação.

Swagger UI: `/swagger-ui.html`

## Rodando localmente

```bash
export DATABASE_URL=jdbc:postgresql://localhost:5432/devshowcase
export DATABASE_USERNAME=postgres
export DATABASE_PASSWORD=postgres
./mvnw spring-boot:run
```

## Deploy em produção (Render + Supabase)

1. **Banco de dados (Supabase)**
   - Crie um projeto no Supabase → copie a `Connection string` (modo *Session pooler*, porta 5432 ou 6543).
   - Monte a URL JDBC: `jdbc:postgresql://<host>:<porta>/postgres`.

2. **Repositório no GitHub**
   - Suba este projeto para um repositório público/privado no GitHub.

3. **Deploy no Render**
   - New → Web Service → conecte o repositório.
   - Runtime: **Docker** (ou Java, se disponível) — Build Command: `./mvnw clean package -DskipTests` — Start Command: `java -jar target/devshowcase-api-1.0.0.jar`.
   - Em **Environment**, configure as variáveis:
     - `DATABASE_URL=jdbc:postgresql://<host-supabase>:5432/postgres`
     - `DATABASE_USERNAME=postgres`
     - `DATABASE_PASSWORD=<senha do supabase>`
     - `DDL_AUTO=update`
     - `PORT=8080` (o Render injeta a própria porta automaticamente também)
   - Ative **Auto-Deploy** a partir do branch `main`.

4. Ao concluir, a API ficará disponível em `https://seu-servico.onrender.com`, com Swagger em `https://seu-servico.onrender.com/swagger-ui.html`.
