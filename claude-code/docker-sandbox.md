
# Guide: Initializing Claude Code with LAN Access in Docker Sandbox

This guide documents the full "clean slate" workflow for setting up **Claude Code** within a **Docker Sandbox** with custom local network permissions.

## 1. Reset & Create the Sandbox

To ensure a fresh environment, remove any existing instances and create a new one named `cc-test`.

```bash
# Delete the existing sandbox (if it exists)
docker sandbox rm cc-test

# Create the new sandbox
docker sandbox create cc-test

```

## 2. Configure Network Authorization

By default, the sandbox is isolated from your local network. You must explicitly allow the specific IP and port where your local API or service is running.

**The Command Used:**

```bash
docker sandbox network proxy cc-test --policy deny --allow-host 192.168.1.244:1234

```

* **`--policy deny`**: Blocks all external/internal traffic by default.
* **`--allow-host`**: Creates a specific "hole" in the firewall for your local service.

## 3. Prepare Environment Variables (`.env`)

Claude Code needs to know to look at your local IP instead of the standard Anthropic API. Ensure your `.env` file in `/Users/frefur/Projects/docker-sandbox` is configured as follows:

**File Content (`.env`):**

```text
ANTHROPIC_BASE_URL=http://192.168.1.244:1234
ANTHROPIC_AUTH_TOKEN=<ANY-STRING-HERE>

```

To verify the file is ready:

```bash
cat .env

```

## 4.  Run a test with curl
```bash
curl  -v 192.168.1.244:1234/v1/models
* Uses proxy env variable no_proxy == 'localhost,127.0.0.1,::1'
* Uses proxy env variable http_proxy == 'http://host.docker.internal:3128'
* Host host.docker.internal:3128 was resolved.
* IPv6: (none)
* IPv4: 192.168.65.254
*   Trying 192.168.65.254:3128...
* Connected to host.docker.internal (192.168.65.254) port 3128
* using HTTP/1.x
> GET http://192.168.1.244:1234/v1/models HTTP/1.1
> Host: 192.168.1.244:1234
> User-Agent: curl/8.14.1
> Accept: */*
> Proxy-Connection: Keep-Alive
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Connection: keep-alive
< Content-Length: 390
< Content-Type: application/json; charset=utf-8
< Date: Sat, 14 Feb 2026 10:48:10 GMT
< Etag: W/"186-z2CpBnur2nzfkzCeXuT0Eu66SjU"
< Keep-Alive: timeout=5
< X-Powered-By: Express
< 
{
  "data": [
    {
      "id": "qwen3-coder-next",
      "object": "model",
      "owned_by": "organization_owner"
    },
    {
      "id": "zai-org/glm-4.7-flash",
      "object": "model",
      "owned_by": "organization_owner"
    },
    {
      "id": "text-embedding-nomic-embed-text-v1.5",
      "object": "model",
      "owned_by": "organization_owner"
    }
  ],
  "object": "list"
* Connection #0 to host host.docker.internal left intact
}
```

## 4. Execute Claude Code

Once the sandbox is created and the network is "punched through," you can launch the agent:

```bash
# Start the Claude Code interface
claude

```
##  Send a quick message to Claude
```bash

╭─── Claude Code v2.1.42 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│                                           │ Tips for getting started                                                                                                                                              │
│               Welcome back!               │ Run /init to create a CLAUDE.md file with instructions for Claude                                                                                                     │
│                                           │ ─────────────────────────────────────────────────────────────────                                                                                                     │
│                                           │ Recent activity                                                                                                                                                       │
│                  ▐▛███▜▌                  │ No recent activity                                                                                                                                                    │
│                 ▝▜█████▛▘                 │                                                                                                                                                                       │
│                   ▘▘ ▝▝                   │                                                                                                                                                                       │
│      Sonnet 4.5 · API Usage Billing       │                                                                                                                                                                       │
│   /Users/frefur/Projects/docker-sandbox   │                                                                                                                                                                       │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
                                                         
  /model to try Opus 4.6                  
                       
❯ hey                                                                                                                                                                                                                
                                                                                                                                                                                                                     
● Hello! How can I help you today?                                                                                                                                                                                   
                                                                                                                                                                                                                     
✻ Baked for 35s     
```
