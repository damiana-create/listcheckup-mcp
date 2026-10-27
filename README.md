# ListCheckup MCP server

Email verification for AI assistants. Connect Claude, Cursor or any MCP client to [ListCheckup](https://listcheckup.com/?utm_source=github&utm_medium=readme&utm_campaign=mcp) and let it check a single address or a whole list, showing you the price before anything is spent.

- **Endpoint:** `https://listcheckup.com/mcp` (remote, Streamable HTTP)
- **Auth:** `Authorization: Bearer lck_live_...` (create a key in your ListCheckup dashboard)
- **Free tier:** 100 checks every month, no card. Catch-all and unknown results are not charged, and credits never expire.
- **Data:** processed on EU servers. Nothing is sent to the addresses being checked.

## Tools

| Tool | What it does | Cost |
|---|---|---|
| `check_email` | Verify one address: valid, invalid, risky, catch-all or unknown, with the reason and a confidence score | 1 credit (free if catch-all or unknown) |
| `get_balance` | Credits left on the account | Free |
| `create_list` | Submit a list and get its price | Free |
| `start_list` | Start checking a list you've priced | Spends credits |
| `get_list` | Status and progress of a list | Free |
| `get_list_summary` | Counts by status once a list is finished | Free |

Pricing and starting are separate tools on purpose: an assistant has to show you what a list will cost before anything is spent, and you can say no. No tool takes an account as an argument; the key in the header decides whose credits are used.

## Connect

**Claude Code**

```bash
claude mcp add --transport http listcheckup https://listcheckup.com/mcp \
  --header "Authorization: Bearer lck_live_..."
```

**Claude Desktop, Cursor and most other clients**

```json
{
  "mcpServers": {
    "listcheckup": {
      "url": "https://listcheckup.com/mcp",
      "headers": { "Authorization": "Bearer lck_live_..." }
    }
  }
}
```

**Check it works**

```bash
curl -X POST "https://listcheckup.com/mcp" \
  -H "Authorization: Bearer lck_live_..." \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

Six tools listed means you're connected.

## Example prompts

- "Check whether anna@example.com is a real address."
- "Here's my leads CSV. How much would it cost to verify it? Don't start until I say so."
- "How many credits do I have left?"

## Treat the key as a password

Anything holding it can spend your credits. Keep config files with the key out of shared repositories, give each machine its own key, and revoke a key in the dashboard if it leaks. Revoking takes effect immediately.

## Links

- REST API docs: https://listcheckup.com/api
- Verify emails in n8n: https://listcheckup.com/n8n-email-verification/
- Free email domain checker (MX, SPF, DMARC): https://listcheckup.com/email-domain-checker/

ListCheckup checks addresses you already hold. Purchased, rented or scraped lists aren't allowed under the [acceptable use policy](https://listcheckup.com/acceptable-use).
