🛡️ JavaScript — Security, Tooling & Browser Concepts
📘 Overview

This document covers key web-development fundamentals:

Security practices (XSS, CSRF, Input Sanitization, CSP)

Development tooling (npm, yarn, ESLint, Prettier, Webpack)

Browser features (DOM, Events, Rendering, Storage, History API, Media API)

⚙️ 1. Security
🔹 Cross-Site Scripting (XSS)

What: Attackers inject malicious scripts into web pages viewed by other users.
Prevention:

Escape or sanitize user inputs before rendering.

Use templating libraries or sanitization packages (e.g. sanitize-html).

Example:

// ❌ Vulnerable
res.send(`<p>Hello ${req.query.name}</p>`);

// ✅ Safe
const sanitizeHtml = require("sanitize-html");
const safeName = sanitizeHtml(req.query.name);
res.send(`<p>Hello ${safeName}</p>`);

🔹 Cross-Site Request Forgery (CSRF)

What: Unauthorized commands are transmitted from a trusted user.
Prevention:

Use CSRF tokens.

Validate request origins.

Example (Express + csurf):

const csrf = require('csurf');
app.use(csrf());

app.get('/form', (req, res) => {
  res.send(`<form method="POST">
    <input type="hidden" name="_csrf" value="${req.csrfToken()}">
    <button type="submit">Submit</button>
  </form>`);
});

🔹 Input Sanitization

What: Cleans user inputs to prevent malicious code or markup injection.
Simple Example:

function sanitizeInput(input) {
  return input
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;");
}


With library:

npm install sanitize-html

const sanitizeHtml = require("sanitize-html");
const clean = sanitizeHtml(req.body.comment);

🔹 Content Security Policy (CSP)

What: Browser-level rule that restricts resource loading and script execution.
Example (HTML Meta Tag):

<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self';">


Example (Helmet in Express):

const helmet = require("helmet");
app.use(helmet.contentSecurityPolicy({
  directives: { defaultSrc: ["'self'"], scriptSrc: ["'self'"] }
}));

🧰 2. Tooling
🔹 npm & yarn

Package managers for installing dependencies.

npm install express
yarn add express

🔹 ESLint

Linter that identifies coding errors or style violations.

npm install eslint --save-dev
npx eslint --init
npx eslint src/

🔹 Prettier

Formats your code automatically for consistent style.

npm install --save-dev prettier
npx prettier --write src/

🔹 Webpack

A module bundler that compiles and optimizes JS, CSS, and images.

npm install --save-dev webpack webpack-cli
npx webpack --watch


Basic config example:

module.exports = {
  entry: "./src/index.js",
  output: { filename: "bundle.js" },
  mode: "development"
};

🌐 3. JavaScript in the Browser
🔹 DOM (Document Object Model)

Represents the structure of a webpage.

document.getElementById("demo").textContent = "Hello, JS!";

🔹 Events

Used to respond to user actions (clicks, typing, etc.).

document.querySelector("button").addEventListener("click", () => {
  alert("Button clicked!");
});

🔹 Rendering

Browser converts HTML, CSS, JS → pixels on screen.
JavaScript manipulates DOM, triggering re-rendering.

document.body.style.backgroundColor = "lightblue";

🔹 Storage APIs
🗂 Local Storage

Persists data even after reload.

localStorage.setItem("user", "Subasri");
console.log(localStorage.getItem("user"));

🕒 Session Storage

Data lasts only for current tab.

sessionStorage.setItem("token", "abc123");

🔹 History API

Modify browser history without reloading.

history.pushState({ page: 1 }, "Page 1", "page1.html");
window.addEventListener("popstate", e => console.log(e.state));

🔹 Media API

Control audio and video playback.

<video id="vid" src="sample.mp4" controls></video>
<script>
  const v = document.getElementById("vid");
  v.play();
</script>

✅ Summary
Category	Key Concept	Purpose
Security	XSS	Prevent malicious script injection
	CSRF	Stop unauthorized form submissions
	Input Sanitization	Clean user inputs
	CSP	Restrict resource/script loading
Tooling	npm / yarn	Dependency management
	ESLint / Prettier	Code quality & formatting
	Webpack	Bundling and optimization
Browser APIs	DOM	Access & modify HTML
	Events	Handle user interactions
	Storage	Save user data locally
	History	Manage navigation
	Media	Control video/audio
🧩 Example Mini Setup
npm init -y
npm install express eslint prettier webpack --save-dev
npm start

💬 Summary Note

Secure and maintainable web applications depend on:

Writing clean code (with tooling)

Implementing proper security (XSS, CSP, CSRF)

Understanding browser APIs for better interactivity and performance
