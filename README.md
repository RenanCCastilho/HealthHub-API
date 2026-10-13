# Health Hub API

![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.1-6DB33F?logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-BCrypt-6DB33F?logo=springsecurity&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle_DB-F80000?logo=oracle&logoColor=white)

API REST do **Health Hub**, uma plataforma de acompanhamento de saúde. O usuário registra batimentos cardíacos, pressão arterial e nível de glicose, e mantém um histórico consultável dessas medições.

Ela faz parte de uma solução maior, que inclui um app com calculadora de taxa metabólica e um dispositivo IoT em Arduino ([GS-IA](https://github.com/RenanCCastilho/GS-IA)). Esse dispositivo sinaliza por LEDs se cada indicador está normal, em atenção ou crítico.

## Funcionalidades

- CRUD de usuários, com senha armazenada usando hash **BCrypt**
- CRUD de registros de saúde vinculados a um usuário
- Busca paginada de registros, com filtro opcional pelo nome do usuário
- Autenticação HTTP Basic com Spring Security
- Tratamento de exceções por domínio e CORS configurado para o front-end

## Arquitetura

```
HealthHub/src/main/java/br/com/fiap
├── controller   # Endpoints REST
├── service      # Regras de negócio
├── repository   # Spring Data JPA
├── model        # Entidades (Usuario, RegistroSaude)
├── exception    # Exceções de domínio
└── config       # Segurança e CORS
```

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/usuarios` | Lista usuários |
| `GET` | `/usuarios/{id}` | Busca usuário por id |
| `POST` | `/usuarios` | Cadastra usuário |
| `PUT` | `/usuarios/{id}` | Atualiza usuário |
| `DELETE` | `/usuarios/{id}` | Remove usuário |
| `GET` | `/registros-saude` | Lista registros de saúde |
| `GET` | `/registros-saude/{id}` | Busca registro por id |
| `GET` | `/registros-saude/search?nomeUsuario=&page=&size=` | Busca paginada |
| `POST` | `/registros-saude` | Cadastra registro |
| `PUT` | `/registros-saude/{id}` | Atualiza registro |
| `DELETE` | `/registros-saude/{id}` | Remove registro |

**Exemplo — cadastrar registro de saúde**

```json
POST /registros-saude
{
  "usuario": { "id": 1 },
  "pressaoArterial": 120.0,
  "batimentosCardiacos": 80,
  "nivelGlicose": 90.5,
  "dataRegistro": "2023-11-16"
}
```

## Como executar

Pré-requisitos: Java 17 e acesso a um banco Oracle.

```bash
cd HealthHub
export DB_USERNAME=seu_usuario
export DB_PASSWORD=sua_senha
# opcionais: DB_URL (padrão: Oracle da FIAP) e ADMIN_PASSWORD (padrão: admin123)
./mvnw spring-boot:run
```

A API sobe em `http://localhost:8080`. O usuário de acesso é `admin` e a senha é o valor de `ADMIN_PASSWORD`.

## Deploy em nuvem

A versão com pipeline de CI/CD para o Azure App Service está em [HelthHub-API-Nuvem](https://github.com/RenanCCastilho/HelthHub-API-Nuvem).

---

Projeto acadêmico desenvolvido na **FIAP** (Global Solution, 2023).
