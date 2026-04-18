# JO2024 — Plateforme de billetterie Paris 2024

Application full-stack de gestion et de billetterie pour les Jeux Olympiques Paris 2024 : achat de billets, génération de QR codes, scan en entrée et administration des événements.

---

## Documentation

| Document | Description |
|---|---|
| [Guide utilisateur](doc/Guide/USER-GUIDE.md) | Parcours complets : utilisateur, staff, administrateur |
| [Améliorations v2](doc/Enhancements/améliorations.md) | Roadmap et évolutions prévues |
| [MCD](doc/MCD/MCD.png) | Modèle conceptuel de données |
| [Architecture](doc/graphs/Architecture/Architecture%20JO.png) | Diagramme d'architecture de l'application |
| [Diagramme de séquence — réservation](doc/graphs/Diagrammes%20séquences/diagramme-séquence-réservation.png) | Flux de réservation d'un billet |
| [Plan de test fonctionnel](doc/Plan%20de%20test%20fonctionnel.csv) | Cas de test fonctionnels |
| [Analyse des risques](doc/Analyse%20des%20risques.csv) | Identification et évaluation des risques |
| [Process & Documentation](doc/Process%20&%20Documentation.pdf) | Documentation de processus |

---

## Stack technique

| Couche | Technologies |
|---|---|
| API | Java 21 · Spring Boot 4.0 · Spring Security · JWT · Liquibase |
| Base de données | PostgreSQL 18 |
| Frontend | Next.js 16 · TypeScript · Tailwind CSS · Radix UI · Zustand |
| Infrastructure | Docker · Docker Compose |

---

## Lancer le projet en local

### Prérequis

- [Docker](https://docs.docker.com/get-docker/) et Docker Compose
- Java 21 + Maven (pour le développement API sans Docker)
- Node.js 20+ + pnpm (pour le développement UI sans Docker)

---

### Option 1 — Docker Compose (recommandé)

Lance l'ensemble de la stack (base de données, API, frontend) en une seule commande.

**1. Variables d'environnement**

Copier et remplir les fichiers d'exemple :

```bash
# Variables PostgreSQL (racine du projet)
cp .env.example .env

# Variables de l'API
cp api-jo2024/.env.example api-jo2024/.env.prod
```

| Variable (`.env`) | Description |
|---|---|
| `POSTGRES_DB` | Nom de la base de données |
| `POSTGRES_USER` | Utilisateur PostgreSQL |
| `POSTGRES_PASSWORD` | Mot de passe PostgreSQL |

| Variable (`api-jo2024/.env.prod`) | Description |
|---|---|
| `DATABASE_URL` | URL JDBC de connexion à PostgreSQL |
| `DB_USER` / `DB_PASSWORD` | Identifiants de la base |
| `JWT_ACCESS_SECRET` | Clé secrète JWT (Base64) pour les access tokens |
| `JWT_REFRESH_SECRET` | Clé secrète JWT (Base64) pour les refresh tokens |
| `ALLOWED_ORIGINS` | Origines autorisées par CORS (ex: `http://localhost:3000`) |
| `SPRING_PROFILES_ACTIVE` | Profil Spring actif (ex: `prod`) |

**2. Lancer**

```bash
docker compose up --build
```

| Service | URL |
|---|---|
| Frontend | http://localhost:3000 |
| API | http://localhost:8000 |
| Swagger UI | http://localhost:8000/swagger-ui.html |

---

### Option 2 — Développement local (sans Docker)

#### Base de données

Démarrer une instance PostgreSQL locale et créer la base. Liquibase applique les migrations automatiquement au démarrage de l'API.

#### API (Spring Boot)

```bash
cd api-jo2024
cp .env.example .env.dev
# Remplir .env.dev avec les valeurs locales
./mvnw spring-boot:run
```

L'API démarre sur le port défini dans `SERVER_PORT` (par défaut `8000`).

#### Frontend (Next.js)

```bash
cd ui-jo2024
cp .env.example .env.local
# Vérifier que API_BASE_URL pointe vers l'API locale
pnpm install
pnpm dev
```

Le frontend démarre sur http://localhost:3000.
