# Telaria

Telaria is a collaborative mobile application for managing group trips. It brings
together in a single platform the tasks that are usually spread across several
tools: shared expense tracking and debt settlement, a common event calendar,
document storage, a group chat and an AI-based conversational assistant.

This project was developed as a Bachelor's Thesis (Traballo de Fin de Grao) for the
Degree in Computer Engineering at the Escola Técnica Superior de Enxeñaría,
Universidade de Santiago de Compostela.

## Project status

The thesis report and its oral presentation date from **July 2026**. Both are
included in this repository (see [Repository structure](#repository-structure)) and
describe the system as it stood at that time.

The project has continued to evolve since then. As a result, parts of the code may
no longer match the report exactly, whether in implementation details, the API,
the data model or the user interface. When the report and the source code
disagree, the source code reflects the current state of the project.

## Features

- User registration and authentication with JWT access tokens and refresh tokens.
- Trip creation and management, with invitations, join requests and QR codes for
  joining.
- Friends system: friend requests by username and a friends list integrated into
  trip invitations.
- Shared expenses by category, balance calculation and debt settlement.
- Shared trip calendar with events and locations.
- Document sharing, with object storage and thumbnail generation.
- Real-time group chat for each trip.
- AI travel assistant powered by a locally hosted language model.
- Interface available in Galician, Spanish and English, with light and dark themes.

## Architecture and technologies

The system follows an API-first approach: the REST API is defined in an OpenAPI
specification, from which the backend server interfaces and the frontend
TypeScript types are generated.

| Component       | Technologies                                                                  |
|-----------------|-------------------------------------------------------------------------------|
| Mobile frontend | React Native 0.81, Expo 54, Expo Router, TanStack Query, i18next, TypeScript  |
| Backend         | Java 25, Spring Boot 4, Spring Security (OAuth2 resource server), Spring Data JPA |
| API contract    | OpenAPI 3, OpenAPI Generator, openapi-typescript, springdoc                   |
| Database        | PostgreSQL 16                                                                 |
| File storage    | MinIO (S3-compatible)                                                         |
| Language model  | Ollama running `llama3.2:3b`                                                  |
| Infrastructure  | Containers managed with Podman Compose (Docker-compatible)                    |
| Testing         | JUnit, Testcontainers, JaCoCo, SpotBugs, Jest, React Native Testing Library   |

## Repository structure

```
.
├── backend/        Spring Boot REST API, OpenAPI specification and container setup
├── frontend/       Expo / React Native mobile application
├── memoria/        Thesis report (LaTeX, in Galician), including technical and user manuals
├── presentacion/   Slides for the thesis defence (LaTeX Beamer)
└── .github/        Continuous integration workflows
```

## Getting started

### Requirements

- JDK 25
- Node.js and npm
- Podman with Podman Compose (Docker with Docker Compose also works)
- An Android device or emulator with Expo Go, or a development build
- Optional: an NVIDIA GPU to speed up the language model

### Backend

From the `backend/` directory:

```bash
./start.sh
```

The script starts the PostgreSQL, MinIO and Ollama containers (using the GPU
profile if an NVIDIA GPU is detected), downloads the language model, waits for the
database to be ready and then launches the application with `./gradlew bootRun`.

By default the API listens on port `8082`. The interactive API documentation is
available through Swagger UI at `http://localhost:8082/swagger-ui.html`.

Configuration lives in `backend/src/main/resources/application.yaml`. The
credentials included there are for local development only and must be changed in
any other environment.

### Frontend

From the `frontend/` directory:

```bash
npm install
npx expo start
```

Before running the application, set `BASE_URL` in
`frontend/constants/constants.tsx` to the network address of the machine running
the backend. The `public-endpoint` property for MinIO in the backend configuration
must also point to an address the mobile device can reach.

If the OpenAPI specification changes, regenerate the frontend types with:

```bash
npm run generate:types
```

## Testing

Backend tests (unit and integration tests, the latter using Testcontainers):

```bash
cd backend
./gradlew test
```

When using rootless Podman instead of Docker, Testcontainers needs the
`DOCKER_HOST` variable to point at the Podman socket.

Frontend tests:

```bash
cd frontend
npm test
```

## Documentation

- `memoria/`: full thesis report in Galician, covering requirements, design,
  testing and conclusions. The technical manual and the user manual are included as
  appendices.
- `presentacion/`: slides used for the thesis defence.

As noted in [Project status](#project-status), both documents reflect the state of
the project in July 2026.

## License

This project is distributed under the MIT License, as stated in the corresponding
appendix of the thesis report.

## Author

Jorge Otero Pailos
