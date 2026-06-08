# Transcendence

A real-time multiplayer Pong platform built at [42 Lausanne](https://42lausanne.ch).

Transcendence is a full-stack web application featuring multiplayer Pong, matchmaking, tournaments, live chat, user accounts management, and an experimental AI opponent.

**Play here:**
[https://transcendence-frontend-923872734712.europe-west6.run.app/register](https://transcendence-frontend-923872734712.europe-west6.run.app/register)

---

## Installation

Clone the repository:

```bash
git clone https://github.com/whatevacreates/transcendence.git
cd transcendence
```

Add SSL certificates inside the `nginx/ssl` directory.

Create the directory:

```bash
cd nginx
mkdir ssl
cd ssl
```

Generate local development certificates:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout privkey.pem \
  -out cert.pem \
  -subj "/CN=localhost"
```

The expected files are:

```text
nginx/ssl/privkey.pem
nginx/ssl/cert.pem
```

Run the project with Docker Compose:

```bash
docker compose up --build
```

---

## Usage

Once the application is running, open the frontend in your browser.

Depending on your local configuration, the app should be available through the Nginx entry point, usually at:

```text
https://localhost
```

---

## Dev features

### Main user-facing features :

* Registering and logging in
* Choosing a nickname
* Playing real-time Pong matches
* Playing against the AI opponent
* Joining matchmaking
* Participating in tournaments
* Using live chat
* Managing friendship
* Viewing stored match results
  
### Server-side Pong engine

The Pong game logic is handled server-side to keep the backend authoritative.

The server manages:

* Ball movement
* Paddle movement
* Collision detection
* Score calculation
* Game state updates
* Match lifecycle
* Synchronization with frontend clients through WebSockets

### WebSockets

Real-time communication is implemented with WebSockets using Socket.IO.

WebSockets are used for:

* Sending player inputs to the backend
* Broadcasting game state updates
* Managing live matches
* Handling chat messages
* Supporting matchmaking and tournament interactions

### Architecture

The project uses a hybrid architecture combining:

* **Domain-Driven Design**, to separate business logic from meaningful domains
* **Event-Driven Design**, to manage real-time interactions and asynchronous game events

This structure was especially useful for separating concerns between authentication, matchmaking, tournaments, chat, game state, and AI behavior.

### Deployment

The project is fully Dockerized and deployed on **Google Cloud Run**.

The infrastructure includes:

* Dockerized frontend
* Dockerized backend
* Nginx reverse proxy
* SSL configuration
* Cloud deployment through Google Cloud Run


### AI opponent

The AI opponent was trained using **NEAT**, NeuroEvolution of Augmenting Topologies.

The main experimental constraint was that the AI could observe the game state only once per second. This made the training harder, because the model had to learn useful behavior from sparse input rather than continuous frame-by-frame information.

Several optimization methods were used during training:

* Data pruning
* Early stopping
* Preventing unnecessary network bloat
* Training and comparing hundreds of models
* Exporting the final model from Pickle to JSON

The final JSON model is embedded directly into the backend and can play Pong consistently.

Additional experiments were also made with:

* RNN models
* Deep Q-Learning

---

## Technical notes

### Tech stack

| Layer          | Technology                                                 |
| -------------- | ---------------------------------------------------------- |
| Frontend       | Custom framework, TypeScript, TailwindCSS                  |
| Backend        | Node.js, Fastify, Socket.IO                                |
| AI             | Python, NEAT, RNN experiments, Deep Q-Learning experiments |
| Database       | SQLite                                                     |
| Infrastructure | Docker, Docker Compose, Nginx, Google Cloud Run            |
| Architecture   | Domain-Driven + Event-Driven hybrid architecture           |


---

## Demo

Live demo:

[https://transcendence-frontend-923872734712.europe-west6.run.app/register](https://transcendence-frontend-923872734712.europe-west6.run.app/register)


