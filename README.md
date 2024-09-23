# how-to-bypass-cloudflare-turnstile
How to bypass cloudflare turnstile




![Solve Cloudflare Captcha with Nodejs, Cloudflare Nodejs solver](https://assets.capsolver.com/prod/images/post/2023-10-18/24e2fc61-c832-429c-8f50-a12f27e0d6cb.png)
# Cloudflare Turnstile Overview
Cloudflare Turnstile offers a free solution for replacing traditional CAPTCHAs, providing a hassle-free web experience through a simple code snippet. It ensures that visitors are genuine and prevents abuse without the data privacy issues or poor user experience associated with conventional CAPTCHAs.

# Identifying Cloudflare Turnstile CAPTCHAs
- **Non-Interactive Challenge:** The process runs without user interaction. Example: [Non-Interactive Test](https://peet.ws/turnstile-test/non-interactive.html)
  ![](https://assets.capsolver.com/prod/images/post/2024-05-14/328b697d-936c-4736-b999-8f773bfc0a17.gif)
  
- **Minimal Interactive Challenge:** This may involve simple actions like clicking a button if the system suspects the visitor might be a bot. Example: [Managed Test](https://peet.ws/turnstile-test/managed.html)
  ![Cloudflare Turnstile Captcha](https://assets.capsolver.com/prod/images/post/2024-05-13/aafc0187-0920-4e61-8000-5347209fb122.png)
  
- **Invisible Challenge:** The challenge operates unseen, loading discreetly within the HTML of the webpage. Example: [Invisible Test](https://peet.ws/turnstile-test/invisible.html)
  ![](https://assets.capsolver.com/prod/images/post/2024-05-14/535da1a2-2aff-4e8c-9e3a-a48fc1024011.png)

# 🛠️ Solving cloudflare turnstile captcha 
## ⚙️ Prerequisites
- NodeJs installed
- Capsolver API key

## 🤖 Step 1: Install Necessary Packages
Execute the following commands to install the required packages:

```python
npm i axios
```

## 👨‍💻 Step 2: NodeJS Code for solve Cloudflare Turnstile Captcha

Here's a Python sample script to accomplish the task:

```js
const axios = require('axios');
const CAPSOLVER_API_KEY = "your api key";
const PAGE_URL = "site ";
const WEBSITE_KEY = "site key";

function solvecf(metadata_action = null, metadata_cdata = null) {
    const url = "https://api.capsolver.com/createTask";
    const task = {
        type: "AntiTurnstileTaskProxyLess",
        websiteURL: PAGE_URL,
        websiteKey: WEBSITE_KEY,
    };
    if (metadata_action || metadata_cdata) {
        task.metadata = {};
        if (metadata_action) {
            task.metadata.action = metadata_action;
        }
        if (metadata_cdata) {
            task.metadata.cdata = metadata_cdata;
        }
    }
    const data = {
        clientKey: CAPSOLVER_API_KEY,
        task: task
    };
    return axios.post(url, data)
        .then(response => {
            console.log(response.data);
            return response.data.taskId;
        });
}

function solutionGet(taskId) {
    const url = "https://api.capsolver.com/getTaskResult";
    let status = "";
    const checkStatus = () => {
        const data = { clientKey: CAPSOLVER_API_KEY, taskId: taskId };
        return axios.post(url, data)
            .then(response => {
                console.log(response.data);
                status = response.data.status || "";
                console.log(status);
                if (status === "ready") {
                    return response.data.solution;
                }
                return new Promise(resolve => setTimeout(resolve, 2000)).then(checkStatus);
            });
    };
    return checkStatus();
}

async function main() {
    try {
        const taskId = await solvecf();
        const solution = await solutionGet(taskId);
        if (solution) {
            const user_agent = solution.userAgent;
            const token = solution.token;
            console.log("User_Agent:", user_agent);
            console.log("Solved Turnstile Captcha, token:", token);
        }
    } catch (error) {
        console.error("Error in CAPSOLVER API interaction:", error);
    }
}

main();


```

## ⚠️ Change these variables
- **CAPSOLVER_API_KEY**: Obtain your API key from the [Capsolver Dashboard](https://capsolver.com/dashboard).
- **PAGE_URL**: Replace with the URL of the website for which you wish to solve the CloudFlare Turnstile Captcha.
- **WEBSITE_KEY**: Replace with the site key of the website 

### What the CloudFlare Turnstile Captcha Looks Like
![Cloudflare Turnstile Captcha](https://assets.capsolver.com/prod/images/post/2024-05-13/aafc0187-0920-4e61-8000-5347209fb122.png)



