# email-intel (Go)

Enterprise standard email intelligence and verification library for Go applications and CLI.

This library is primarily focused on **checking if an email is valid**, **checking if it's a temp/disposable email**, and inferring the underlying email provider (e.g. Google Workspace, Microsoft 365, Zoho) with deep DNS analysis.

## Features

- **Email Validation**: Real-time MX record resolution to check if the domain can actually receive emails.
- **Disposable Email Detection**: Checks domains against an automatically updated daily list of thousands of disposable/temp email services (like 10minutemail, GuerrillaMail).
- **Provider Inference**: Identifies enterprise security gateways and providers like Proofpoint, Mimecast, Google Workspace, Microsoft 365, Zoho, etc.
- **Domain Classification**: Intelligently classifies domains into `Education`, `Government`, `Organization`, `Public Webmail`, or `Business` based on TLDs and regex.
- **Performance**: High-speed, concurrent DNS probing utilizing Go's native `net` package.

## Installation

```bash
go get github.com/itshivams/email-intel
```

---

## Integration Guide

### Backend Integration (Go)

In your Go backend (e.g., Gin, Echo, Fiber), you can use `email-intel` to block signups from disposable emails or validate that an email's domain actually exists before saving it to your database.

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/itshivams/email-intel"
	"github.com/gin-gonic/gin"
)

type RegisterRequest struct {
	Email string `json:"email" binding:"required,email"`
}

func main() {
	router := gin.Default()

	router.POST("/register", func(c *gin.Context) {
		var req RegisterRequest
		if err := c.ShouldBindJSON(&req); err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": "Invalid email format"})
			return
		}

		// Analyze the email using email-intel
		report, err := emailintel.Analyze(req.Email)
		if err != nil {
			c.JSON(http.StatusInternalServerError, gin.H{"error": "Email validation failed"})
			return
		}

		// 1. Check if the email domain is valid (has MX records)
		if !report.Valid {
			c.JSON(http.StatusBadRequest, gin.H{"error": "Invalid email domain. Please provide a real email."})
			return
		}

		// 2. Check if the email is a disposable/temp email
		if report.Disposable {
			c.JSON(http.StatusBadRequest, gin.H{"error": "Disposable emails are not allowed."})
			return
		}

		// 3. Optional: Block free public webmails if you only want B2B users
		if report.Type == "Public Webmail" {
			c.JSON(http.StatusBadRequest, gin.H{"error": "Please use your company email address."})
			return
		}

		// Proceed with registration...
		c.JSON(http.StatusOK, gin.H{"message": "Registration successful!", "data": report})
	})

	router.Run(":8080")
}
```

---

## CLI Usage

You can also install the package globally as a CLI tool:

```bash
go install github.com/itshivams/email-intel/cmd/email-intel@latest

email-intel shivam@test.com
```

This will output a nice, formatted intelligence report in your terminal!
