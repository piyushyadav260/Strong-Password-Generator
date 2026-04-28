# 🔐 VaultKey — Strong Password Generator

A cryptographically secure, beautifully designed password generator that runs entirely in the browser. No backend required. No data ever leaves your device.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🎲 Crypto-safe RNG | Uses `crypto.getRandomValues()` with rejection sampling to eliminate modulo bias |
| 📐 Entropy analysis | Displays real-time entropy in bits (Strong ≥ 80 bits) |
| 🔢 Custom length | Slider from 8 to 32 characters |
| 🔠 Character sets | Uppercase, Lowercase, Numbers, Symbols — toggle any combination |
| 💪 Strength meter | 4-bar visual indicator: Weak / Fair / Good / Strong |
| 📋 Copy to clipboard | One-click copy with toast confirmation |
| 📜 Session history | Last 10 passwords with strength badges and timestamps |
| 💾 Download | Export current password or entire history as `.txt` |
| 🌙 Dark / Light mode | Toggle with the moon/sun button |
| 🚫 No Math.random() | Never uses insecure pseudo-random generation |

---

## 🗂️ Project Structure

```
vaultkey/
├── index.html          ← Full app (HTML + CSS + JS, self-contained)
└── README.md           ← This file
```

> The app is intentionally self-contained in a single HTML file for maximum portability. No build tools, no npm, no server.

---

## 🚀 How to Run Locally

### Option 1 — Just open the file
```bash
# Double-click index.html, or:
open index.html         # macOS
start index.html        # Windows
xdg-open index.html     # Linux
```

### Option 2 — Local HTTP server (recommended for clipboard API)
```bash
# Python 3
python -m http.server 8080
# Then visit: http://localhost:8080

# Node.js (npx)
npx serve .
```

---

## 🔐 Security Implementation

### 1. Cryptographically Secure Random Number Generation
```javascript
function getSecureRandomInt(max) {
  const arr = new Uint32Array(1);
  const limit = Math.floor(0xFFFFFFFF / max) * max;
  let val;
  do {
    crypto.getRandomValues(arr);    // CSPRNG — not Math.random()
    val = arr[0];
  } while (val >= limit);           // Rejection sampling — no modulo bias
  return val % max;
}
```

### 2. Password Entropy Formula
```
Entropy (bits) = Length × log₂(Charset Size)

Examples:
  16 chars, all sets (95 chars)  → 105 bits  ✅ Strong
  8  chars, lowercase only (26)  → 37 bits   ❌ Weak
  12 chars, upper+lower+nums     → 72 bits   ⚠️  Good
```

### 3. Guaranteed Character Diversity
At least one character from each selected charset is guaranteed before random fill, then the entire password is shuffled using Fisher-Yates with CSPRNG — so there are no predictable positional patterns.

### 4. Input Validation
- Minimum 1 charset must be selected
- Length clamped to [8, 32]
- All inputs sanitised before use

---

## 📊 Sample Output

```
Password:  aK9#mZqP!vR2@cXw
Strength:  STRONG (102 bits entropy)
Length:    16
Charsets:  Uppercase + Lowercase + Numbers + Symbols

Password:  trumpet44
Strength:  WEAK (42 bits entropy)
Length:    9
Charsets:  Lowercase + Numbers
```

---

## 🏗️ How to Extend with a Backend (Optional)

If you want an API endpoint for password generation, here's a minimal Flask example:

### Python Flask API
```python
# app.py
from flask import Flask, jsonify, request
import secrets, string

app = Flask(__name__)

@app.route('/api/generate', methods=['GET'])
def generate():
    length   = int(request.args.get('length', 16))
    charset  = ''
    if request.args.get('upper')   == '1': charset += string.ascii_uppercase
    if request.args.get('lower')   == '1': charset += string.ascii_lowercase
    if request.args.get('numbers') == '1': charset += string.digits
    if request.args.get('symbols') == '1': charset += string.punctuation
    if not charset: return jsonify(error='No charset selected'), 400
    password = ''.join(secrets.choice(charset) for _ in range(length))
    return jsonify(password=password, length=length, charset_size=len(set(charset)))

if __name__ == '__main__':
    app.run(debug=True)
```

```bash
pip install flask
python app.py
# API: GET http://localhost:5000/api/generate?length=20&upper=1&lower=1&numbers=1&symbols=1
```

### requirements.txt
```
flask>=3.0.0
```

---

## 📄 License

MIT — free to use, modify, and distribute.

---

*Built with 🔐 VaultKey — because your passwords deserve better.*
