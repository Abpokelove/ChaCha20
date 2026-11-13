# # ChaCha20 Playground — Interactive Stream Cipher Lab

An interactive, zero-backend Virtual Lab website that demonstrates the **ChaCha20** symmetric stream cipher (RFC 8439). It shows how a 256-bit key, a 96-bit nonce and a block counter turn into a keystream, and how that keystream encrypts and decrypts data, with step-by-step visualisation of the 4×4 state matrix and the quarter-round.

## Team Members

* Afrin Braulio A
* Abhishek P
* Akshay N

## About the Algorithm

ChaCha20 was designed by Daniel J. Bernstein in 2008 as a refinement of Salsa20. It is an **ARX cipher**: it uses only **A**ddition, **R**otation and **X**OR, so it is fast in software and resistant to timing attacks. It is used in TLS 1.3, SSH and WireGuard, especially on mobile devices.

### Core Phases

1. **State setup:** a 4×4 matrix of sixteen 32-bit words is built from 4 constants (`"expand 32-byte k"`), 8 key words, 1 block counter and 3 nonce words.
2. **20 rounds of mixing:** each round applies four *quarter-rounds*. Odd rounds mix the columns, even rounds mix the diagonals.
3. **Add back the input:** the starting state is added to the mixed state, which makes the block one-way. The result is 64 bytes of keystream.
4. **Bitwise XOR (⊕):**
   * Encryption: `Cᵢ = Pᵢ ⊕ Kᵢ`
   * Decryption: `Pᵢ = Cᵢ ⊕ Kᵢ`

### Quarter-round

```
a += b; d ^= a; d <<<= 16;
c += d; b ^= c; b <<<= 12;
a += b; d ^= a; d <<<= 8;
c += d; b ^= c; b <<<= 7;
```

## Features

* **100% frontend:** plain HTML5, CSS3 and vanilla JavaScript. No backend and no libraries.
* **Four colour themes:** Violet, Midnight, Neon and Sunset, switched from the navigation bar. The choice is remembered in the browser.
* **State matrix visualiser:** the 4×4 matrix is colour-coded (constants, key, counter, nonce). Play or drag through all 20 rounds and watch the changed words flip.
* **Quarter-round lab:** step through all 12 operations on a, b, c and d, with the rotated bits highlighted.
* **Avalanche effect demo:** flip one random key bit and see about half of the 512 keystream bits change.
* **Bit-by-bit XOR view:** shows plaintext byte, keystream byte and ciphertext byte with animation.
* **Universal input:** UTF-8 text, numbers, symbols and emojis, with hex output. Decrypt mode turns hex back into text.
* **RFC 8439 verification:** one-click test vector and a self-test for the quarter-round and block function.
* **Story-style sections:** What it does, How it works, Try it, Playground, Test yourself, Go deeper and The crew.
* **Mobile responsive:** works on phones, tablets and desktops, with a collapsing menu and scrollable panels.
* **Accessible:** keyboard focus styles, labelled controls and reduced-motion support.

## Project Structure

```
.
├── index.html   # the whole website (HTML, CSS and JavaScript)
└── README.md    # this file
```

## How to Run Locally

1. Download both files into one folder.
2. Double-click `index.html` to open it in any modern browser.

No installation or internet connection is needed for the simulation. Only the Google Fonts need internet, and the page falls back to system fonts without it.

Optional: serve it with a local server.

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## How to Use

1. Choose **Encrypt** or **Decrypt**.
2. Enter a message, a 64-hex-character key and a 24-hex-character nonce, or press **Random**.
3. Press **Load RFC 8439 vector** to reproduce the official test output.
4. Drag the round slider or press **Play** to watch the state mix.
5. Use the **Quarter-round lab** and **Avalanche effect** panels to explore the details.
6. Finish with the **quiz**.

## Security Note

Never reuse the same key and nonce for two messages. The keystream repeats and `C1 ⊕ C2 = P1 ⊕ P2` leaks information. This lab is for learning. For real systems use a vetted library with an authenticated mode such as ChaCha20-Poly1305.

## References

* RFC 8439 — ChaCha20 and Poly1305 for IETF Protocols
* D. J. Bernstein — *ChaCha, a variant of Salsa20*
* RFC 7905 — ChaCha20-Poly1305 Cipher Suites for TLS
* RFC 8446 — TLS 1.3