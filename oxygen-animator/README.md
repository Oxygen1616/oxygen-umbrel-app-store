# Animator on umbrelOS

**Type an idea, get a narrated hand-drawn whiteboard explainer video.**

Animator turns any prompt or topic into an animated explainer video:
1. **Storyboard:** Gemini structures the topic into sequential visual beats (one sentence + one icon each).
2. **Images:** Gemini illustrates each scene as a 16:9 whiteboard frame.
3. **Narration:** Gemini TTS speaks the narration, with pauses marking beat intervals.
4. **Regions:** Visual detection isolates each icon and associates it with its spoken sentence.
5. **Animation:** CPU stroke engine renders hand-drawn reveals synchronized to narration into an MP4 video.

---

## Access & Network Endpoints

| Context | URL | Description |
| :--- | :--- | :--- |
| **Browser / Web UI** | `http://umbrel.local:8002` | Built-in studio dashboard for manual video generation & playback |
| **LAN / Agent HTTP** | `http://umbrel.local:8002/api` | REST API accessible from other machines and LAN clients |
| **Docker Internal** | `http://oxygen-animator_server_1:8000/api` | Direct inter-container communication on the Umbrel Docker network |
| **Interactive Docs** | `http://umbrel.local:8002/api/docs` | OpenAPI / Swagger interactive API documentation |

---

## Connecting Animator to Local Supabase on umbrelOS

Animator supports two operational modes:
1. **Direct Agent Mode (Default):** Generates videos directly via `/api/generate` without requiring user accounts, external tokens, or database dependencies. MP4 files are stored in `${APP_DATA_DIR}/outputs`.
2. **Local Supabase Mode:** Connects to the local `Supabase` app on umbrelOS for user management, credit accounting, persistent PostgreSQL job records, and Supabase Storage bucket uploads.

### Step 1: Install Supabase on umbrelOS
Install **Supabase** (`oxygen-supabase`) from your Umbrel Community App Store.

Once running:
* **Studio UI:** `http://umbrel.local:53000`
* **Kong API Gateway:** `http://umbrel.local:58000`

### Step 2: Initialize the Animator Schema
1. Open Supabase Studio in your browser: `http://umbrel.local:53000`
2. Navigate to the **SQL Editor** tab.
3. Copy and paste the contents of [`supabase-schema.sql`](file:///c:/Users/Kasutaja/Desktop/umbrelos/oxygen-umbrel-app-store/oxygen-animator/supabase-schema.sql) into the editor.
4. Click **Run**. This automatically creates:
   - `public.profiles` table (with starting credit balances).
   - `public.videos` table and `video_status` enum.
   - `public.spend_credits` and `public.refund_credits` atomic database functions.
   - `videos` storage bucket in Supabase Storage with Row Level Security.

### Step 3: Get Your Supabase Secret Key
Open your Umbrel file manager or terminal and read the service role key from `apps/oxygen-supabase/exports.sh` (or from Supabase Studio -> **Project Settings** -> **API**):

```bash
# Look for SERVICE_ROLE_KEY
cat ~/umbrel/app-data/oxygen-supabase/exports.sh | grep SERVICE_ROLE_KEY
```

### Step 4: Configure Animator Environment
In Animator's Umbrel App settings or `docker-compose.yml`, supply:

```env
SUPABASE_URL=http://host.docker.internal:58000
SUPABASE_SECRET_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

> **Note:** If communicating from container to container, `host.docker.internal:58000` reaches Kong on the Umbrel host.

---

## Agent Access & API Guide

Any AI agent running on umbrelOS (e.g. **Hermes Agent**, **OpenClaw**, **Agent Zero**) or external scripts can generate whiteboard videos automatically via HTTP.

### 1. Trigger Video Generation

Send a `POST` request to `/api/generate`:

```http
POST http://umbrel.local:8002/api/generate
Content-Type: application/json

{
  "prompt": "How wings create lift when an airplane flies",
  "scenes": 4,
  "voice": "Kore",
  "quality": "medium",
  "gemini_api_key": ""
}
```

#### Parameters:
* `prompt` *(string, required)*: The topic or concept to explain.
* `scenes` *(integer, optional)*: Exact number of scenes (default: auto, usually 3–5).
* `voice` *(string, optional)*: Gemini TTS voice name. Options: `"Kore"` (default female), `"Puck"` (energetic male), `"Fenrir"` (deep male), `"Aoede"` (warm female), `"Zephyr"` (smooth male), `"Leda"` (calm female), `"Orus"`, `"Charon"`.
* `quality` *(string, optional)*: `"low"` (fast 20fps), `"medium"` (standard 24fps 1.5Mbps), `"high"` (24fps 3.0Mbps).
* `gemini_api_key` *(string, optional)*: Your Google Gemini API key. If omitted, Animator uses the `GEMINI_API_KEY` configured in the container environment.

#### Response (`202 Accepted`):
```json
{
  "job_id": "anim_4f89b1c2d3",
  "status": "processing",
  "prompt": "How wings create lift when an airplane flies",
  "video_url": "/api/videos/anim_4f89b1c2d3.mp4",
  "detail": "Video generation queued. Poll /api/status/{job_id} until status is 'ready'."
}
```

---

### 2. Poll Generation Status

Poll `GET /api/status/{job_id}`:

```bash
curl -s http://umbrel.local:8002/api/status/anim_4f89b1c2d3
```

#### When Processing:
```json
{
  "job_id": "anim_4f89b1c2d3",
  "status": "processing",
  "prompt": "How wings create lift when an airplane flies",
  "video_url": null,
  "detail": null
}
```

#### When Complete:
```json
{
  "job_id": "anim_4f89b1c2d3",
  "status": "ready",
  "prompt": "How wings create lift when an airplane flies",
  "video_url": "/api/videos/anim_4f89b1c2d3.mp4",
  "detail": null
}
```

---

### 3. Retrieve / Download Rendered Video

Stream or download the MP4 file:

```bash
curl -O http://umbrel.local:8002/api/videos/anim_4f89b1c2d3.mp4
```

---

## Code Examples for Agents

### cURL (Bash Script or Command-line Agent)

```bash
# 1. Start generation
JOB_JSON=$(curl -s -X POST http://umbrel.local:8002/api/generate \
  -H "Content-Type: application/json" \
  -d '{"prompt": "How photosynthesis works", "voice": "Kore"}')

JOB_ID=$(echo "$JOB_JSON" | grep -o '"job_id": *"[^"]*"' | cut -d'"' -f4)
echo "Started job: $JOB_ID"

# 2. Poll until complete
while true; do
  STATUS_JSON=$(curl -s "http://umbrel.local:8002/api/status/$JOB_ID")
  STATUS=$(echo "$STATUS_JSON" | grep -o '"status": *"[^"]*"' | cut -d'"' -f4)
  if [ "$STATUS" = "ready" ]; then
    VIDEO_PATH=$(echo "$STATUS_JSON" | grep -o '"video_url": *"[^"]*"' | cut -d'"' -f4)
    echo "Video ready at: http://umbrel.local:8002$VIDEO_PATH"
    curl -s -O "http://umbrel.local:8002$VIDEO_PATH"
    break
  elif [ "$STATUS" = "failed" ]; then
    echo "Generation failed!"
    exit 1
  fi
  sleep 4
done
```

---

### Python (Hermes Agent / Agent Zero / Custom Skills)

```python
import time
import requests

def create_whiteboard_video(prompt: str, voice: str = "Kore", quality: str = "medium") -> str:
    """Generate an explainer video and return the full download URL."""
    base_url = "http://umbrel.local:8002"
    
    # Trigger generation
    resp = requests.post(f"{base_url}/api/generate", json={
        "prompt": prompt,
        "voice": voice,
        "quality": quality
    }).json()
    
    job_id = resp["job_id"]
    print(f"Animation started (Job {job_id}) for prompt: '{prompt}'")
    
    # Poll for completion
    while True:
        status_data = requests.get(f"{base_url}/api/status/{job_id}").json()
        status = status_data["status"]
        
        if status == "ready":
            video_url = f"{base_url}{status_data['video_url']}"
            print(f"Animation completed: {video_url}")
            return video_url
        elif status == "failed":
            raise RuntimeError(f"Animation failed: {status_data.get('detail')}")
            
        time.sleep(3)

# Example usage:
if __name__ == "__main__":
    url = create_whiteboard_video("Why does the moon have phases?")
    print("Download URL:", url)
```

---

### Hermes Agent Skill Definition

To register Animator as a tool for Hermes Agent on umbrelOS, save the following in Hermes' tools directory (`~/.hermes/tools/animator_tool.py`):

```python
import time
import requests

def render_whiteboard_explainer(prompt: str, voice: str = "Kore") -> str:
    """
    Generate an AI hand-drawn whiteboard explainer video with synchronized narration.
    Use this when a user asks for a video explanation, visual breakdown, or whiteboard animation.
    
    :param prompt: The concept or explanation topic to illustrate.
    :param voice: Voice for narration (Kore, Puck, Fenrir, Aoede, Zephyr, Leda).
    :return: Full URL to download or watch the generated MP4 explainer video.
    """
    base_url = "http://umbrel.local:8002"
    
    res = requests.post(f"{base_url}/api/generate", json={"prompt": prompt, "voice": voice}).json()
    job_id = res.get("job_id")
    if not job_id:
        return f"Error triggering animation: {res}"
        
    for _ in range(60): # wait up to 3 minutes
        time.sleep(3)
        poll = requests.get(f"{base_url}/api/status/{job_id}").json()
        if poll.get("status") == "ready":
            return f"Whiteboard video successfully generated: {base_url}{poll.get('video_url')}"
        elif poll.get("status") == "failed":
            return f"Failed to generate video: {poll.get('detail')}"
            
    return "Generation timed out. Check status at http://umbrel.local:8002"
```

---

## Configuration & Environment Variables

Configure via Umbrel App Settings or environment file:

| Variable | Default | Purpose |
| :--- | :--- | :--- |
| `GEMINI_API_KEY` | *(None)* | Default Google AI Studio key for all requests (avoids having agents send key in payload) |
| `GEMINI_MODEL` | `gemini-2.5-flash` | Gemini text model used for storyboarding |
| `ANIMATOR_OUTPUT_DIR` | `/app/outputs` | Output path inside container for rendered MP4 files |
| `SUPABASE_URL` | `http://host.docker.internal:58000` | Local or remote Supabase API gateway URL |
| `SUPABASE_SECRET_KEY`| *(None)* | Supabase `service_role` key (bypasses RLS to manage credits & storage) |
| `API_ALLOWED_ORIGINS`| `*` | Allowed CORS origins |

All rendered videos are persisted in the Umbrel app data folder (`${APP_DATA_DIR}/outputs`).
