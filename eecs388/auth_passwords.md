# Authentication
- confirming the truth of a single piece of data from a single entity

### Three ways to authenticate
- something you know (password, PIN, secret key)
- something you have (phone, security token, ID card)
- something you are (biometrics)

### Passwords
- not very good way to authenticate
- people are bad at choosing strong passwords
- never reuse passwords
- use 2 factor authentication
- use a password manager
- as a dev, prefer to outsource login to another site (Google, Facebook, Github, etc) and avoid restrictive password complexity or rotations

### Password guessing attack
- either guess a bunch of different passwords for one user on one site, or guess a few different passwords on a bunch of different sites
- defenses: lock acount after *n* guesses, rate limit login, anomaly detection, require CAPTCHA

