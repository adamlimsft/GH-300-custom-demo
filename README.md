# GH-300 training for Telstra

A small full-stack website for demonstrating GitHub Copilot. Telstra employees
can browse six training topics and try a registration flow.
The default backend is a small **Python FastAPI app**; the original Node.js
backend remains available as an alternative.

## Run in GitHub Codespaces

1. Select **Code → Codespaces → Create codespace on main** (or select your demo branch).
2. Wait for the dev container to install Python dependencies in `.venv`.
   It runs `.venv/bin/python app.py` automatically.
3. The website opens in a browser tab for port **3000**, or select **Open in Browser**
   in the **Ports** tab. Keep the forwarded port private. Use a browser tab rather
   than an embedded preview because the website blocks iframe embedding.

If the server is not running, run `.venv/bin/python app.py` in a terminal.
For an existing Codespace, select **Codespaces: Rebuild Container** to apply
the Python setup. If port 3000 is already in use, use the existing server
instead of starting a second instance. No database or credentials are required.

## Run locally

### Python (recommended for demos)

Use Python 3.12 or newer. From the repository directory:

```sh
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python app.py
```

On Windows, activate the environment with `.venv\Scripts\Activate.ps1` instead.
Open http://localhost:3000. Python edits reload the server automatically;
refresh the browser after frontend edits. Reloading resets demo registrations.

### Node.js alternative

Use Node.js 22 or newer (no package installation required):

```sh
npm start
```

Open http://localhost:3000. Use `npm run dev` for automatic backend restarts.
Run only one backend at a time; both serve the same website and API.
Set `PORT` to use another port with either backend (forward it manually in Codespaces).

## Tweak the Python demo

- Edit `app.py` to change the topic and registration endpoints or add new routes.
  It contains the FastAPI `app`, route decorators, validation, and an in-memory
  `registrations` dictionary, similar to a simple activity-signup example.
- Edit `topics.json` to change the training catalogue shared by both backends.
  Restart the server after changing this file.
- Edit the files in `public/` to change the website.
- Visit http://localhost:3000/docs for FastAPI's interactive API documentation,
  or `/openapi.json` for the API schema. The docs UI requires internet access
  to load its assets; the training website itself uses local assets only.

## Project layout

- `public/index.html`, `public/styles.css`, `public/app.js`: accessible, responsive frontend.
- `app.py`: Python FastAPI backend and demo entry point.
- `requirements.txt`, `requirements-dev.txt`: runtime and test dependencies.
- `server.js`: alternative Node.js HTTP backend.
- `topics.json`: shared editable training catalogue (sample topics, not an official course syllabus).
- `topics.js`: loads the shared catalogue for Node.js.
- `test/test_app.py`: Python API and static-file tests using `unittest` and FastAPI's test client.
- `test/server.test.js`: API and static-file tests using Node's built-in test runner.
- `.devcontainer/devcontainer.json`: Codespaces environment and private port forwarding.

With your Python virtual environment activated, install test dependencies and run:

```sh
python -m pip install -r requirements-dev.txt
python -m unittest discover -s test -p "test_*.py" -v
```

Run the original Node.js tests with `npm test`. There is no frontend build step.

## API

- `GET /api/topics`: returns the training topic catalogue.
- `POST /api/registrations`: accepts JSON with `name`, `email`, and `topicId`;
  returns `201` and a registration confirmation. Invalid input returns `400`,
  duplicate email/topic pairs return `409`, oversized requests return `413`,
  and non-JSON requests return `415`.

**Demo only:** use fictional details. Registrations live only in server memory
and disappear when the process restarts. No email is sent and no real place is
reserved. This demo has no authentication or employee verification; do not use it
to collect real employee data or expose it as a production registration service.
There is intentionally no public endpoint that lists registrations.

## Try these Copilot prompts

- “Add a topic search filter and an empty-results message.”
- “Add a difficulty level to topics and display it on each card.”
- “Write tests for registration validation edge cases.”
- “Add session dates and let participants choose a timeslot.”
- “Plan authenticated registration and persistent storage for production use.”