# 🍽️ A Tasca do Microserviço

> Labs 3 & 4 de **Microservices + DevOps + Cloud Academy 2026**

Projeto com **Product Service** e **Order Service** containerizados com **Docker Compose** e **PostgreSQL**.

---

# 👨‍🎓 Informações do Aluno

| Campo | Valor |
|------|------|
| Nome | SEU NOME COMPLETO |
| Turma | Microservices Academy 2026 |
| Labs | Lab 3 & 4 – Dia 2 |
| Data de entrega | DATA |
| Email | SEU EMAIL |

---

# 🏗️ Arquitectura

```
Docker Network

+------------------+       +------------------+
|  product-service | <---- |   order-service  |
|       :8080      |       |       :8081      |
|    Spring Boot   |       |    Spring Boot   |
+--------+---------+       +--------+---------+
         |                          |
         +-----------+--------------+
                     |
              +-------------+
              | PostgreSQL  |
              |    :5432    |
              |  db:tascadb |
              +-------------+
```

---

# 📦 Serviços

| Serviço | Porta | Responsabilidade |
|--------|------|------------------|
| product-service | 8080 | CRUD de produtos (ementa da Tasca) |
| order-service | 8081 | Gestão de encomendas (pedidos de mesa) |
| PostgreSQL | 5432 | Persistência de produtos e encomendas |

---

# ⚡ Execução

## Pré-requisitos

- Docker Desktop instalado e em execução
- Java 17 + Maven (para compilar os JARs antes do build Docker)

---

# Passo 1 – Clonar o projeto

```bash
git clone https://github.com/SEU_USERNAME/tasca-do-microservico.git
cd tasca-do-microservico
```

---

# Passo 2 – Compilar os microserviços

### Compilar product-service

```bash
cd product-service
./mvnw clean package -DskipTests
cd ..
```

### Compilar order-service

```bash
cd order-service
./mvnw clean package -DskipTests
cd ..
```

---

# Passo 3 – Iniciar com Docker Compose

```bash
docker compose up --build
```

⏱ **Primeira execução:** pode demorar **2–4 minutos**

Aguardar até aparecer nos logs:

```
Started ProductServiceApplication
Started OrderServiceApplication
```

---

# 🔎 Verificar serviços

```bash
docker compose ps
```

Os seguintes serviços devem aparecer com **Status: Up**

- db
- product-service
- order-service

Testar endpoints:

```bash
curl http://localhost:8080/api/products
curl http://localhost:8081/api/orders
```

---

# 🧪 Testar o Fluxo Completo

## 1️⃣ Criar um produto

```bash
curl -X POST http://localhost:8080/api/products \
-H "Content-Type: application/json" \
-d '{"name":"Bacalhau com Natas","description":"O clássico","price":14.50,"quantity":50}'
```

Resposta esperada:

```
{"id":1,"name":"Bacalhau com Natas"}
```

---

## 2️⃣ Criar uma encomenda

```bash
curl -X POST http://localhost:8081/api/orders \
-H "Content-Type: application/json" \
-d '{"productId":1,"quantity":2,"customerName":"Zé das Couves"}'
```

---

## 3️⃣ Listar encomendas

```bash
curl http://localhost:8081/api/orders
```

---

# 🗄️ Inspecionar a Base de Dados

## Aceder ao PostgreSQL

```bash
docker exec -it tasca-db psql -U zecouves -d tascadb
```

Dentro do `psql`:

```sql
SELECT * FROM product;
SELECT * FROM orders;
\q
```

---

# 🌐 Swagger UI

| Serviço | URL |
|--------|-----|
| product-service | http://localhost:8080/swagger-ui/index.html |
| order-service | http://localhost:8081/swagger-ui/index.html |

---

# 🧹 Parar e Limpar

Parar containers:

```bash
docker compose down
```

Reset total (apaga a base de dados):

```bash
docker compose down -v
```

---

# 📁 Estrutura do Projecto

```
tasca-do-microservico/

product-service
 ├─ src/main/java
 │   ├─ controller/ProductController.java
 │   ├─ model/Product.java
 │   └─ repository/ProductRepository.java
 ├─ Dockerfile
 └─ pom.xml

order-service
 ├─ src/main/java
 │   ├─ controller/OrderController.java
 │   ├─ model/Order.java
 │   └─ service/OrderService.java
 ├─ Dockerfile
 └─ pom.xml

docker-compose.yml
.gitignore
README.md
```

---

# 🛠️ Troubleshooting

## JAR file not found durante docker build

Não esquecer de compilar antes do build:

```bash
cd product-service && ./mvnw clean package -DskipTests && cd ..
cd order-service && ./mvnw clean package -DskipTests && cd ..

docker compose up --build
```

---

## Connection refused à base de dados

Spring Boot pode arrancar antes do PostgreSQL.

Reiniciar serviços:

```bash
docker compose restart product-service
docker compose restart order-service
```

---

## Order Service não encontra Product Service

Confirmar variável no `docker-compose.yml`

```
PRODUCT_SERVICE_URL=http://product-service:8080
```

O hostname deve ser **product-service**, não **localhost**.

Ver logs:

```bash
docker compose logs order-service
```

---

## Porta já em uso

```bash
docker compose down
docker compose up --build
```

Ou verificar o processo que usa a porta:

Mac / Linux:

```bash
lsof -i :8080
```

---

# ✅ Resultado Esperado

Após executar todos os passos:

- **product-service** disponível em `http://localhost:8080`
- **order-service** disponível em `http://localhost:8081`
- **PostgreSQL** na porta `5432`
- Comunicação entre microserviços dentro da **Docker Network**
- APIs documentadas com **Swagger UI**

---

# 📚 Microservices + DevOps + Cloud Academy 2026

Laboratórios **3 & 4 — Dia 2**

Projeto: **Tasca do Microserviço**