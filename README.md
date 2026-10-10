# AptiQuiz Live — Multiplayer Aptitude Arena

A full-stack hackathon MVP for the third problem statement (AptiQuiz): real-time multiplayer aptitude quiz rooms, a live player lobby, timed MCQs, scoring, leaderboard, answer explanations and basic session analytics.

## Tech stack
- React + Vite frontend
- Express REST API
- Socket.IO for live room/player/question/leaderboard events
- In-memory server state for quick hackathon setup (no database service needed)
- Lucide icons and responsive custom UI

## Requirements
Node.js 20+ and npm.

## Run locally
1. Extract this ZIP and open the `APTIQUIZ` directory in VS Code.
2. In the integrated terminal, run `npm install`.
3. Optional: copy `.env.example` to `.env` and adjust `PORT` / `CLIENT_ORIGIN` if needed.
4. Run `npm run dev` to start Express and Vite together.
5. Open `http://localhost:5173`. API health check: `http://localhost:5000/api/health`.

## Demo flow
1. Click **Host a quiz**, create a room and note the six-character room code.
2. Open another browser/incognito window (or another device on the same deployed site) and choose **Join with code**.
3. Enter the room code and a unique display name.
4. Back in the host window, launch the quiz.
5. Submit answers in the player window; the timer, scoring and leaderboard update over Socket.IO.
6. After a question ends, the host advances to the next question. The final leaderboard appears when the game ends.
7. Visit **Live analytics** to view current in-memory room and answer stats.

## Connected API routes
- `GET /api/health` — service health and active room count
- `GET /api/questions` — public question catalogue without answer keys
- `GET /api/analytics` — basic aggregate analytics from current in-memory rooms
- `POST /api/rooms` — create room and receive room code

## Socket.IO events
Client → server: `room:join`, `room:host`, `game:start`, `answer:submit`, `game:next`, `game:end`.
Server → client: `room:update`, `activity`, `question:started`, `question:result`, `question:ended`, `leaderboard:update`, `game:finished`, `host:disconnected`.

## Important MVP boundaries
- This is a hackathon-ready starter, not a hardened production platform. Rooms and analytics are held in memory and reset when the server restarts.
- Adaptive mode is a UI/config flag and difficulty labels exist in the question bank; the current question order is randomized rather than a sophisticated adaptive algorithm.
- Sample questions are editable starter content. Review wording, answer keys and difficulty before public use.
- No authentication, persistent database, anti-cheat, teacher account system, or durable campus league history is included. For production, add these, plus rate limiting, event validation, logging, load testing and proper deployment configuration.
- Up to 50 is the intended room cap. Verify under expected network conditions with a load test before claiming performance guarantees.
- Do not commit `.env` secrets.

## Suggested judge demo
Show the same room on two windows: a host dashboard and a player. Demonstrate join → live start → timed question → answer submission → leaderboard change → explanation/review → next question → final ranking. Explain that scoring rewards correctness and response speed, with a small streak bonus.

## Gameplay fixes in this build
- The host is added as a scored participant and can answer each question.
- Questions advance automatically after every connected participant has answered, with a short 2.5-second review pause.
- If not everyone answers, the question timer ends the round and the same automatic progression continues.
- The final question automatically leads to the results leaderboard.
- Socket.IO Vite proxy enables WebSocket forwarding; CORS allows the default Vite ports 5173–5175.

Rooms are held in server memory; restarting the server clears active rooms and scores.


## Saved quiz history
Completed quiz leaderboards are written to `server/data/history.json` and can be opened from **Saved results** in the sidebar. Keep this file when backing up the project.

## Question count and text size
The setup accepts a numeric question count from 5 to 15 (the current sample bank has 15 questions). The stylesheet readability pass raises small text sizes to at least 15px.
