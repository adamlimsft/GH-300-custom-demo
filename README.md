# GH-300 training for Telstra

A small full-stack website for demonstrating GitHub Copilot. Telstra employees
can browse six training topics and try a registration flow.

## Run in GitHub Codespaces

1. Select **Code → Codespaces → Create codespace on main** (or select your demo branch).
2. Wait for the dev container to finish starting. It runs `npm start` automatically.
3. Open the preview for port **3000**, or select **Open in Browser** in the **Ports** tab.
   Keep the forwarded port private.

If the server is not running, run `npm start` in a terminal. If port 3000 is
already in use, use the existing server instead of starting a second instance.
No package installation, database, credentials, or external services are required.

## Run locally

Use Node.js 22 or newer:

```sh
npm start
```

Open http://localhost:3000. Use `npm run dev` to restart the backend automatically
when files change; refresh the browser to see frontend edits.
Set `PORT` to use another port (forward that port manually in Codespaces).

## Project layout

- `public/index.html`, `public/styles.css`, `public/app.js`: accessible, responsive frontend.
- `server.js`: Node.js HTTP backend, static file serving, and registration validation.
- `topics.js`: editable training catalogue (sample topics, not an official course syllabus).
- `test/server.test.js`: API and static-file tests using Node's built-in test runner.
- `.devcontainer/devcontainer.json`: Codespaces environment and private port forwarding.

Run tests with `npm test`. There is no build step or external dependency.

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