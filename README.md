# Whisper API SDK - Bash

[![CI](https://github.com/whisper-sec/sdk-bash/actions/workflows/ci.yml/badge.svg)](https://github.com/whisper-sec/sdk-bash/actions/workflows/ci.yml)
[![Publish](https://github.com/whisper-sec/sdk-bash/actions/workflows/publish.yml/badge.svg)](https://github.com/whisper-sec/sdk-bash/actions/workflows/publish.yml)
[![GitHub release](https://img.shields.io/github/release/whisper-sec/sdk-bash.svg)](https://github.com/whisper-sec/sdk-bash/releases)
[![Bash](https://img.shields.io/badge/Bash-4.0%2B-green.svg)](https://www.gnu.org/software/bash/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## Overview

**The Foundational Intelligence Layer for the Internet**

This is a Bash client for the Whisper API v1, providing command-line access to comprehensive, real-time intelligence on any internet asset. By connecting billions of data points across live internet routing, historical registration records, and deep resolution data, our API moves beyond simple enrichment to deliver predictive, context-rich insights.

The script uses cURL underneath for making all REST calls, making it perfect for shell scripts, automation, and command-line workflows.

---

## 🚀 Quick Start

### 1. Installation

Choose your preferred installation method:

#### Quick Install (Recommended)
```bash
curl -fsSL https://github.com/whisper-sec/sdk-bash/releases/latest/download/install.sh | bash
```

#### Homebrew (macOS/Linux)
```bash
# Add tap (first time only)
brew tap whisper-sec/tap https://github.com/whisper-sec/homebrew-tap

# Install
brew install whisper-api-cli

# Update
brew upgrade whisper-api-cli
```

#### Manual Installation
```bash
# Download and extract
curl -fsSL https://github.com/whisper-sec/sdk-bash/releases/latest/download/whisper-api-cli-VERSION.tar.gz | tar xz

# Make executable and move to PATH
chmod +x whisper-api-cli
sudo mv whisper-api-cli /usr/local/bin/
```

#### From Source
```bash
# Clone repository
git clone https://github.com/whisper-sec/sdk-bash.git
cd sdk-bash

# Install
sudo cp whisper-api-cli /usr/local/bin/
chmod +x /usr/local/bin/whisper-api-cli
```

### 2. Get Your API Key

Sign up at [dash.whisper.security](https://dash.whisper.security) to get your API key.

### 3. Configure Authentication

```bash
# Set your API key as an environment variable
export WHISPER_API_KEY="YOUR_API_KEY"

# Or pass it with each request using the -a flag
```

### 4. Make Your First Request

```bash
# Enrich an IP address
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8

# Get geolocation data
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIpLocation ip=8.8.8.8

# Analyze a domain
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=domain value=example.com
```

---

## 🎯 Key Features

- **Unified & Simple**: Small set of powerful, resource-oriented endpoints
- **Performant by Design**: Asynchronous-first with strategic caching (<500ms typical response)
- **Workflow-Oriented**: Built for real-world security operations, not just data dumps
- **Comprehensive**: IP, Domain, DNS, WHOIS, Routing, Geolocation, Screenshots, Monitoring
- **Shell-Friendly**: Perfect for pipes, scripts, and automation
- **JSON Output**: Easy to parse with jq or other tools

---

## ⚡ Performance Targets

| Endpoint Type | Response Time | Use Case |
|---------------|---------------|----------|
| Geolocation | <150ms | Real-time fraud detection |
| Single Indicator | <500ms | Incident response enrichment |
| With Routing Data | <2s (cached: 200ms) | Deep network analysis |
| Bulk Operations | 5-30s | Batch log enrichment |
| Search/Discovery | 10-60s | Threat hunting |

---

## 📋 Usage Examples

### Basic Usage

```bash
# List all available operations
whisper-api-cli -h

# Get help for a specific operation
whisper-api-cli getIndicator -h

# Set the API base URL (optional, defaults to https://api.whisper.security)
whisper-api-cli --host https://api.whisper.security -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8
```

### IP Address Enrichment

```bash
# Basic IP lookup
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8

# Include routing data
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 include=routing,rpki

# Pretty print with jq
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 | jq '.'
```

### Domain Analysis

```bash
# Analyze a domain
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=domain value=example.com

# Include WHOIS and DNS details
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=domain value=example.com include=whois,dns_details

# Get subdomains
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getSubdomains domain=example.com
```

### Geolocation Lookups

```bash
# Single IP geolocation
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIpLocation ip=8.8.8.8

# Search by location
whisper-api-cli -a "Bearer $WHISPER_API_KEY" searchLocation field=country value="United States" limit=10
```

### Bulk Operations (Async)

```bash
# Submit a bulk job
JOB_RESPONSE=$(whisper-api-cli -a "Bearer $WHISPER_API_KEY" bulkEnrichment - <<EOF
{
  "indicators": ["8.8.8.8", "1.1.1.1", "example.com"],
  "include": ["basic", "reputation"]
}
EOF
)

# Extract job ID
JOB_ID=$(echo $JOB_RESPONSE | jq -r '.jobId')

# Check job status
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getJob jobId=$JOB_ID
```

### Screenshots

```bash
# Capture a screenshot
whisper-api-cli -a "Bearer $WHISPER_API_KEY" createScreenshot - <<EOF
{
  "url": "https://example.com",
  "options": {
    "fullPage": true,
    "format": "png"
  }
}
EOF
```

---

## 🔐 Authentication

All API endpoints require Bearer token authentication. There are three ways to provide your API key:

### 1. Environment Variable (Recommended)

```bash
export WHISPER_API_KEY="YOUR_API_KEY"
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8
```

### 2. Inline with -a Flag

```bash
whisper-api-cli -a "Bearer YOUR_API_KEY" getIndicator type=ip value=8.8.8.8
```

### 3. Configuration File

Create a `.whisper-api-config` file in your home directory:

```bash
WHISPER_API_KEY="YOUR_API_KEY"
```

Then source it:

```bash
source ~/.whisper-api-config
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8
```

---

## 🛠️ Advanced Usage

### Using with jq for JSON Processing

```bash
# Extract specific fields
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 \
  | jq '{organization: .summary.organization, risk: .reputation.riskScore}'

# Filter by risk score
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 \
  | jq 'select(.reputation.riskScore > 50)'
```

### Batch Processing with Loops

```bash
# Process multiple IPs
for ip in 8.8.8.8 1.1.1.1 208.67.222.222; do
  echo "Processing $ip..."
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=$ip \
    | jq -r ".summary.organization"
done
```

### Piping from Files

```bash
# Read IPs from a file and enrich them
cat ips.txt | while read ip; do
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=$ip >> results.json
done
```

### Custom cURL Options

```bash
# Use custom cURL options (e.g., for SSL, proxies)
whisper-api-cli -k --proxy http://proxy:8080 -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8
```

---

## 📚 CLI Commands Reference

The Bash client provides command-line access to all Whisper API endpoints. Use `whisper-api-cli -h` for a full list of commands.

### Available Operations

#### Indicators API
Comprehensive indicator enrichment for IPs and domains.

**Commands:**
- `getIndicator` - Enrich a single indicator (IP or domain)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 include=routing,rpki
  ```

- `getIndicatorHistory` - Get historical data for indicator
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicatorHistory type=domain value=example.com limit=10
  ```

- `getIndicatorGraph` - Get infrastructure relationship graph
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicatorGraph type=ip value=8.8.8.8
  ```

- `getSubdomains` - Get domain subdomains
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getSubdomains domain=example.com limit=50
  ```

- `generateSimilarDomainsGet` - Generate similar domains (GET)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" generateSimilarDomainsGet domain=example.com type=typosquatting
  ```

- `generateSimilarDomainsPost` - Generate similar domains (POST)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" generateSimilarDomainsPost domain=example.com - <<EOF
  {
    "types": ["typosquatting", "homoglyph"],
    "config": {"includeRegistered": true}
  }
  EOF
  ```

- `bulkEnrichment` - Bulk indicator enrichment (async job)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" bulkEnrichment - <<EOF
  {
    "indicators": ["8.8.8.8", "1.1.1.1", "example.com"],
    "include": ["basic", "reputation"]
  }
  EOF
  ```

- `searchIndicators` - Search indicators (async job)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" searchIndicators - <<EOF
  {
    "query": "Google LLC",
    "field": "registrantName",
    "limit": 100
  }
  EOF
  ```

[View Complete Indicators API Documentation](docs/IndicatorsApi.md)

#### Location API
Geolocation lookups and searches.

**Commands:**
- `getIpLocation` - Get IP geolocation
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIpLocation ip=8.8.8.8
  ```

- `searchLocation` - Search by location attributes
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" searchLocation field=country value="United States" limit=10
  ```

[View Complete Location API Documentation](docs/LocationApi.md)

#### Operations API
Manage asynchronous jobs and operations.

**Commands:**
- `getJob` - Get job status and results
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getJob jobId=job-uuid-here
  ```

- `listJobs` - List user jobs
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" listJobs status=completed limit=10
  ```

- `createScreenshot` - Capture website screenshot (async job)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" createScreenshot - <<EOF
  {
    "url": "https://example.com",
    "options": {
      "fullPage": true,
      "format": "png"
    }
  }
  EOF
  ```

- `createScan` - Infrastructure scan (async job)
  ```bash
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" createScan - <<EOF
  {
    "domain": "example.com",
    "type": "subdomains"
  }
  EOF
  ```

[View Complete Operations API Documentation](docs/OperationsApi.md)

---

## 📦 Response Data Models

All API responses are returned as JSON. Use `jq` or similar tools for parsing.

### Core Response Models

#### IndicatorResponse
Comprehensive intelligence for an IP or domain.

**Fields:**
- `query` - Query metadata
- `summary` - Quick summary with organization, location, and risk level
- `geolocation` - Geographic data (country, city, coordinates)
- `network` - Network information (ASN, CIDR, organization)
- `isp` - ISP details
- `registration` - WHOIS registration data
- `dns` - DNS records (A, AAAA, MX, NS, etc.)
- `relationships` - Related infrastructure (subdomains, IPs, links)
- `reputation` - Risk and reputation scores
- `security` - Security context (blacklists, threat intelligence)
- `metadata` - Response metadata (processing time, data freshness)

**Example:**
```bash
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 | jq '{
  organization: .summary.organization,
  country: .geolocation.country,
  asn: .network.asn,
  risk: .reputation.riskScore
}'
```

[View Full Model Documentation](docs/IndicatorResponse.md)

#### LocationResponse
Geolocation data for an IP address.

**Fields:**
- `ip` - IP address
- `country` - Country name
- `countryCode` - ISO country code
- `city` - City name
- `latitude` - Latitude coordinate
- `longitude` - Longitude coordinate
- `timezone` - Timezone identifier
- `isp` - ISP name
- `asn` - Autonomous System Number

**Example:**
```bash
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIpLocation ip=8.8.8.8 | jq '{
  location: "\(.city), \(.country)",
  coordinates: "\(.latitude),\(.longitude)",
  isp: .isp
}'
```

[View Full Model Documentation](docs/LocationResponse.md)

#### JobResponse
Asynchronous job status and results.

**Fields:**
- `jobId` - Unique job identifier
- `status` - Job status (pending, in_progress, completed, failed)
- `progress` - Progress information with percentage
- `result` - Job results (when completed)
- `error` - Error details (when failed)
- `createdAt` - Creation timestamp
- `updatedAt` - Last update timestamp

**Example:**
```bash
# Check job status
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getJob jobId=$JOB_ID | jq '{
  status: .status,
  progress: .progress.percentage,
  message: .progress.message
}'
```

[View Full Model Documentation](docs/JobResponse.md)

### Request Data Models

When submitting POST requests, use JSON with the following structures:

#### BulkRequest
Request for bulk indicator enrichment.

**Fields:**
- `indicators` (array) - List of IPs/domains (max 100)
- `include` (array) - Data modules to include (basic, reputation, routing, etc.)
- `options` (object) - Processing options

[View Full Model Documentation](docs/BulkRequest.md)

#### SearchRequest
Request for indicator search.

**Fields:**
- `query` (string) - Search query
- `field` (string) - Field to search (registrantName, registrarName, etc.)
- `limit` (number) - Maximum results (default: 100)

[View Full Model Documentation](docs/SearchRequest.md)

#### ScreenshotRequest
Request for website screenshot.

**Fields:**
- `url` (string) - Target URL
- `options` (object) - Capture options (fullPage, format, viewport)

[View Full Model Documentation](docs/ScreenshotRequest.md)

#### InfraScanRequest
Request for infrastructure scanning.

**Fields:**
- `domain` (string) - Target domain
- `type` (string) - Scan type (subdomains, ports, dns)
- `config` (object) - Type-specific configuration

[View Full Model Documentation](docs/InfraScanRequest.md)

### Complete Model Reference

For detailed documentation on all data models, see the [docs/](docs/) directory:

**Response Models:**
- [IndicatorResponse](docs/IndicatorResponse.md) - Main enrichment response
- [LocationResponse](docs/LocationResponse.md) - Geolocation data
- [JobResponse](docs/JobResponse.md) - Async job status
- [HistoryResponse](docs/HistoryResponse.md) - Historical data

**Request Models:**
- [BulkRequest](docs/BulkRequest.md) - Bulk enrichment parameters
- [SearchRequest](docs/SearchRequest.md) - Search parameters
- [ScreenshotRequest](docs/ScreenshotRequest.md) - Screenshot options
- [InfraScanRequest](docs/InfraScanRequest.md) - Scan configuration

---

## 🔧 Advanced CLI Usage

### Working with Async Jobs

```bash
#!/bin/bash
# Submit a bulk enrichment job and poll for completion

# Submit job
JOB_RESPONSE=$(whisper-api-cli -a "Bearer $WHISPER_API_KEY" bulkEnrichment - <<EOF
{
  "indicators": ["8.8.8.8", "1.1.1.1", "example.com"],
  "include": ["basic", "reputation"]
}
EOF
)

# Extract job ID
JOB_ID=$(echo $JOB_RESPONSE | jq -r '.jobId')
echo "Job submitted: $JOB_ID"

# Poll for completion
while true; do
  JOB_STATUS=$(whisper-api-cli -a "Bearer $WHISPER_API_KEY" getJob jobId=$JOB_ID)
  STATUS=$(echo $JOB_STATUS | jq -r '.status')
  PROGRESS=$(echo $JOB_STATUS | jq -r '.progress.percentage // 0')

  echo "Status: $STATUS, Progress: $PROGRESS%"

  if [ "$STATUS" = "completed" ] || [ "$STATUS" = "failed" ]; then
    break
  fi

  sleep 2
done

# Get results
if [ "$STATUS" = "completed" ]; then
  echo $JOB_STATUS | jq '.result'
fi
```

### Batch Processing with Loops

```bash
#!/bin/bash
# Process multiple IPs from a file

while IFS= read -r ip; do
  echo "Processing $ip..."

  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=$ip \
    | jq -r '{ip: .query.value, org: .summary.organization, risk: .reputation.riskScore}'

  sleep 0.1  # Rate limiting
done < ips.txt
```

### Extracting Specific Data with jq

```bash
# Get high-risk IPs only
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 \
  | jq 'select(.reputation.riskScore > 50)'

# Extract nameservers from domain
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=domain value=example.com \
  | jq '.dns.nameServers[]'

# Get all A records
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=domain value=example.com \
  | jq '.dns.aRecords[]'

# Extract WHOIS registrar
whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=domain value=example.com \
  | jq -r '.registration.registrar'
```

### CSV Export

```bash
# Export IP enrichment data to CSV
echo "IP,Organization,Country,ASN,Risk Score" > output.csv

cat ips.txt | while read ip; do
  whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=$ip \
    | jq -r '[
        .query.value,
        .summary.organization // "N/A",
        .geolocation.country // "N/A",
        .network.asn // "N/A",
        .reputation.riskScore // "0"
      ] | @csv' >> output.csv
done
```

### Error Handling

```bash
#!/bin/bash
# Robust error handling

RESPONSE=$(whisper-api-cli -a "Bearer $WHISPER_API_KEY" getIndicator type=ip value=8.8.8.8 2>&1)
EXIT_CODE=$?

if [ $EXIT_CODE -ne 0 ]; then
  echo "Error: Request failed"
  echo "$RESPONSE"
  exit 1
fi

# Check for API errors
if echo "$RESPONSE" | jq -e '.error' > /dev/null 2>&1; then
  echo "API Error:"
  echo "$RESPONSE" | jq '.error'
  exit 1
fi

# Success
echo "$RESPONSE" | jq '.'
```

---

## 📚 External Documentation

For detailed API documentation, visit:
- **Full Documentation**: [docs.whisper.security](https://docs.whisper.security)
- **API Reference**: [docs.whisper.security/api](https://docs.whisper.security/api)
- **Quick Start Guide**: [docs.whisper.security/quickstart](https://docs.whisper.security/quickstart)
- **CLI Command Reference**: See the [docs/](docs/) directory for complete operation documentation

---

## 📊 Rate Limits

| Category | Limit |
|----------|-------|
| Standard Enrichment | 100 req/min |
| Bulk Operations | 10 req/min |
| Search/Discovery | 5 req/min |
| Screenshots | 10 req/min |

Rate limits return HTTP 429. Retry after the time specified in the `Retry-After` header.

---

## 🐚 Shell Completion

### Bash Completion

```bash
# Load completion for current session
source whisper-api-cli.bash-completion

# Install permanently
sudo cp whisper-api-cli.bash-completion /etc/bash-completion.d/whisper-api-cli
```

### Zsh Completion

```bash
# Copy the completion file
cp _whisper-api-cli /usr/local/share/zsh/site-functions/

# Reload completions
compinit
```

---

## 🐳 Docker Usage

```bash
# Build Docker image
docker build -t whisper-api-cli .

# Run in container
docker run -it -e WHISPER_API_KEY="YOUR_API_KEY" whisper-api-cli getIndicator type=ip value=8.8.8.8

# Interactive shell
docker run -it whisper-api-cli
```

---

## 🆘 Support

- **Email**: api-support@whisper.security
- **Documentation**: https://docs.whisper.security
- **Issues**: https://github.com/whisper-sec/sdk-bash/issues

---

## 📄 Requirements

- Bash 4.0 or higher
- cURL 7.0 or higher
- jq (optional, for JSON processing)

---

*Generated with ❤️ by Whisper Security*
