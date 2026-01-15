# PRD: Portfolio Backend & CMS

## 1. Project Overview
The objective is build a dynamic backend powered by **Node.js/Express** and **MySQL**. This allows for real-time content management.

## 2. Technical Stack
- **Database**: MySQL 8.0+
- **Backend**: Node.js (TypeScript) + Express.
- **ORM**: Prisma (Recommended for type-safe schema management).
- **Authentication**: JWT for CMS access.
- **Frontend Markdown**: `react-markdown` or `markdown-it`.

## 3. Database Schema (MySQL)

### 3.1. Entity Relationship Summary
The database uses a relational structure where most tables are independent except for `project_tags` which links to `projects`.

### 3.2. Complete Table Definitions

#### `projects`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| id | INT | PK, AI | Unique ID |
| title | VARCHAR(150) | | Project name |
| description | TEXT | | Short overview |
| period | VARCHAR(50) | | e.g., "2024.01 — 2024.06" |
| thumbnailUrl | TEXT | | Preview image URL |
| link | VARCHAR(255) | | Live site URL |
| github | VARCHAR(255) | | Repo URL |

#### `project_tags`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| id | INT | PK, AI | Unique ID |
| project_id | INT | FK (projects.id) | Linked project |
| tag_name | VARCHAR(50) | | Tech tag (e.g., "React") |

#### `tech_stack`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| id | INT | PK, AI | Unique ID |
| name | VARCHAR(100) | | Full tech name |
| slug | VARCHAR(100) | | SimpleIcons slug |
| color | VARCHAR(20) | | Hex or brand color |
| url | VARCHAR(255) | | Documentation link |

#### `admins`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| id | INT | PK, AI | Unique ID |
| username | VARCHAR(50) | UNIQUE | Login identifier |
| password_hash | VARCHAR(255) | | Bcrypt hash |

## 4. Functional Requirements

### 4.1. Public API
- **`GET /api/portfolio`**: Core endpoint for site landing page.

### 4.2. CMS API (Protected)
- **`POST /api/portfolio`**: Create with portfolio.
- **`PUT /api/portfolio/:id`**: Update existing portfolio.
- **`DELETE /api/portfolio/:id`**: Remove portfolio.


## Development Checklist

### Phase 1: Environment & Database Setup
- [ ] Use .env file for database connection.
- [ ] Initialize Node.js/TypeScript for the backend.
- [ ] Configure Prisma with all tables defined above.
- [ ] Create migrations and seed admin user.

### Phase 2: Core Public API
- [ ] Implement `GET /api/portfolio` with joined data (projects with tags).

### Phase 3: Authentication & CMS
- [ ] Implement JWT Login.
- [ ] Build CRUD for projects.

### Phase 4: Integration
- [ ] Update frontend components to handle routing.
