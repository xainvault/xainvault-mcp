# Troubleshooting

Everything on this page comes from the Xainvault documentation:
[Connect through MCP](https://xainvault.com/docs/connect-agents) and
[Access keys](https://xainvault.com/docs/access-keys). If your problem is not
listed, see [Feedback & support](https://xainvault.com/docs/feedback-and-support).

## The connection fails

Check these in order:

1. The URL is exactly `https://xainvault.com/mcp`, with no extra path and no
   trailing slash.
2. Direct clients send the key in the `X-API-Key` header, including the full
   `xv_key_` prefix.
3. The key is active and has not expired or been replaced.
4. The client was restarted, or its MCP server list was refreshed, after
   saving the configuration.
5. For hosted Team or Enterprise accounts, an organization Owner has allowed
   the custom connector.

## "Connection request unavailable" (ChatGPT or Claude)

Return to ChatGPT or Claude and choose **Connect** again. Connection requests
expire after a short time, and reopening the same consent URL does not create a
new one. **Open Connections** takes you to Xainvault's setup guidance.

For a temporary loading problem, choose **Try again**. If a decision fails, your
vault selection stays available so you can retry your choice.

## Claude is connected but no Xainvault tools appear

Enable Xainvault for that conversation: open **+ → Connectors** and turn it on.

## Expected skills are missing

Agents see granted **Ready** versions by default. Versions under review are not
available for installation, and your role, group memberships and per-skill
grants decide what each agent can list and pull. A hosted connector only sees
the one vault chosen during consent; each connected account app can be pinned
to a specific vault from **Connections** in Xainvault.

## An install or update link stopped working

Signed download links are valid for 15 minutes. If a version is pulled from the
vault after the link was created, the download stops working immediately.
Create a new install session to pick up the current state.

A Draft test link is bound to the exact Draft snapshot. Changing or deleting the
Draft, submitting it for review, or releasing it as Ready makes the link return
`410 Gone`. Create a new session after every Draft change.

## Rotating or replacing an access key

Rotate a key in place from **Connections**. The new value is issued immediately
and the old value keeps working for 24 hours while you update your client
configuration.

If a key leaks, rotate it or delete it immediately.
