# TUTOR_NOTES.md — notas para o tutor avaliar o projecto
## O que foi implementado
### product-service (Labs 1 & 2)
- REST API CRUD completo: GET, POST, PUT, DELETE /api/products
- Persistência em PostgreSQL via Spring Data JPA
- Swagger/OpenAPI em /swagger-ui/index.html
### order-service (Lab 2)
- REST API de encomendas: POST /api/orders, GET /api/orders
- Comunicação com product-service via RestTemplate (PRODUCT_SERVICE_URL)
- Verificação de stock antes de aceitar encomenda
### Docker & Docker Compose (Labs 3 & 4)
- Dockerfile para cada serviço (eclipse-temurin:17-jre-alpine)
- docker-compose.yml com 3 serviços: db, product-service, order-service
- healthcheck no PostgreSQL (pg_isready) com depends_on condition
- Volume postgres-data para persistência após docker compose down
- Comunicação inter-serviço via nomes DNS internos do Docker
## Desafios implementados (opcional)
- [ ] Multi-stage build no Dockerfile (Maven → JRE alpine)
- [ ] .dockerignore criado
- [ ] pgAdmin adicionado ao docker-compose.yml
## Dificuldades encontradas
- [Descrever as principais dificuldades e como foram resolvidas]
## Tempo de desenvolvimento
- Labs 1 & 2: aproximadamente X horas
- Labs 3 & 4: aproximadamente X horas