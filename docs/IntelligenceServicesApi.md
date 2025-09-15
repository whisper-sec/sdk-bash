# IntelligenceServicesApi

All URIs are relative to **

Method | HTTP request | Description
------------- | ------------- | -------------
[**getDomainIntelligence**](IntelligenceServicesApi.md#getDomainIntelligence) | **GET** /intelligence/v1/domain/{domain} | Get comprehensive domain intelligence with streaming support
[**getIpIntelligence**](IntelligenceServicesApi.md#getIpIntelligence) | **GET** /intelligence/v1/ip/{address} | Get comprehensive IP address intelligence with streaming support



## getDomainIntelligence

Get comprehensive domain intelligence with streaming support

Analyzes a domain name and returns comprehensive intelligence data from multiple sources.

**🚀 Response Modes:**
- **Batch Mode** (Default): Complete JSON response in 2-5 seconds
- **Streaming Mode**: Server-Sent Events with 200ms time-to-first-byte
  - Accept: 'application/json' for batch mode
  - Accept: 'text/event-stream' for streaming mode

**Key Features:**
- WHOIS registration data with ownership history
- Complete DNS record enumeration (A, AAAA, MX, NS, TXT, CNAME, SOA)
- Subdomain discovery and enumeration
- Link analysis showing connected domains
- IP intelligence for all resolved addresses
- Trademark and brand protection status
- Infrastructure relationships and shared hosting

**Streaming Example:**
'''bash
curl -N -H 'Accept: text/event-stream' \\
     -H 'Authorization: Bearer {token}' \\
     https://api.example.com/intelligence/v1/domain/example.com
'''

**Streaming Events:**
- 'whois': Domain registration info
- 'dns': DNS records
- 'subdomains': Discovered subdomains
- 'ip_intelligence': Intelligence for each resolved IP
- 'complete': Final aggregated data

**Input Processing:**
- Automatically strips protocols (http://, https://)
- Removes www prefix if present
- Validates domain format before processing

**Note:** Swagger UI only displays batch mode. Use curl or EventSource for streaming.

### Example

```bash
 getDomainIntelligence domain=value
```

### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain** | **string** | Domain name to analyze. Can be a root domain or subdomain. Protocol and www prefix are automatically removed. | [default to null]

### Return type

[**DomainIntelligenceResponse**](DomainIntelligenceResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: Not Applicable
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)


## getIpIntelligence

Get comprehensive IP address intelligence with streaming support

Analyzes an IP address and returns comprehensive intelligence data aggregated from multiple sources.

**🚀 Response Modes:**
- **Batch Mode** (Default): Complete JSON response in 2-3 seconds
- **Streaming Mode**: Server-Sent Events with 150ms time-to-first-byte
  - Accept: 'application/json' for batch mode
  - Accept: 'text/event-stream' for streaming mode

**Key Features:**
- Geolocation with city-level precision and confidence scores
- Network topology including ASN, BGP prefixes, and routing visibility
- ISP and organization identification
- DNS relationships showing associated domains
- Risk scoring based on threat intelligence feeds
- RPKI validation and routing security assessment
- Historical routing data and stability metrics

**Streaming Example:**
'''bash
curl -N -H 'Accept: text/event-stream' \\
     -H 'Authorization: Bearer {token}' \\
     https://api.example.com/intelligence/v1/ip/8.8.8.8
'''

**Note:** Swagger UI only displays batch mode. Use curl or EventSource for streaming.

### Example

```bash
 getIpIntelligence address=value
```

### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **address** | **string** | IPv4 or IPv6 address to analyze. Supports standard notation (e.g., 192.168.1.1 or 2001:db8::1) | [default to null]

### Return type

[**IpIntelligenceResponse**](IpIntelligenceResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: Not Applicable
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

