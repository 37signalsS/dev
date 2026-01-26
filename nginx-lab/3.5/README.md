# Task 3.5: Protection Against Basic Threats

This directory contains Nginx configuration for implementing basic security measures.

## Files
- `security.conf` – Nginx configuration snippet implementing all security requirements for task 3.5

## Requirements Implemented

### 1. Rate Limiting for /login
- Configured rate limiting zone `login_limit`
- Maximum 10 requests per minute per IP address
- Protects against brute force attacks

### 2. Security Headers
- `X-Frame-Options: SAMEORIGIN` - Prevents clickjacking attacks
- `X-Content-Type-Options: nosniff` - Prevents MIME type sniffing

### 3. Hidden Files Protection
- Blocks access to files starting with `.` (dotfiles)
- Protects sensitive files like `.git`, `.env`, `.htaccess`

### 4. Server Version Hiding
- `server_tokens off` - Hides Nginx version in error pages and headers

## Usage

Add the configuration to your Nginx setup:

```bash
# Add to main nginx.conf or include in server block
include /home/q/dev/nginx-lab/3.5/security.conf;
```

Then reload Nginx:

```bash
sudo nginx -t && sudo systemctl reload nginx
```

## Testing

Test rate limiting:

```bash
# Make multiple requests to /login
for i in {1..15}; do
  curl -I http://site1.local/login
  sleep 0.5
done
```

Test hidden file blocking:

```bash
# Should return 403 Forbidden
curl -I http://site1.local/.git/config
curl -I http://site1.local/.env
```

Test security headers:

```bash
curl -I http://site1.local/
```

Expected response headers should include:
- `X-Frame-Options: SAMEORIGIN`
- `X-Content-Type-Options: nosniff`
- `Server: nginx` (version hidden)