
# Example Internal Auction Site

Lightweight auction platform used for demos and local development. This repository contains a full-featured FastAPI backend with websocket support and a Vite React frontend.

**Contents**
- `backend/` — Full FastAPI backend service and Dockerfile. Main app: `backend/server/app/main.py`.
- `frontend/` — React + TypeScript (Vite) single-page app.
- `main.py` — Small demo FastAPI app (minimal example useful for quick testing).
- `docker-compose.yml` — Local compose setup for backend + frontend.

**Table of Contents**
- **Overview**
- **Architecture & Key Components**
- **Quick Start (Docker Compose)**
- **Run Locally (backend & frontend)**
- **Environment Variables**
- **API Summary**
- **Websocket Events**
- **Database & Collections**
- **Testing**
- **Development Notes**
- **Troubleshooting**
- **Contributing**
- **License**

**Overview**

This project demonstrates an internal auction platform with features commonly found in auction systems: user authentication (OTP), item management, bidding, live auction state via websockets, background tasks for auto-closing auctions, and a wishlist feature. The frontend consumes the backend APIs and falls back to mock data when the backend is not reachable.

**Architecture & Key Components**

- Backend: `FastAPI` with modular routers under `backend/server/app/` for `auth`, `items`, `bids`, `auction`, `users`, and `wishlist`.
- Database: MongoDB (async access via `motor`).
- Websockets: Real-time auction events are broadcast via a websocket manager and outbox pattern for reliability.
- Frontend: Vite + React + TypeScript under `frontend/`.

**Quick Start (Docker Compose)**

Start both services (backend + frontend) using Docker Compose:

```bash
docker-compose up --build
```

Default ports:
- Backend: `http://localhost:8000`
- Frontend: `http://localhost:8080`

**Run Locally (Backend)**

Prerequisites: Python 3.11, a running MongoDB instance, and optional virtualenv.

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
# Copy or create backend/.env with MONGODB_URI set
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

If you prefer the lightweight demo API, run the root `main.py` instead:

```bash
python main.py
# or
uvicorn main:app --reload
```

**Run Locally (Frontend)**

```bash
cd frontend
npm install
# optionally set VITE_API_BASE_URL in frontend/.env
npm run dev
```

When running the frontend dev server, set `VITE_API_BASE_URL` to the backend URL if it doesn't autodetect.

**Environment Variables**

Common variables (set in `backend/.env` or environment):

- `MONGODB_URI` — MongoDB connection string (e.g. `mongodb://localhost:27017`).
- `MONGODB_DB_NAME` — Database name (default: `auction_system`).
- `CORS_ALLOW_ORIGINS` — Comma-separated origins for CORS.
- `WS_OUTBOX_COLLECTION_NAME` — Collection name for websocket outbox (default: `ws_outbox`).
- `AUCTION_*_POLL_SECONDS` — Poll intervals for background tasks (e.g. auto-end/inactivity).

Frontend:
- `VITE_API_BASE_URL` — Base URL for API requests in dev.

**API Summary (high level)**

The backend exposes REST endpoints organized per feature. Below are the main routes and their purpose (refer to router files for full details):

- Authentication (`/auth`): endpoints for OTP generation/verification, login flows. See `backend/server/app/auth/`.
- Items (`/items`): create, read, update auction items. See `backend/server/app/items/`.
- Auctions (`/auctions`): create and manage auctions, fetch public auction state. See `backend/server/app/auction/`.
- Bids (`/bids`): place bids and query bid history. See `backend/server/app/bids/`.
- Users (`/users`): user CRUD and profile endpoints. See `backend/server/app/users/`.
- Wishlist (`/wishlist`): manage user wishlist items. See `backend/server/app/wishlist/`.

For quick testing there is a small demo API in `main.py` (root) that provides simple `/items` and `/auctions` endpoints.

**Websocket Events**

The backend exposes websocket endpoints to subscribe to auction events (e.g., state updates, new bids, auction ended). The websocket manager broadcasts events and an outbox dispatcher ensures reliable delivery. See `backend/server/app/auction/auction_ws.py` and `ws_outbox.py`.

Typical event types:
- `AUCTION_STATE_UPDATED` — current auction state update
- `AUCTION_ENDED` — auction finished and results available
- `NEW_BID` — a new bid was placed

Clients should connect to the websocket route under the auction module and handle JSON-encoded events. The frontend includes a `auction_ws` client implementation.

**Database & Collections**

Primary collections used by the backend:

- `users` — user records
- `items` — auction item details
- `auctions` — auction records and metadata
- `bids` — bid history
- `auction_messages`, `auction_chat_messages` — chat/message logs
- `ws_outbox` — outbox for reliable websocket events

Data model details are defined near the routers and `models/` packages in the backend. Example: auction documents hold `status`, `starts_at` / `ends_at`, `itemIds`, and `currentHighestBid`.

**Testing**

Backend tests are present under `backend/server/app/tests/` and use `pytest` and `httpx`.

Run tests:

```bash
cd backend/server
pytest -q
```

**Development Notes**

- Use `uvicorn --reload` for hot-reload development of the backend.
- Frontend uses Vite's HMR during `npm run dev`.
- Logs: backend uses standard logging; set `LOG_LEVEL` or configure logging in `backend/server/app/main.py` for more verbosity.
- Background tasks: the backend launches tasks for auction auto-end and inactivity checks when `MONGODB_URI` is configured.

**Troubleshooting**

- Mongo errors: confirm `MONGODB_URI` and network accessibility.
- CORS: set `CORS_ALLOW_ORIGINS` to include your frontend origin.
- Websocket connect issues: ensure backend is reachable and `WS_OUTBOX_*` settings are correct.

**Contributing**

Contributions are welcome. Suggested next docs to add:

- `CONTRIBUTING.md` — contribution guidelines and code style
- `API.md` — full API reference with request/response examples
- `ARCHITECTURE.md` — deeper architecture and design decisions

To propose changes, open an issue or submit a PR with a clear description and tests where applicable.

