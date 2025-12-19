# SSIO FIWARE Project - Gateway with PEP Proxy

This project integrates a custom Node.js gateway service with the FIWARE ecosystem, implementing security and access control through Identity Management (Keyrock) and PEP Proxy (Wilma).

## Architecture

### Current: Orion-Wilma Architecture

The project currently uses the **Orion-Wilma** architecture, which provides:

- **Orion Context Broker**: NGSI-v2 context broker for managing context information
- **IoT Agent UltraLight**: Protocol adapter for IoT devices
- **Keyrock**: Identity Management system for authentication and authorization
- **Wilma PEP Proxy**: Policy Enforcement Point protecting Orion access
- **SSIO Gateway**: Custom backend service that integrates with all FIWARE components
- **MongoDB**: Database for Orion and IoT Agent
- **MySQL**: Database for Keyrock

### Component Flow

```
Client → Gateway (Port 3000) → Keyrock (Auth) → PEP Proxy (Port 1027) → Orion (Port 1026)
                               ↓
                         IoT Agent (Port 4041/7896)
```

### Future Architectures

The project structure supports future implementations of:
- **Northport Protection**: PEP Proxy between Orion and IoT Agent
- **Southport Protection**: PEP Proxy between IoT Agent and IoT Devices

Configuration files are available in `docker-compose/`:
- `northport-wilma.yml`
- `southport-wilma.yml`

## Quick Start

### Prerequisites

- Docker and Docker Compose installed
- Git

### Running the Project

1. Clone the repository:
```bash
git clone <repository-url>
cd tutorials.PEP-Proxy
```

2. Start all services:

**PowerShell (Windows):**
```powershell
Get-Content .env | ForEach-Object { if ($_ -notmatch '^#' -and $_ -match '=') { $name, $value = $_ -split '=', 2; [Environment]::SetEnvironmentVariable($name, $value, 'Process') } }; docker-compose -f docker-compose/orion-wilma.yml up -d
```

**Bash (Linux/Mac):**
```bash
./services orion
```

3. Wait for all services to be healthy (approximately 30-60 seconds)

## Accessing the Services

### Gateway API Documentation
Access the interactive API documentation (Swagger/OpenAPI):

**🔗 [http://localhost:3000/api/docs](http://localhost:3000/api/docs)**

### Other Services
- **Keyrock (Identity Management)**: http://localhost:3005
- **Orion (via PEP Proxy)**: http://localhost:1027
- **Orion (Direct)**: http://localhost:1026
- **IoT Agent**: http://localhost:4041

## Environment Variables

Key environment variables are configured in `.env`:

- `KEYROCK_CLIENT_ID`: OAuth2 client ID for gateway
- `KEYROCK_CLIENT_SECRET`: OAuth2 client secret
- `KEYROCK_APP_ID`: Application ID registered in Keyrock
- `JWT_SECRET`: Secret for JWT token signing
- `FIWARE_SERVICE`: FIWARE service header (default: `openiot`)
- `FIWARE_SERVICE_PATH`: FIWARE service path (default: `/`)

## Gateway Features

The SSIO Gateway provides:
- Authentication via Keyrock OAuth2
- JWT-based session management
- Secure communication with Orion through PEP Proxy
- IoT Agent integration for device management
- RESTful API with comprehensive documentation

## Services Breakdown

| Service | Container Name | Port | Description |
|---------|---------------|------|-------------|
| Gateway | ssio-gateway | 3000 | Custom backend API |
| Keyrock | fiware-keyrock | 3005 | Identity Management |
| Orion | fiware-orion | 1026 | Context Broker |
| PEP Proxy | fiware-orion-proxy | 1027 | Policy Enforcement |
| IoT Agent | fiware-iot-agent | 4041, 7896 | Device Protocol Adapter |
| MongoDB | db-mongo | 27017 | Orion/IoT Agent Database |
| MySQL | db-mysql | 3307 | Keyrock Database |

## Stopping the Services

```bash
docker-compose -f docker-compose/orion-wilma.yml down
```

To also remove volumes (databases):
```bash
docker-compose -f docker-compose/orion-wilma.yml down -v
```

## Development Status

🚧 **In Development**: Currently implementing and testing the Orion-Wilma architecture with the custom gateway service.

## License

See [LICENSE](LICENSE) file for details.
