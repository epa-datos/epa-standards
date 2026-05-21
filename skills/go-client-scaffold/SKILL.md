---
name: go-client-scaffold
description: Create a new Go internal client library
keywords:
  - create Go client
  - new internal client
  - Go client template
  - client library
---

# Go Client Scaffold - EPA Digital Standard

Create a new Go client library for consuming internal or external APIs.

## What This Skill Does

1. ✅ Creates simple, focused client structure
2. ✅ Generates client.go with NewClient() and retry logic
3. ✅ Adds types.go with request/response types
4. ✅ Creates example usage in examples/main.go
5. ✅ Adds tests with httptest
6. ✅ Customizes CLAUDE.md
7. ✅ Sets up GitHub workflows

## Inputs

Ask the user for:

- **Client name** (e.g., "analytics-client", "billing-client")
  - Validate: kebab-case, 2-50 characters
- **GitHub organization** (e.g., "epa-datos")
- **Service URL** (e.g., "https://analytics-api.example.com" or internal service)
  - Or: "internal" if calling another EPA service
- **Primary operations** (comma-separated, e.g., "GetEvents, TrackEvent, GetAnalytics")

## Workflow

```
1. Validate inputs
   └─ Client name format
   └─ Service URL reachable

2. Create directory structure
   ├─ client.go (main client implementation)
   ├─ types.go (request/response types)
   ├─ errors.go (custom error types)
   ├─ client_test.go (unit tests with httptest)
   ├─ examples/
   │  └─ main.go (usage example)
   ├─ go.mod
   ├─ CLAUDE.md (customized)
   ├─ .env.example
   └─ README.md

3. Generate client code
   ├─ type Client struct with:
   │  ├─ baseURL
   │  ├─ httpClient
   │  ├─ timeout
   │  └─ retry logic
   ├─ NewClient() constructor
   ├─ method for each operation
   └─ Proper error handling

4. Create example methods
   └─ One method per operation specified

5. Set up tests
   ├─ Mock HTTP server with httptest
   ├─ Test successful response
   ├─ Test error scenarios
   └─ Test retry logic

6. Initialize Git
   ├─ git init
   ├─ git add .
   ├─ git commit -m "initial: Go client template"
   └─ git branch -M main

7. Provide checklist
   └─ GitHub setup
   └─ Publishing instructions (optional)
```

## Example Outputs

```go
// client.go
package {client-name}

import "net/http"

type Client struct {
    baseURL    string
    httpClient *http.Client
    timeout    time.Duration
}

func NewClient(baseURL string) *Client {
    return &Client{
        baseURL: baseURL,
        httpClient: &http.Client{
            Timeout: 30 * time.Second,
        },
        timeout: 30 * time.Second,
    }
}

// GetEvents retrieves events from the analytics service
func (c *Client) GetEvents(ctx context.Context, userID string) (*EventsResponse, error) {
    req, err := http.NewRequestWithContext(ctx, http.MethodGet,
        c.baseURL+"/events?user_id="+userID, nil)
    if err != nil {
        return nil, err
    }

    resp, err := c.httpClient.Do(req)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    if resp.StatusCode != http.StatusOK {
        return nil, newError(resp)
    }

    var result EventsResponse
    if err := json.NewDecoder(resp.Body).Decode(&result); err != nil {
        return nil, err
    }

    return &result, nil
}
```

```go
// types.go
package {client-name}

type EventsResponse struct {
    Events []Event `json:"events"`
}

type Event struct {
    ID        string    `json:"id"`
    EventType string    `json:"event_type"`
    Timestamp time.Time `json:"timestamp"`
}
```

```go
// client_test.go
package {client-name}

func TestGetEvents(t *testing.T) {
    server := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Content-Type", "application/json")
        json.NewEncoder(w).Encode(EventsResponse{
            Events: []Event{{ID: "1"}},
        })
    }))
    defer server.Close()

    client := NewClient(server.URL)
    resp, err := client.GetEvents(context.Background(), "user1")
    
    assert.NoError(t, err)
    assert.Len(t, resp.Events, 1)
}
```

## GitHub Setup Checklist

```markdown
## ✅ GitHub Setup Checklist

- [ ] Create GitHub repo: https://github.com/{org}/{client-name}
- [ ] Push your local repo:
  \`\`\`bash
  git remote add origin https://github.com/{org}/{client-name}.git
  git push -u origin main
  \`\`\`

- [ ] Enable Branch Protection:
  Go to Settings → Branches → Add rule
  - Branch name pattern: \`main\`
  - [x] Require pull request reviews (1 approval)
  - [x] Require status checks to pass

- [ ] (Optional) Tag for releases:
  \`\`\`bash
  git tag v1.0.0
  git push origin v1.0.0
  \`\`\`

- [ ] (Optional) Publish to pkg.go.dev (automatic once public)
```

## Success Output

After completion:

```
{client-name}/
├── client.go
├── types.go
├── errors.go
├── client_test.go
├── examples/
│   └── main.go
├── go.mod
├── go.sum
├── CLAUDE.md (customized)
├── README.md
├── .env.example
├── .gitignore
└── .git (initialized)
```

## Next Steps

1. Push to GitHub (use checklist)
2. Run tests: `go test -v ./...`
3. Review examples/main.go to understand usage
4. Integrate into other services
5. Tag releases for versioning

---

For detailed info, see CLAUDE.md in the generated project.
