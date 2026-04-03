The project is in text format.
index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Portfolio</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<nav>
  <h2>My Portfolio</h2>
  <ul>
    <li><a href="#home">Home</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#skills">Skills</a></li>
    <li><a href="#projects">Projects</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
  <button id="themeToggle">🌙</button>
</nav>

<section id="home"><h1>Home</h1></section>
<section id="about"><h1>About</h1></section>
<section id="skills"><h1>Skills</h1></section>
<section id="projects"><h1>Projects</h1></section>

<section id="contact">
  <h1>Contact</h1>
  <form id="contactForm">
    <input type="text" id="name" placeholder="Name" required>
    <input type="email" id="email" placeholder="Email" required>
    <small id="error"></small>
    <textarea id="message" placeholder="Message" required></textarea>
    <button type="submit">Submit</button>
  </form>
</section>

<section id="admin">
  <h2>Admin Login</h2>
  <div id="loginBox">
    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">
    <button onclick="login()">Login</button>
  </div>

  <div id="responses" class="hidden">
    <h2>Messages</h2>
    <div id="data"></div>
  </div>
</section>

<script src="script.js"></script>
</body>
</html>

style.css
body {
  font-family: Arial;
  margin: 0;
}

nav {
  display: flex;
  justify-content: space-between;
  background: #333;
  color: white;
  padding: 10px;
  position: sticky;
  top: 0;
}

nav ul {
  display: flex;
  list-style: none;
  gap: 15px;
}

nav a {
  color: white;
  text-decoration: none;
}

section {
  padding: 50px;
  border-bottom: 1px solid #ddd;
}

form {
  display: flex;
  flex-direction: column;
  width: 300px;
}

input, textarea {
  margin: 10px 0;
  padding: 10px;
}

button {
  padding: 10px;
  cursor: pointer;
}

.hidden {
  display: none;
}

.card {
  border: 1px solid #ccc;
  margin: 10px;
  padding: 10px;
}

.dark {
  background: #111;
  color: white;
}

.dark nav {
  background: black;
}
script.js
document.getElementById("contactForm").addEventListener("submit", function(e) {
  e.preventDefault();

  let name = document.getElementById("name").value;
  let email = document.getElementById("email").value;
  let message = document.getElementById("message").value;
  let error = document.getElementById("error");

  let regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

  if (!regex.test(email)) {
    error.textContent = "Invalid Email";
    return;
  }

  error.textContent = "";

  let data = JSON.parse(localStorage.getItem("messages")) || [];

  data.push({
    name,
    email,
    message,
    time: new Date().toLocaleString()
  });

  localStorage.setItem("messages", JSON.stringify(data));

  alert("Submitted");
  this.reset();
});

function login() {
  let user = document.getElementById("username").value;
  let pass = document.getElementById("password").value;

  if (user === "admin" && pass === "1234") {
    document.getElementById("loginBox").style.display = "none";
    document.getElementById("responses").classList.remove("hidden");
    showData();
  } else {
    alert("Wrong credentials");
  }
}

function showData() {
  let data = JSON.parse(localStorage.getItem("messages")) || [];
  let container = document.getElementById("data");

  container.innerHTML = "";

  data.forEach(item => {
    let div = document.createElement("div");
    div.className = "card";

    div.innerHTML = `
      <p><b>Name:</b> ${item.name}</p>
      <p><b>Email:</b> ${item.email}</p>
      <p><b>Message:</b> ${item.message}</p>
      <p><b>Time:</b> ${item.time}</p>
    `;

    container.appendChild(div);
  });
}

let btn = document.getElementById("themeToggle");

btn.onclick = () => {
  document.body.classList.toggle("dark");
  localStorage.setItem("theme", document.body.classList.contains("dark"));
};

if (localStorage.getItem("theme") === "true") {
  document.body.classList.add("dark");
}

