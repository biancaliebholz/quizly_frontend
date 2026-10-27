# Quizly Frontend

![Quizly Logo](assets/icons/logoheader.png)

## Project Attribution & License

> „Entwickelt im Rahmen des Weiterbildungsprogramms der Developer Akademie GmbH.“

- The **frontend source code and UI assets** (layouts, styles, images, icons, fonts) were provided by **Developer Akademie GmbH**.
- I (Bianca Liebholz) developed the **corresponding Django/DRF backend**.
- The components provided by Developer Akademie are covered by the **Developer Akademie Lernlizenz (Nicht-kommerziell)**. The full terms are in [`LICENSE.md`](LICENSE.md).
- This is a **non-commercial portfolio/application demo**. The public demo is for **test data only** and is **not a productive service**.

## Project Overview

Quizly is an interactive quiz application that allows users to create, take, and manage quizzes. The application features a modern UI with a dark theme and green accents, providing an engaging user experience.

## Features

- **User Authentication**: Register, login, and logout functionality
- **Quiz Generation**: Create quizzes from Youtube-URLs
- **Quiz Taking**: Interactive quiz interface with multiple-choice questions
- **Results Review**: View quiz results with correct/incorrect answers
- **Quiz Management**: View, edit, and delete quizzes

## Requirements

- The backend must support **JWT authentication** with **HttpOnly cookies**. This means:
    - The login response must set the JWT access token as an HttpOnly cookie.
    - Requests to protected routes must be authenticated via this cookie.
    - The frontend must not have direct access to the token (e.g., no localStorage or Authorization header).
    - If the frontend and the API run on different origins (e.g. local development), the backend must allow cross-origin requests (CORS) with credentials for the frontend origin.
---

## Project Structure

```
quizly_frontend/
├── assets/               # Static assets
│   ├── fonts/            # Font files (self-hosted woff2)
│   ├── icons/            # Icon images
│   └── img/              # Other images (incl. local video placeholder)
├── pages/                # HTML pages
│   ├── legalnotice.html  # Legal notice page
│   ├── library.html      # Quiz library page
│   ├── login.html        # Login page
│   ├── privacy.html      # Privacy policy page
│   ├── quiz.html         # Quiz taking page
│   ├── quizoverview.html # Quiz overview page
│   └── registration.html # Registration page
├── shared/               # Shared resources
│   ├── css/              # CSS stylesheets
│   │   ├── auth.css      # Authentication styles
│   │   ├── fonts.css     # Font definitions
│   │   ├── library.css   # Library page styles
│   │   ├── policy.css    # Policy pages styles
│   │   ├── quiz.css      # Quiz page styles
│   │   ├── quizoverview.css # Quiz overview styles
│   │   ├── standart.css  # Base styles
│   │   ├── toastmsg.css  # Toast message styles
│   │   └── variables.css # CSS variables
│   └── js/               # JavaScript files
│       ├── api.js        # API interaction functions
│       ├── auth.js       # Authentication functions
│       ├── config.js     # Configuration constants
│       ├── header.js     # Header component
│       ├── helper.js     # Helper functions
│       ├── library.js    # Library page functions
│       ├── quiz.js       # Quiz functionality
│       ├── quizoverview.js # Quiz overview functions
│       └── template.js   # HTML templates
├── index.html            # Main entry point
└── styles.css            # Global styles
```

## Setup and Installation

The API base URL is set in `shared/js/config.js`:

```js
const API_BASE_URL = "/api/";
```

The frontend calls the API on the **same origin** under `/api/`. It uses root-relative paths (e.g. `/shared/...`, `/assets/...`), so it has to be served from the web root.

### Production (same origin)

1. Serve this folder as static files from the web root (e.g. with Nginx).
2. Proxy `/api/` on the same domain to the Django/DRF backend.
3. Serve the site over HTTPS. The backend should set its auth cookies with `Secure`.

Because the frontend and the API share one origin, the browser doesn't make cross-origin requests, and the HttpOnly cookies are first-party.

### Local development

1. Clone the repository and start the backend (by default at `http://127.0.0.1:8000`).
2. Choose one of the following:
    - **Recommended:** serve the frontend and the API through one local origin, using a small reverse proxy that serves this folder and forwards `/api/` to the backend.
    - **Alternative:** serve the frontend with a static server (e.g. Live Server on `http://127.0.0.1:5500`) and temporarily set `API_BASE_URL` to `"http://127.0.0.1:8000/api/"`. Add the frontend origin to the backend's `FRONTEND_ORIGINS`. **Don't commit this change.**
3. Open `index.html` through the web server.

## Privacy

- **No third-party requests on page load:** fonts, images and scripts are all served locally. The app uses no CDNs and no external fonts.
- **YouTube (two-click):** the quiz overview first shows only a local placeholder with a privacy notice. The video is loaded from `youtube-nocookie.com` only after the user clicks **"Load video"**.
- **Legal pages:** `pages/legalnotice.html` and `pages/privacy.html` link to the central pages at [biancaliebholz.de/imprint](https://biancaliebholz.de/imprint) and [biancaliebholz.de/privacy](https://biancaliebholz.de/privacy).
- **No secrets in the frontend:** the frontend never sees API keys or tokens. Authentication runs entirely through HttpOnly cookies set by the backend.
