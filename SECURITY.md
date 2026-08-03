# Security

Do not commit completed WireGuard profiles or proxy subscriptions to this repository.

Treat these values as secrets:

- WireGuard private keys
- WireGuard preshared keys
- Proxy subscription URLs and tokens
- VPN account credentials
- Personal dedicated IP details when they identify an account

If a real key is committed, revoke or delete the corresponding WireGuard peer at the provider before removing the Git history. Removing the text from the latest commit does not invalidate an exposed key.
