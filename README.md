# Portfolio-website-
const express = require("express");
const app = express();

app.use(express.json());

// store messages in memory
let messages = [];

// MAIN WEBSITE (HTML + CSS + JS together)
app.get("/", (req, res) => {
  res.send(`
<!DOCTYPE html>
<html>
<head>
  <title>Stephanie Portfolio</title>

  <style>
    body {
      margin: 0;
      font-family: Arial;
      background: black;
      color: white;
      text-align: center;
    }

    header {
      padding: 20px;
      background: #111;
    }

    h1 {
      color: hotpink;
    }

    section {
      padding: 20px;
    }

    .images img {
      width: 200px;
      margin: 10px;
      border-radius: 10px;
    }

    input {
      padding: 10px;
      margin: 5px;
      width: 220px;
    }

    button {
      padding: 10px 20px;
      background: hotpink;
      border: none;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background: deeppink;
    }

    #status {
      margin-top: 10px;
      color: lightgreen;
    }
  </style>
</head>

<body>

  <header>
    <h1>Stephanie Portfolio</h1>
    <p>Fashion Designer | Creative Stylist</p>
  </header>

  <section>
    <h2>About Me</h2>
    <p>
      Welcome to Stephanie Fashion Trend. I create modern, elegant fashion designs.
    </p>
  </section>

  <section>
    <h2>My Designs</h2>
    <div class="images">
      <img src="https://images.unsplash.com/photo-1521334884684-d80222895322">
      <img src="https://images.unsplash.com/photo-1520975916090-3105956dac38">
      <img src="https://images.unsplash.com/photo-1483985988355-763728e1935b">
    </div>
  </section>

  <section>
    <h2>Contact / Order</h2>

    <input id="name" type="text" placeholder="Your Name"><br>
    <input id="message" type="text" placeholder="Your Order / Message"><br>

    <button onclick="sendMessage()">Send</button>

    <p id="status"></p>
  </section>

  <script>
    async function sendMessage() {
      const name = document.getElementById("name").value;
      const message = document.getElementById("message").value;

      const res = await fetch("/message", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({ name, message })
      });

      const data = await res.json();

      document.getElementById("status").innerText = data.reply;
    }
  </script>

</body>
</html>
  `);
});

// BACKEND API (receives messages)
app.post("/message", (req, res) => {
  const { name, message } = req.body;

  messages.push({
    name,
    message,
    time: new Date()
  });

  console.log("New message:", name, message);

  res.json({ reply: "Message received successfully ✔️" });
});

// view messages (admin)
app.get("/messages", (req, res) => {
  res.json(messages);
});

// start server
app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
