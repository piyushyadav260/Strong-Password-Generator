# 🔐 VaultKey — Strong Password Generator

A **cryptographically secure**, beautifully designed password generator that runs entirely in the browser.
No backend required. **No data ever leaves your device.**

---

## ✨ Features

| Feature              | Details                                                                          |
| -------------------- | -------------------------------------------------------------------------------- |
| 🎲 Crypto-safe RNG   | Uses `crypto.getRandomValues()` with rejection sampling to eliminate modulo bias |
| 📐 Entropy analysis  | Displays real-time entropy in bits *(Strong ≥ 80 bits)*                          |
| 🔢 Custom length     | Adjustable slider from 8 to 32 characters                                        |
| 🔠 Character sets    | Toggle Uppercase, Lowercase, Numbers, Symbols                                    |
| 💪 Strength meter    | 4-level indicator: Weak / Fair / Good / Strong                                   |
| 📋 Copy to clipboard | One-click copy with confirmation                                                 |
| 📜 Session history   | Stores last 10 generated passwords with timestamps                               |
| 💾 Download          | Export password or history as `.txt`                                             |
| 🌙 Dark / Light mode | Toggle between themes                                                            |
| 🚫 No Math.random()  | Uses only secure random generation                                               |

---

## 🗂️ Project Structure

```
vaultkey/
├── index.html      # Complete app (HTML + CSS + JS)
└── README.md       # Documentation
```

> 💡 The app is fully self-contained in a single HTML file — no dependencies, no setup required.

---

## 🚀 How to Run

### 🔹 Option 1: Open directly

```bash
open index.html         # macOS
start index.html        # Windows
xdg-open index.html     # Linux
```

### 🔹 Option 2: Run a local server (recommended)

```bash
# Python
python -m http.server 8080

# Node.js
npx serve .
```

Then open:
👉 [http://localhost:8080](http://localhost:8080)

---

## 🔐 Security Implementation

### 1. Cryptographically Secure RNG

```javascript
function getSecureRandomInt(max) {
  const arr = new Uint32Array(1);
  const limit = Math.floor(0xFFFFFFFF / max) * max;
  let val;
  do {
    crypto.getRandomValues(arr);
    val = arr[0];
  } while (val >= limit);
  return val % max;
}
```

---

### 2. Password Entropy Formula

```
Entropy (bits) = Length × log₂(Charset Size)
```

**Examples:**

* 16 chars (all sets) → ~105 bits ✅ Strong
* 12 chars (letters + numbers) → ~72 bits ⚠️ Good
* 8 chars (lowercase only) → ~37 bits ❌ Weak

---

### 3. Character Diversity

* Ensures at least one character from each selected set
* Uses **Fisher-Yates shuffle** with secure randomness
* Prevents predictable patterns

---

### 4. Input Validation

* At least one charset required
* Length restricted (8–32)
* Sanitized inputs

---

## 📊 Sample Output

```
Password:  aK9#mZqP!vR2@cXw
Strength:  STRONG (102 bits)
Length:    16

Password:  trumpet44
Strength:  WEAK (42 bits)
Length:    9
```

---

## 🏗️ Optional Backend Extension

You can extend this project with an API using Python Flask:

```python
from flask import Flask, jsonify, request
import secrets, string

app = Flask(__name__)

@app.route('/api/generate')
def generate():
    length = int(request.args.get('length', 16))
    charset = string.ascii_letters + string.digits + string.punctuation
    password = ''.join(secrets.choice(charset) for _ in range(length))
    return jsonify(password=password)

app.run()
```

---

## 🎯 Future Improvements

* 🔐 Password strength AI suggestions
* 🌐 Browser extension
* 📱 Mobile app version
* 🔑 Integration with password managers
* 🔒 Multi-factor authentication support

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 💡 Final Note

**VaultKey** is designed with one goal:
👉 *Make strong password generation simple, secure, and accessible.*

