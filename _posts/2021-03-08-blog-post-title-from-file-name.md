## ICS Cyber Siege 2025 Prelim CTF Writeup
---
The team's first serious CTF, where we put our all into solving the questions. After some sleepless nights and patience, we were able to solve most of the baby questions (LOL).
Even though it's not an impressive start, we still had a ton of fun working together and overall just bonding while solving these CTF challenges.
---
### Rev

### Web
1. Baby web (PancakeSS)
   
As a person who started off doing web, this challenge reminded me why I hate web so much :). The source code is as follows:

```
const express = require('express');
const bodyParser = require('body-parser');
const crypto = require('crypto');
const app = express();
const port = 10009;

app.use(bodyParser.urlencoded({ extended: true }));

// Generate the key properly
const key = "randomBytes(16).toString('hex')";

const htmlPage = (result = '') => `
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Flag Vault</title>
</head>
<body>
  <div class="container">
    <h1>Flag Vault</h1>
    <p>Enter the exact key to unlock the flag</p>
    <form method="POST" action="/search">
      <input name="query" placeholder="Paste your key here..." required />
      <button type="submit">Unlock</button>
    </form>
    <pre>${result}</pre>
  </div>
</body>
</html>
`;

app.get('/', (req, res) => {
  res.send(htmlPage());
});

app.post('/search', (req, res) => {
  const query = req.body.query;

  if (query && query.includes && query.includes("String")) {
    return res.send(htmlPage(`❌ Access Denied: Suspicious pattern detected.\n\n📦 Query was:\n${JSON.stringify(query, null, 2)}`));
  }

  if (query && query.includes && query.includes(key)) {
    return res.send(htmlPage(`✅ Key matched: ${query}\n🎉 Here is your flag: fakeflag{not the flag, and i love teh ais :D}`));
  } else {
    return res.send(htmlPage(`❌ Key did not match.\n\n📦 Query was:\n${JSON.stringify(query, null, 2)}`));
  }
});

app.listen(port, () => {
  console.log(`🚀 Challenge running at http://localhost:${port}`);
});
```
I deleted the style section as it serves no purpose. 

![image](https://github.com/user-attachments/assets/52261114-c21c-465d-8593-2aa6167e8c2a)

The challenge revolves around sending a key that, if you look at the top, is hardcoded. No obfuscation, it's just there. The key is `const key = "randomBytes(16).toString('hex')";` which tries to disguise itself as code. But there is a check for the work "String", and as you can see, the key has the word "String" in it.

After hours of cracking my head, a Google search for bodyParser.urlencoded from express.js shows that it can also take in objects as input. So we put our key in object form, but it's still getting rejected

![image](https://github.com/user-attachments/assets/9c51fa71-1c66-452a-9be3-7c829431fcfe)

So we url encoded the brackets so that it gets url decoded and treated as an object and we can get the flag.

`query%5B%5D=randomBytes(16).toString('hex')`


### Mobile

### Crypto

### Blockchain

### Pwn
