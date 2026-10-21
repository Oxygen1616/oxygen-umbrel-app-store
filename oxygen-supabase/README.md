# Supabase Self-Hosting on umbrelOS

Supabase is the open-source Firebase alternative providing a dedicated PostgreSQL database, GoTrue authentication, instant RESTful & GraphQL APIs (PostgREST), Realtime subscriptions, and S3-compatible file storage.

---

## Network & Access Endpoints

| Service | Address | Notes |
| :--- | :--- | :--- |
| **Supabase Studio (Dashboard)** | `http://umbrel.local:53000` | Web UI for tables, SQL editor, auth & storage |
| **API Gateway (Kong)** | `http://umbrel.local:58000` | Unified gateway for REST, Auth, Realtime, & Storage |
| **PostgREST (REST API)** | `http://umbrel.local:58000/rest/v1` | Direct database query & RPC endpoints |
| **GoTrue (Auth API)** | `http://umbrel.local:58000/auth/v1` | User signup, login, recovery, and sessions |
| **Storage API** | `http://umbrel.local:58000/storage/v1` | Object upload, download, and public/signed URLs |
| **Internal Docker Gateway** | `http://oxygen-supabase_kong_1:8000` | Inter-container communication on Umbrel network |

---

## Default Credentials & Keys

* **Studio Login:** Username `umbrel` / Password `umbrel` (configurable in `exports.sh`)
* **Default Database:** `postgres`, User: `postgres`, Port: `5432`
* **Default Keys:** Located inside your Umbrel app directory under `apps/oxygen-supabase/exports.sh`:
  - `ANON_KEY`: Public client/browser token for authenticated or guest queries
  - `SERVICE_ROLE_KEY`: Admin secret key that bypasses Row Level Security (RLS) for backend services and AI agents.

---

## Connecting Apps & AI Agents to Supabase

### 1. Connecting Animator (`oxygen-animator`)

Animator can persist user videos, credits, and render status in your local Supabase:

1. Open Supabase Studio at `http://umbrel.local:53000`.
2. Go to the **SQL Editor** on the left menu.
3. Paste the contents of `oxygen-animator/supabase-schema.sql` and click **Run**.
4. In Animator settings or `docker-compose.yml`, set:
   ```env
   SUPABASE_URL=http://umbrel.local:58000
   SUPABASE_SECRET_KEY=<your-service-role-key-from-exports.sh>
   ```

### 2. Querying Supabase from AI Agents

Agents can query tables or trigger PostgreSQL stored procedures using standard HTTP calls:

```bash
# Query table with service_role key
curl -s "http://umbrel.local:58000/rest/v1/videos?select=*" \
  -H "apikey: <SERVICE_ROLE_KEY>" \
  -H "Authorization: Bearer <SERVICE_ROLE_KEY>"
```
