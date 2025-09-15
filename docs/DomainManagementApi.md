# DomainManagementApi

All URIs are relative to **

Method | HTTP request | Description
------------- | ------------- | -------------
[**findSubdomains**](DomainManagementApi.md#findSubdomains) | **GET** /domainer/api/domains/subdomains/{baseDomain} | Find subdomains



## findSubdomains

Find subdomains

Finds domains that are subdomains of the given base domain (e.g., finding 'www.example.com' for base 'example.com'). Allows filtering by absolute domain level (dot count). NOTE: This differs from relative depth filtering.

### Example

```bash
 findSubdomains baseDomain=value  limit=value  level=value
```

### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **baseDomain** | **string** | Base domain name (e.g., example.com) | [default to null]
 **limit** | **integer** | Maximum number of results | [optional] [default to 100]
 **level** | **string** | Level of subdomains to find relative to the base domain (ALL, IMMEDIATE=one level deeper, MAX_DEPTH=deepest found relative to base). | [optional] [default to null]

### Return type

[**DomainerStringListResponse**](DomainerStringListResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: Not Applicable
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

