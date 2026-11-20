# NETWORKWALKS-HASANTARIQ-B084-WEEK3-PASSWORD-CRACKING

Week 3 submission for the Networkwalks Cybersecurity Internship (Batch B084). This week covers password cracking on password-protected PDF files, done two different ways: once with John the Ripper from the macOS Terminal, and once with Networkwalks' own browser-based Hash Calculator and Password Cracker.
---

## Overview

Both projects target the same three course-provided files:

- `My-Locked-PDF1.pdf`
- `My-Locked-PDF2.pdf`
- `My-Locked-PDF3.pdf`

The idea was to crack the same files twice, with two completely different tools, and then compare the results. If both tools agree, that's a good sign the work is correct and not a fluke.

| Project | Tool | Result |
|---|---|---|
| Project 1 | John the Ripper (Jumbo), macOS Terminal | All 3 files cracked |
| Project 2 | Networkwalks Hash Calculator + Password Cracker (browser) | All 3 files cracked, same passwords |

---

## A note on platform and tooling

The course material was written with Kali Linux and the Johnny GUI in mind. I did this week on a MacBook Air (Apple M4), so I used John the Ripper's Jumbo edition from the Terminal instead, since there's no simple Mac install for the Johnny GUI. I checked with my instructor before going ahead with this, and got the following confirmation:

> "Hi! Yes, you can complete Week 3 using John the Ripper through your Mac Terminal. The GUI version (Johnny) is not mandatory. Just capture screenshots of hash extraction, password recovery and verification, and include them in your report. Mention that you used macOS Terminal instead of Johnny GUI."

Johnny is just a GUI wrapper around the same John the Ripper engine, so running the same commands from the Terminal produces identical results. This is called out explicitly in both reports.

---

## Project 1: Password Cracking Using John the Ripper

**Setup:**
```bash
brew install john-jumbo
```

**Workflow for each file:**

1. Extract a crackable hash from the PDF using John's bundled `pdf2john.pl` script
2. Run `john` against the hash using John's default wordlist and mangling rules
3. Confirm the recovered password with `john --show`

**Results:**

| File | Cracked Password |
|---|---|
| My-Locked-PDF1.pdf | `password1` |
| My-Locked-PDF2.pdf | `password1` |
| My-Locked-PDF3.pdf | `1qaz2wsx` |

`1qaz2wsx` is a common keyboard-walk pattern (it's not a dictionary word, it's the shape your fingers make on a QWERTY keyboard). It wasn't a literal entry in John's wordlist, but John's mangling rules caught it anyway.

**Screenshots:**

| File | Description |
|---|---|
| `screenshots/pm1-00-setup-check.png` | John the Ripper installed and verified |
| `screenshots/pm1-01-pdf1-crack.png` | Hash extraction and crack for PDF1 |
| `screenshots/pm1-02-pdf2-crack.png` | Hash extraction and crack for PDF2 |
| `screenshots/pm1-03-pdf3-crack.png` | Hash extraction and crack for PDF3 |
| `screenshots/pm1-04-pdf1-flag.png` | PDF1 unlocked and verified |
| `screenshots/pm1-05-pdf2-unlocked.png` | PDF2 unlocked and verified |
| `screenshots/pm1-06-pdf3-flag.png` | PDF3 unlocked and verified |

Full write-up: [`Week 3 Project 1 - Password Cracking Using John the Ripper.pdf`](<./Week%203%20Project%201%20-%20Password%20Cracking%20Using%20John%20the%20Ripper.pdf>)

---

## Project 2: Password Cracking Using Networkwalks Tools

Same three files, this time using Networkwalks' own browser-based tools instead of a Terminal:

- **Hash Calculator** (networkwalks.com/hash-calculator) extracts the `$pdf$...` hash from a PDF entirely inside the browser
- **Password Cracker** (networkwalks.com/password-cracker) runs that hash against a built-in 100-password list

**Results:**

| File | Cracked Password | Found at |
|---|---|---|
| My-Locked-PDF1.pdf | `password1` | 91/100 |
| My-Locked-PDF2.pdf | `password1` | 91/100 |
| My-Locked-PDF3.pdf | `1qaz2wsx` | 35/100 |

**Cross-check against Project 1:** the hashes extracted by the Hash Calculator matched byte-for-byte with the hashes extracted by `pdf2john` in Project 1, for all three files. Same hashes, same passwords, two independent tools. That's strong confirmation the results are correct.

One interesting difference: `1qaz2wsx` needed John's mangling rules to be found in Project 1, but the Networkwalks list has it as a literal entry, which is why it was found much earlier (35/100 instead of needing rule-based transformations).

**Screenshots:**

| File | Description |
|---|---|
| `screenshots/pm2-01-pdf1-hash.png` | Hash extracted for PDF1 |
| `screenshots/pm2-02-pdf1-cracked.png` | Crack result for PDF1 |
| `screenshots/pm2-03-pdf2-hash.png` | Hash extracted for PDF2 |
| `screenshots/pm2-04-pdf2-cracked.png` | Crack result for PDF2 |
| `screenshots/pm2-05-pdf3-hash.png` | Hash extracted for PDF3 |
| `screenshots/pm2-06-pdf3-cracked.png` | Crack result for PDF3 |

Full write-up: [`Week 3 Project 2 - Password Cracking Using Networkwalks Tools.pdf`](<./Week%203%20Project%202%20-%20Password%20Cracking%20Using%20Networkwalks%20Tools.pdf>)

---

## What I learned

- A password-protected PDF's encryption can be reduced to a hash, and once you have that hash, the strength of the password is the only thing standing between you and the file.
- A tool's interface doesn't change what it's doing underneath. Johnny GUI, the John the Ripper Terminal, and the Networkwalks browser tools are all attacking the same `$pdf$` hash format, just with different front-ends.
- Wordlists and mangling rules each catch different weak passwords. A bigger wordlist with transformation rules and a smaller list with common patterns built in can both land on the same answer, just by different routes.
- Cross-checking results between two independent tools is a simple but effective way to confirm your work is actually correct.

## Ethical note

All three files used in this exercise were provided by Networkwalks specifically for training purposes. No third-party system, account, or file was accessed at any point. This work was carried out strictly for educational purposes as part of the Networkwalks Cybersecurity Internship.

---

## Repository structure

```
.
├── README.md
├── Week 3 Project 1 - Password Cracking Using John the Ripper.pdf
├── Week 3 Project 2 - Password Cracking Using Networkwalks Tools.pdf
└── screenshots/
    ├── pm1-00-setup-check.png
    ├── pm1-01-pdf1-crack.png
    ├── pm1-02-pdf2-crack.png
    ├── pm1-03-pdf3-crack.png
    ├── pm1-04-pdf1-flag.png
    ├── pm1-05-pdf2-unlocked.png
    ├── pm1-06-pdf3-flag.png
    ├── pm2-01-pdf1-hash.png
    ├── pm2-02-pdf1-cracked.png
    ├── pm2-03-pdf2-hash.png
    ├── pm2-04-pdf2-cracked.png
    ├── pm2-05-pdf3-hash.png
    └── pm2-06-pdf3-cracked.png
```

**Intern:** Muhammad Hasan Tariq

