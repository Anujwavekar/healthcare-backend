# Healthcare Backend

A backend for a healthcare application built with Django, Django REST Framework,
PostgreSQL and JWT authentication. Users can register, log in, and manage
patients, doctors, and patient-doctor assignments.

## Tech Stack
- Python, Django, Django REST Framework
- PostgreSQL 15 or later
- JWT authentication (djangorestframework-simplejwt)
- Environment variables via python-dotenv

## Project Structure
```
healthcare_backend/
├── config/        # project settings and root URLs
├── users/         # custom user model, register, login
├── patients/      # patient CRUD
├── doctors/       # doctor CRUD
├── mappings/      # patient-doctor assignments
├── manage.py
├── requirements.txt
└── .env.example
```

## Setup

1. Clone the repository and open the project folder.
2. Create and activate a virtual environment:
```bash
   python -m venv venv
   venv\Scripts\activate        # Windows
   source venv/bin/activate     # macOS / Linux
```
3. Install dependencies:
```bash
   pip install -r requirements.txt
```
4. Create a PostgreSQL database:
```sql
   CREATE DATABASE healthcare_db;
```
5. Copy `.env.example` to `.env` and fill in your values.
6. Run migrations and start the server:
```bash
   python manage.py migrate
   python manage.py runserver
```

## Environment Variables

| Variable | Description |
|---|---|
| SECRET_KEY | Django secret key |
| DEBUG | True or False |
| DB_NAME | PostgreSQL database name |
| DB_USER | PostgreSQL user |
| DB_PASSWORD | PostgreSQL password |
| DB_HOST | Database host (e.g. localhost) |
| DB_PORT | Database port (e.g. 5432) |

## API Endpoints

Protected endpoints need the header `Authorization: Bearer <access_token>`.

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| POST | /api/auth/register/ | Register with name, email, password |
| POST | /api/auth/login/ | Log in and receive access and refresh tokens |
| POST | /api/auth/token/refresh/ | Get a new access token |

### Patients (authenticated, owner only)
| Method | Endpoint | Description |
|---|---|---|
| POST | /api/patients/ | Add a patient |
| GET | /api/patients/ | List your patients |
| GET | /api/patients/<id>/ | Get a patient |
| PUT | /api/patients/<id>/ | Update a patient |
| DELETE | /api/patients/<id>/ | Delete a patient |

### Doctors (authenticated)
| Method | Endpoint | Description |
|---|---|---|
| POST | /api/doctors/ | Add a doctor |
| GET | /api/doctors/ | List all doctors |
| GET | /api/doctors/<id>/ | Get a doctor |
| PUT | /api/doctors/<id>/ | Update a doctor (creator only) |
| DELETE | /api/doctors/<id>/ | Delete a doctor (creator only) |

### Patient-Doctor Mappings (authenticated)
| Method | Endpoint | Description |
|---|---|---|
| POST | /api/mappings/ | Assign a doctor to a patient |
| GET | /api/mappings/ | List your patients' mappings |
| GET | /api/mappings/<patient_id>/ | List doctors assigned to a patient |
| DELETE | /api/mappings/<id>/ | Remove a mapping (id is the mapping id) |

## Notes
- Login uses email and password (custom user model).
- A user can only view and modify their own patients.
- Doctors are visible to all authenticated users, but only the creator can edit or delete them.
- The same doctor cannot be assigned to the same patient twice.

## Postman
A Postman collection is included in the `postman/` folder.