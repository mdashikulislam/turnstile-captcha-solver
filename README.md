<div align="center">
  <h2 align="center">Cloudflare - Turnstile Solver</h2>
</div>



### ❗ Disclaimers
- I am not responsible for anything that may happen, such as API Blocking, IP ban, etc.
- This was a quick project that was made for fun and personal use if you want to see further updates, star the repo & create an "issue" [here]https://github.com/mdashikulislam/turnstile-captcha-solver

---

### ⚙️ Installation Instructions

1. **Ensure Python 3.8+ is installed** on your system.

2. **Create a Python virtual environment**:
   ```bash
   python -m venv venv
   ```

3. **Activate the virtual environment**:
   - On **Windows**:
     ```bash
     venv\Scripts\activate
     ```
   - On **macOS/Linux**:
     ```bash
     source venv/bin/activate
     ```

4. **Install required dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

5. **Select the browser to install**:
   You can choose between **Chromium**, **Chrome**, **Edge** or **Camoufox**:
   - To install **Chromium**:
     ```bash
     python -m patchright install chromium
     ```
   - To install **Chrome**:
     - On **macOS/Windows**: [Click here](https://www.google.com/chrome/)  
     - On **Linux (Debian/Ubuntu-based)**:
       ```bash
       apt update
       wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
       apt install -y ./google-chrome-stable_current_amd64.deb
       apt -f install -y  # Fix dependencies if needed
       rm ./google-chrome-stable_current_amd64.deb
       ```
   - To install **Edge**:
     ```bash
     python -m patchright install msedge
     ```
   - To install **Camoufox**:
     ```bash
     python -m camoufox fetch
     ```

6. **Start testing**:
   - Run the script (Check [🔧 Command line arguments](#-command-line-arguments) for better setup):
     ```bash
     python api_solver.py
     ```
     
---

### 🔧 Command line arguments
| Parameter     | Default   | Type      | Description                                                                                   |
|--------------|-----------|-----------|-----------------------------------------------------------------------------------------------|
| `--headless`   | `False`  | `boolean` | Runs the browser in headless mode. Requires the `--useragent` argument to be set.             |
| `--useragent`  | `None`   | `string`  | Specifies a custom User-Agent string for the browser. (No need to set if camoufox used)                                        |
| `--debug`      | `False`  | `boolean` | Enables or disables debug mode for additional logging and troubleshooting.                   |
| `--browser_type` | `chromium`  | `string` | Specify the browser type for the solver. Supported options: chromium, chrome, msedge, camoufox      |
| `--thread`     | `1`      | `integer` | Sets the number of browser threads to use in multi-threaded mode.                           |
| `--host`       | `127.0.0.1` | `string`  | Specifies the IP address the API solver runs on.                                            |
| `--port`       | `5000`   | `integer` | Sets the port the API solver listens on.                                                    |
| `--proxy`       | `False`   | `boolean` | Select a random proxy from proxies.txt for solving captchas                                                   |

---

### 🐳 Docker Image
#### Running the Container
To start the container, use:
- Change the TZ environment variable and ports to the correct one for yourself:
```sh
docker run -d -p 3389:3389 -p 5000:5000 -e TZ=Asia/Baku --name turnstile_solver theyka/turnstile_solver:latest
```

#### Connecting to the Container
1. Use an **RDP client** (like Windows Remote Desktop, Remmina, or FreeRDP)
2. Connect to `localhost:3389`
3. Login with the default user:
   - **Username:** root
   - **Password:** root
4. After this, you can start the solver by navigating to the `Turnstile-Solver` folder.

---

### 📡 API Documentation
#### Solve turnstile
```http
  GET /turnstile?url=https://example.com&sitekey=0x4AAAAAAA
```
#### Request Parameters:
| Parameter  | Type    | Description                                                                 | Required |
|------------|---------|-----------------------------------------------------------------------------|----------|
| `url`      | string  | The target URL containing the CAPTCHA. (e.g., `https://example.com`) | Yes      |
| `sitekey`  | string  | The site key for the CAPTCHA to be solved. (e.g., `0x4AAAAAAA`) | Yes      |
| `action`   | string  | Action to trigger during CAPTCHA solving, e.g., `login`            | No       |
| `cdata`    | string  | Custom data that can be used for additional CAPTCHA parameters.    | No       |

#### Response:

If the request is successfully received, the server will respond with a `task_id` for the CAPTCHA solving task:

```json
{
  "task_id": "d2cbb257-9c37-4f9c-9bc7-1eaee72d96a8"
}
```

#### Get Result
```http
  GET /result?id=f0dbe75b-fa76-41ad-89aa-4d3a392040af
```

#### Request Parameters:

| Parameter  | Type    | Description                                                                 | Required |
|------------|---------|-----------------------------------------------------------------------------|----------|
| `id`       | string  | The unique task ID returned from the `/turnstile` request.                   | Yes      |

#### Response:

If the CAPTCHA is solved successfully, the server will respond with the following information:

```json
{
  "elapsed_time": 7.625,
  "value": "0.KBtT-r"
}
```


