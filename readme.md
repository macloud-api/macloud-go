# Macloud Go SDK

Official Go SDK for Macloud SCDN API.

**Website:** https://homeconsole.macloud.pro

## Features

- RESTful API support
- JSON request/response format
- SHA256 signature algorithm for secure requests
- Support for GET, POST, PUT, PATCH, DELETE methods
- Automatic request signing and authentication

## Signature Algorithm

Each request is signed to ensure data integrity during transmission:

- **Client**: Uses SHA256 signature algorithm. Parameters are base64 encoded and signed with `app_secret` using SHA256. The signature is included in each request.
- **Server**: Validates the signature using the same algorithm to verify request authenticity.

## Requirements

- Go >= 1.18

## Installation

```bash
go get github.com/macloud-api/macloud-go
```

## Configuration

The SDK requires the following parameters:

- **AppId**: Your assigned application ID
- **AppSecret**: Your assigned application secret, used for signing data
- **ApiPre**: API endpoint prefix (contact support for details)

## Usage

### Initialize SDK

```go
package main

import (
	"os"
	sdk "github.com/macloud-api/macloud-go"
)

func main() {
	app_id := os.Getenv("SDK_APP_ID")
	app_secret := os.Getenv("SDK_APP_SECERT")
	api_pre := os.Getenv("SDK_API_PRE")

	sdkObj := sdk.Sdk{
		AppId:     app_id,
		AppSecret: app_secret,
		ApiPre:    api_pre,
		Timeout:   30,
	}
	
	// Use sdkObj for API calls
}
```

### GET Request Example

```go
api := "test.sdk.get"
reqParams := sdk.ReqParams{
	Query: map[string]interface{}{
		"page":     1,
		"pagesize": 10,
		"data": map[string]interface{}{
			"name":   "name",
			"domain": "baidu.com",
		},
	},
}

resp, err := sdkObj.Get(api, reqParams)
if err != nil {
	fmt.Println("Request error:", err)
	return
}

if resp.BizCode == 1 {
	fmt.Println("Request successful")
	fmt.Println("HTTP Code:", resp.HttpCode)
	fmt.Println("Body:", resp.RespBody)
	fmt.Println("Biz Code:", resp.BizCode)
	fmt.Println("Biz Msg:", resp.BizMsg)
	fmt.Println("Biz Data:", resp.BizData)
} else {
	fmt.Println("Business error:", resp.BizMsg)
}
```

### POST Request Example

```go
api := "test.sdk.post"
reqParams := sdk.ReqParams{
	Data: map[string]interface{}{
		"name": 1,
		"age":  10,
		"data": map[string]interface{}{
			"name":   "name",
			"domain": "baidu.com",
		},
	},
}

resp, err := sdkObj.Post(api, reqParams)
if err != nil {
	fmt.Println("Request error:", err)
	return
}

if resp.BizCode == 1 {
	fmt.Println("Request successful")
	fmt.Println("Biz Data:", resp.BizData)
} else {
	fmt.Println("Business error:", resp.BizMsg)
}
```

### PUT Request Example

```go
api := "test.sdk.put"
reqParams := sdk.ReqParams{
	Data: map[string]interface{}{
		"name": 1,
		"age":  10,
		"data": map[string]interface{}{
			"name":   "name",
			"domain": "baidu.com",
		},
	},
}

resp, err := sdkObj.Put(api, reqParams)
if err != nil {
	fmt.Println("Request error:", err)
	return
}

if resp.BizCode == 1 {
	fmt.Println("Request successful")
	fmt.Println("Biz Data:", resp.BizData)
} else {
	fmt.Println("Business error:", resp.BizMsg)
}
```

### DELETE Request Example

```go
api := "test.sdk.delete"
reqParams := sdk.ReqParams{
	Data: map[string]interface{}{
		"id": 10,
	},
}

resp, err := sdkObj.Delete(api, reqParams)
if err != nil {
	fmt.Println("Request error:", err)
	return
}

if resp.BizCode == 1 {
	fmt.Println("Request successful")
	fmt.Println("Biz Data:", resp.BizData)
} else {
	fmt.Println("Business error:", resp.BizMsg)
}
```

### Request Parameters

The `ReqParams` struct has three optional properties:

- **Query**: GET request parameters (`map[string]interface{}`)
- **Data**: Non-GET request body parameters (`map[string]interface{}`)
- **Headers**: Custom request headers (`map[string]string`)

### Response Structure

The `Response` struct contains:

- **HttpCode**: HTTP status code (200 for success)
- **RespBody**: Raw response body string
- **BizCode**: Business status code (1 = success, non-1 = failure)
- **BizMsg**: Business status message
- **BizData**: Business data (only available when BizCode is 1)

### Important Notes

1. For all requests, URI and GET parameters are separated. For example, for `https://apiv4.local.com/V4/version?v=1`, the `v=1` parameter must be passed through `ReqParams.Query`.

2. If an exception occurs during execution, it will be thrown directly.

3. You can enable debug mode by setting the `Debug` field to `true`.

## License

See LICENSE file for details.
