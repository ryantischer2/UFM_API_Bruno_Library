# NVIDIA UFM Enterprise REST API — Bruno Collection

> **126 ready-to-fire requests** covering the full NVIDIA Unified Fabric Manager (UFM) Enterprise REST API, packaged as an open-source [Bruno](https://www.usebruno.com/) collection.

---

## Why Bruno?

Bruno is a fast, Git-friendly, offline-first API client. Collections live as plain-text `.bru` files right in your repo — no cloud sync, no accounts, no lock-in. Perfect for infrastructure APIs like UFM where you want version-controlled, reproducible requests.

---

## Quick Start

### 1. Install Bruno

Download from **[usebruno.com](https://www.usebruno.com/downloads)** (macOS, Windows, Linux) or install via Homebrew:

```bash
brew install bruno
```

### 2. Clone this repository

```bash
git clone https://github.com/ryantischer2/UFM_API_Bruno_Library.git
```

### 3. Open the collection

Launch Bruno and click **Open Collection**, then navigate to the cloned `UFM_API_Bruno_Library` folder and select it.

### 4. Configure the environment

This is the critical step — you need to tell Bruno where your UFM instance lives.

#### Option A — Use the built-in environment (recommended)

1. In Bruno, click the **Environment** dropdown (top-right) and select **UFM Enterprise**.
2. Click the **gear icon** next to the environment name to edit variables.
3. Set the following values:

| Variable | Description | Example |
|---|---|---|
| `ufm_host` | IP or hostname of your UFM appliance | `10.20.30.40` |
| `username` | UFM admin username | `admin` |
| `password` | UFM admin password | `yourpassword` |

> `base_url` is automatically derived as `https://{{ufm_host}}/ufmRest` — you don't need to touch it.

4. Click **Save** and make sure **UFM Enterprise** is selected as the active environment.

#### Option B — Import the environment file

If the environment doesn't appear automatically:

1. In Bruno, open the collection.
2. Click the **Environment** dropdown → **Configure** (or **Manage Environments**).
3. Click **Import** and select the file at:

```
environments/UFM Enterprise.bru
```

4. Edit the variables as described in Option A.

#### Option C — Import as a global environment

For use across multiple Bruno collections:

1. Go to Bruno **Preferences** → **Environments** → **Import**.
2. Select the `UFM Enterprise.json` file at the root of this repo.
3. Fill in your `ufm_host`, `username`, and `password`.

### 5. Send a request

Expand any folder, click a request, and hit **Send**. All requests inherit Basic Auth from the collection, so authentication is handled automatically.

> **Note:** UFM uses a self-signed TLS certificate by default. If you get certificate errors, go to Bruno **Preferences** → **SSL/TLS Verification** → toggle off, or configure your CA.

---

## Collection Structure

```
UFM_API_Bruno_Library/
├── bruno.json                        # Collection config
├── environments/
│   └── UFM Enterprise.bru            # Environment variables
├── UFM Enterprise.json               # Importable global environment
│
├── Alarms/                           #  4 requests
├── Configuration/                    #  1 request
├── Credentials/                      #  4 requests
├── Events/                           #  4 requests
├── Events Policy/                    #  6 requests
├── Fabric Discovery/                 #  1 request
├── Fabric Validation/                #  2 requests
├── Groups/                           #  7 requests
├── Jobs/                             #  5 requests
├── Links/                            #  2 requests
├── Logical Model/
│   ├── Computes/                     #  1 request
│   ├── Environments/                 #  5 requests
│   ├── Logical Servers/              #  3 requests
│   └── Networks/                     #  1 request
├── Mirroring/                        #  5 requests
├── Modules/                          #  1 request
├── Monitoring/
│   ├── Jobs/                         #  1 request
│   ├── Sessions/                     #  5 requests
│   └── Templates/                    #  5 requests
│   └── (top-level)                   #  6 requests
├── Non-Optimal Links/                #  3 requests
├── Periodic Health/                  #  2 requests
├── PKeys/                            #  2 requests
├── Ports/                            #  7 requests
├── Reports/                          #  4 requests
├── SM Configuration/                 #  2 requests
├── SMTP Configuration/               #  2 requests
├── System Log/                       #  2 requests
├── Systems/                          #  6 requests
├── Telemetry/                        #  3 requests
├── Templates/                        #  5 requests
├── UFM Version/                      #  1 request
├── Unhealthy Devices/                #  2 requests
├── Unhealthy Ports/                  #  9 requests
├── Users/                            #  5 requests
├── Versioning/                       #  1 request
└── Virtualization/                   #  1 request
```

---

## API Coverage

| Category | Endpoints | Methods |
|---|---|---|
| **Monitoring** | Sessions, Templates, Jobs | GET · POST · PUT · DELETE |
| **Templates** | Configuration templates | GET · POST · PUT · DELETE |
| **Systems** | Managed systems & properties | GET · POST · PUT |
| **Ports** | Physical & logical ports | GET |
| **Links** | Inter-switch links | GET |
| **PKeys** | Partition keys | GET · POST · PUT · DELETE |
| **Groups** | Device groups | GET · POST · PUT · DELETE |
| **Events & Alarms** | Event log, alarms, policies | GET · PUT · PATCH · DELETE |
| **Jobs** | Async job status & results | GET · DELETE |
| **Fabric Discovery** | Topology discovery triggers | POST |
| **Fabric Validation** | Validation reports | GET · POST |
| **Mirroring** | Port mirroring sessions | GET · POST · PUT · DELETE |
| **Unhealthy Ports/Devices** | Diagnostics | GET · POST · DELETE |
| **Reports** | Report generation | GET · POST · DELETE |
| **Telemetry** | Telemetry config & data | GET · POST · PUT |
| **Configuration** | Global UFM config | GET · PUT |
| **Users & Credentials** | User management, SSH creds | GET · POST · PUT · DELETE |
| **SM Configuration** | Subnet Manager config | GET · PUT |
| **SMTP Configuration** | Mail server settings | GET · PUT |
| **Logical Model** | Computes, servers, networks | GET · POST · PUT · DELETE |
| **Periodic Health** | Scheduled health checks | GET · POST |
| **Virtualization** | SR-IOV VFs | GET |
| **Versioning** | API version info | GET |
| **Access Tokens** | Token-based auth | GET · POST · DELETE |

---

## Environment Variables Reference

| Variable | Default | Description |
|---|---|---|
| `ufm_host` | *(empty — you must set this)* | IP address or FQDN of your UFM server |
| `base_url` | `https://{{ufm_host}}/ufmRest` | Auto-constructed API base URL |
| `username` | `admin` | UFM username for Basic Auth |
| `password` | `admin` | UFM password for Basic Auth |

All requests use `auth: inherit`, pulling credentials from the collection-level Basic Auth config which references `{{username}}` and `{{password}}`.

---

## Tips & Tricks

- **Path parameters** — Requests that need a dynamic ID (e.g., `/systems/:system_name`) have the parameter pre-filled with a placeholder. Replace it in the **Params** tab before sending.
- **Optional query params** — Many GET requests include optional query parameters (prefixed with `~` in the `.bru` source) that are disabled by default. Enable them in the **Params** tab to filter results.
- **Request bodies** — POST/PUT/PATCH requests include example JSON bodies based on the official UFM API documentation. Modify them to match your use case.
- **V2 endpoints** — A few requests target the `/ufmRestV2/` path (e.g., Periodic Health). These use a full URL rather than `{{base_url}}` since the base path differs.

---

## Requirements

- [Bruno](https://www.usebruno.com/downloads) v1.0+
- NVIDIA UFM Enterprise v6.x (API documented against v6.24.2)
- Network access to your UFM appliance on port 443 (HTTPS)

---

## Contributing

1. Fork this repo
2. Create a feature branch (`git checkout -b add-new-endpoints`)
3. Add or update `.bru` files following the existing conventions
4. Commit and push (`git push origin add-new-endpoints`)
5. Open a Pull Request

All requests should:
- Use `{{base_url}}` (or the full host URL for V2 endpoints)
- Set `auth: inherit`
- Include `Content-Type: application/json` where applicable
- Have a descriptive `name` in the `meta` block

---

## License

This collection is provided as-is for use with NVIDIA UFM Enterprise. The API specification is owned by NVIDIA Corporation. See the [official UFM REST API documentation](https://docs.nvidia.com/networking/display/UFMEnterpriseRESTAPILatest) for authoritative reference.

---

**Built with [Bruno](https://www.usebruno.com/)** — the open-source API client for developers who care about their tools.
