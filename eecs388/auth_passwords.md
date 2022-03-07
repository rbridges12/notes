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

### CAPTCHAs
- **C**ompletely **A**utomated **P**ublic **T**uring tests to tell **C**omputers and **H**umans **A**part
- tasks that are relatively easy for humans to complete but extremely impractical or impossible for computers to complete

## Password breach defenses
- don't store passwords as plaintext, hash them
- even when hashed, passwords can be guessed by hashing a guess and comparing to the hashed password
- GPUs make offline password guessing much faster
- hash functions should be relatively slow and memory hard

### Salted password hashes
- site randomly generates a string of "salt" when password is set
- store \<salt, H(salt || password)\> in database
- identical passwords have different hashes
- hashes in database can't be compared to already cracked hashes from other leaks
- don't reuse salt
- make sure salt is long enough to avoid repeats

### Hardware password defense
- devices can have a "secure enclave" where password hash is stored
- can enforce rate limit and maximum guesses in hardware, so if an attacker steals device, they can't effectively perform offline guessing
- limits attacker to online guessing

## Password problems
- password reset mechanisms are necessary but easily attacked
- social engineering can be used to trick people
- security questions are often easy to guess or find answers to
- phishing attacks can trick users into giving up their passwords

### Multi Factor Authentication
- use another form of authentication in addition to your password
- other authentication form shouldn't be another password, it should be a different type of authentication
- 2FA with your phone uses one time passwords with secret key stored on your device, still susceptible to phishing
- 2FA with a physical USB or RFID key to contain a secret key is better because it isn't susceptible to phishing

### WebAuthn
- challenge response protocol exposed to web apps which supports universal second factor auth
- server sends challenge to browser, browser sends challenge and site origin to U2F device, U2F device signs challenge if the origin is correct and sends signature back to browser which sends it to the server

### Biometrics
- good bc they can't be lost or lent, inconvenient to spoof
- bad bc the sensors are often easy to beat
