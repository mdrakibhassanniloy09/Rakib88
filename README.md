# Rakib88 <!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Aviator Crash</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #0f2027, #203a43, #2c5364);
      color: white;
      text-align: center;
    }header {
  padding: 20px;
  background-color: #111;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 28px;
  color: #ff3c3c;
  font-weight: bold;
  text-shadow: 2px 2px 5px black;
}

#langSwitcher {
  padding: 5px;
  font-size: 16px;
}

#authPopup {
  background: rgba(0,0,0,0.8);
  padding: 20px;
  width: 300px;
  margin: 50px auto;
  border-radius: 10px;
  box-shadow: 0 0 15px red;
}

input {
  padding: 10px;
  margin: 10px 0;
  width: 90%;
  border: none;
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  background-color: #ff3c3c;
  color: white;
  border: none;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: 0.3s;
}

button:hover {
  background-color: #ff6a6a;
}

#gameArea {
  margin-top: 30px;
}

.plane-area {
  position: relative;
  height: 200px;
  background-color: #1c1c1c;
  margin: 20px;
  border-radius: 10px;
}

#plane {
  height: 60px;
  position: absolute;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  animation: fly 7s linear forwards;
}

@keyframes fly {
  0% { left: 0; }
  100% { left: 90%; }
}

#multiplier {
  margin-top: 20px;
  font-size: 32px;
  color: lime;
  font-weight: bold;
}

.controls {
  margin: 20px;
}

#balance {
  position: absolute;
  top: 10px;
  right: 20px;
  font-size: 18px;
  font-weight: bold;
}

footer {
  margin-top: 40px;
  padding: 20px;
  background-color: #111;
}

footer a {
  color: #aaa;
  margin: 0 10px;
  text-decoration: none;
}

footer a:hover {
  color: white;
}

  </style>
</head>
<body>
  <!-- Logo and Language Switcher -->
  <header>
    <h1 class="logo">Aviator Crash</h1>
    <select id="langSwitcher">
      <option value="en">English</option>
      <option value="bn">বাংলা</option>
    </select>
  </header>  <!-- Login/Register Popup -->  <div id="authPopup">
    <h2 id="authTitle">Login / Register</h2>
    <input type="text" id="phoneEmail" placeholder="Phone or Email" />
    <button onclick="sendOTP()">Send OTP</button>
    <div id="otpSection" style="display:none">
      <input type="text" id="otpInput" placeholder="Enter OTP" />
      <button onclick="verifyOTP()">Verify</button>
    </div>
  </div>  <!-- Game Area -->  <main id="gameArea" style="display:none">
    <div id="balance">Balance: ৳<span id="balanceAmount">1000</span></div>
    <div class="plane-area">
      <img id="plane" src="https://i.ibb.co/4KcFjkW/plane.gif" alt="Plane" />
      <div id="multiplier">x1.00</div>
    </div>
    <div class="controls">
      <input type="number" id="betAmount" placeholder="Enter Bet Amount" />
      <button onclick="placeBet()">Place Bet</button>
      <button onclick="cashOut()">Cash Out</button>
    </div>
    <div id="resultMessage"></div>
  </main>  <!-- Footer Links -->  <footer>
    <a href="#" onclick="alert('We respect your privacy. Your data is safe.')">Privacy Policy</a>
    <a href="#" onclick="alert('You must be 18+. No cheating or automation allowed.')">Terms & Conditions</a>
  </footer><audio id="startSound" src="https://www.soundjay.com/buttons/sounds/button-3.mp3"></audio> <audio id="crashSound" src="https://www.soundjay.com/button/beep-07.mp3"></audio>

  <script>
    let balance = 1000;
    let multiplier = 1.00;
    let multiplierInterval;
    let bet = 0;
    let gameStarted = false;

    function sendOTP() {
      const val = document.getElementById('phoneEmail').value;
      if (val.length < 4) return alert("Enter phone/email");
      document.getElementById('otpSection').style.display = 'block';
    }

    function verifyOTP() {
      const otp = document.getElementById('otpInput').value;
      if (otp !== "1234") return alert("Wrong OTP");
      document.getElementById('authPopup').style.display = 'none';
      document.getElementById('gameArea').style.display = 'block';
    }

    function placeBet() {
      if (gameStarted) return;
      bet = parseInt(document.getElementById('betAmount').value);
      if (isNaN(bet) || bet <= 0 || bet > balance) return alert("Invalid bet amount");

      balance -= bet;
      updateBalance();
      document.getElementById('resultMessage').innerText = "";
      document.getElementById('startSound').play();
      document.getElementById('multiplier').style.color = 'lime';
      multiplier = 1.00;
      gameStarted = true;

      multiplierInterval = setInterval(() => {
        multiplier += 0.05;
        document.getElementById('multiplier').innerText = 'x' + multiplier.toFixed(2);
      }, 100);

      setTimeout(() => crash(), Math.random() * 5000 + 4000);
    }

    function crash() {
      if (!gameStarted) return;
      clearInterval(multiplierInterval);
      document.getElementById('multiplier').style.color = 'red';
      document.getElementById('crashSound').play();
      document.getElementById('resultMessage').innerText = `Crashed at x${multiplier.toFixed(2)}!`;
      gameStarted = false;
    }

    function cashOut() {
      if (!gameStarted) return;
      clearInterval(multiplierInterval);
      let win = bet * multiplier;
      balance += win;
      updateBalance();
      document.getElementById('resultMessage').innerText = `You won ৳${win.toFixed(2)}!`;
      document.getElementById('multiplier').style.color = '#00f0ff';
      gameStarted = false;
    }

    function updateBalance() {
      document.getElementById('balanceAmount').innerText = balance.toFixed(2);
    }
  </script></body>
</html>
