const express = require("express");
const jwt = require("jsonwebtoken");

const app = express();
app.use(express.json());

const SECRET_KEY = "supersecretkey";

const USER = { username: "user1", password: "password123" };

let accountBalance = 1000;

app.post("/login", (req, res) => {
  const { username, password } = req.body;

  if (username === USER.username && password === USER.password) {
    const token = jwt.sign({ username }, SECRET_KEY, { expiresIn: "1h" });
    return res.json({ message: "Login successful", token });
  } else {
    return res.status(401).json({ error: "Invalid username or password" });
  }
});

function authenticateJWT(req, res, next) {
  const authHeader = req.headers["authorization"];

  if (!authHeader) {
    return res.status(401).json({ error: "Authorization header missing" });
  }

  const token = authHeader.split(" ")[1]; 

  if (!token) {
    return res.status(401).json({ error: "Token missing" });
  }

  jwt.verify(token, SECRET_KEY, (err, user) => {
    if (err) {
      return res.status(403).json({ error: "Invalid or expired token" });
    }
    req.user = user; 
    next();
  });
}

app.get("/balance", authenticateJWT, (req, res) => {
  res.json({ username: req.user.username, balance: accountBalance });
});

app.post("/deposit", authenticateJWT, (req, res) => {
  const { amount } = req.body;

  if (!amount || amount <= 0) {
    return res.status(400).json({ error: "Invalid deposit amount" });
  }

  accountBalance += amount;
  res.json({ message: Deposited ₹${amount} successfully., newBalance: accountBalance });
});

app.post("/withdraw", authenticateJWT, (req, res) => {
  const { amount } = req.body;

  if (!amount || amount <= 0) {
    return res.status(400).json({ error: "Invalid withdrawal amount" });
  }

  if (amount > accountBalance) {
    return res.status(400).json({ error: "Insufficient balance" });
  }

  accountBalance -= amount;
  res.json({ message: Withdrew ₹${amount} successfully., newBalance: accountBalance });
});

const PORT = 3000;
app.listen(PORT, () => console.log(Banking API running on http://localhost:${PORT}));
