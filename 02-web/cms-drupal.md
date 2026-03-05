## Installation

```bash
# Install droopescan (Drupal/WordPress/etc. scanner)
sudo pip3 install droopescan
```
## Hidden Page Enumeration

Drupal stores content at `/node/<NUMBER>`. Fuzzing this range can reveal hidden or unlisted pages (e.g. dev, staging, test) not indexed by search engines.

```bash
# Fuzz nodes 1–500, hide 404s
wfuzz -c -z range,1-500 --hc 404 <URL>/node/FUZZ
```
## Remote Code Execution (RCE)

### Prerequisites

Check if the PHP Filter module is installed by visiting `/modules/php`:

| Response | Meaning                        |
| -------- | ------------------------------ |
| `403`    | Module exists (proceed)        |
| `404`    | Module not installed (blocked) |
### Steps

```
1. Navigate to: Modules → Check "PHP Filter" → Save configuration

2. Go to: Content → Add Content → Basic Page (or Article)

3. In the Body field, paste a PHP web shell:
   https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php

4. Set "Text format" to: PHP code

5. Click Preview to trigger execution
```
## Username Enumeration

Drupal leaks user existence through several unauthenticated endpoints.

### Registration Page (`/user/register`)

Attempt to register an existing username — Drupal will respond with a distinct error:

```
The name admin is already taken.
```

### Password Reset Page (`/user/password`)

Request a password reset and observe the response:

| Input              | Response                                                                 |
|--------------------|-------------------------------------------------------------------------|
| Valid username     | `Unable to send e-mail. Contact the site administrator if the problem persists.` |
| Invalid username   | `Sorry, <username> is not recognized as a user name or an e-mail address.` |
### User ID Enumeration (`/user/<number>`)

Iterate over user IDs to confirm existence based on HTTP response:

```
/user/1  → 403 Access Denied   (user exists)
/user/2  → 404 Page Not Found  (user does not exist)
```

```bash
# Automate with wfuzz
wfuzz -c -z range,1-50 --hc 404 <URL>/user/FUZZ
```