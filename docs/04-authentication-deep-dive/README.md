# Module 5: Authentication Deep Dive

## What I did

This module moved past identity storage and into how authentication actually happens: Kerberos ticket-based auth, fine-grained password policies for privileged accounts, and RADIUS authentication via NPS — the last of which turned into the longest real troubleshooting chain of the lab so far, going from a completely silent failure to a specific, named root cause.

## Steps

### Authentication concepts and Kerberos in action

Covered how Windows domain authentication actually works under the hood — ticket-based auth via Kerberos rather than sending credentials on every request. Watched Kerberos tickets get issued in real time on a domain-joined machine (TGT on logon, service tickets issued as needed), which makes the concept concrete instead of just theoretical.

### Fine-Grained Password Policy (FGPP)

Built a Password Settings Object (PSO) to apply a stricter password policy to a specific group instead of relying on the single domain-wide default. This is the realistic enterprise pattern: privileged accounts (like IT admins) should have tighter password requirements than a general end-user account, and a single blanket policy for the whole domain can't express that — FGPP/PSOs are what let you actually differentiate.

### RADIUS Authentication using NPS

This lab was the real test of the module. Set up FreeRADIUS client tools on Kali and used it to send test authentication requests against NPS running on the domain controller. Nothing worked on the first several attempts, and diagnosing why turned into a proper troubleshooting exercise.

**Wrong target, wrong tools.** First attempt pointed `radtest` at Kali's own IP address instead of the Server. Got "No reply from server" — which, it turns out, means something very specific in RADIUS: not a rejection, just literally nobody listening. Only `freeradius-utils` (client tools) had been installed on Kali, not an actual RADIUS server, so testing against itself could never have worked regardless of the target IP being wrong too.

**A shell scripting trap.** The test command included a password starting with `$$`, typed unquoted in bash. `$$` is a special shell variable that expands to the current process ID — so the password being sent wasn't the intended one at all, silently swapped for something like `3344Bamikole10`. Fixed by wrapping it in single quotes, which stops bash from touching it.

**Still no reply, even at the right target.** Pointed the test at the Server's actual IP and still got silence. Root cause: Kali had never been registered as a RADIUS Client in NPS. NPS doesn't send a rejection for an unregistered client — it just silently drops the packet, which produces the exact same symptom as a network-level connectivity problem, making it easy to misdiagnose as a firewall issue instead of a missing configuration step.

**From silence to a real rejection.** After registering Kali as a RADIUS client with a matching shared secret, the response changed from no reply to an actual `Access-Reject` — real progress, since it confirmed the client was now recognized and NPS was actively evaluating the request rather than ignoring it outright.

**Finding the actual reason.** The reject itself didn't explain why. Went into the Security event log on the domain controller, found the NPS audit failure event (Event ID 6273), and pulled the specific Reason Code from the XML details: **Reason Code 66 — "the user attempted to use an authentication method that is not enabled on the matching network policy."** `radtest` sends PAP by default, and the Network Policy didn't have PAP enabled as an allowed method. Enabled it under the policy's Constraints → Authentication Methods, and the next test came back Access-Accept.

---

## Skills demonstrated

- Kerberos ticket-based authentication concepts in a live domain environment
- Fine-Grained Password Policies (PSOs) for differentiated password requirements by account type
- End-to-end RADIUS/NPS setup and troubleshooting, client through server
- Cross-platform authentication testing (Linux client authenticating against a Windows RADIUS server)
- Diagnosing "silent failure" conditions (unregistered RADIUS client) versus explicit rejections
- Reading Windows Security Event Log audit failures (Event ID 6273) to extract precise Reason Codes rather than guessing at causes
- Debugging shell variable expansion pitfalls affecting credential testing
- Understanding authentication protocol security tradeoffs (PAP's cleartext exposure vs. stronger methods)
