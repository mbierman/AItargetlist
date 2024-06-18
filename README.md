You can use this list with Firealla as a target list to block (or allow) AI tools on your network. You can: 
1. import manually through the [free web portal](https://my.firewalla.com) the [paid MSP](https://firewalla.net)
2. use the [MSP target API](https://kaleb.firewalla.net/api/docs/api-reference/target-lists/) to import and update

# Create Target List 
`POST https://msp_domain/v2/target-lists`

Note you have to have a target included when you create the target list. 
```
curl --location 'https://[mspname].firewalla.net/v2/target-lists' \
--header 'Content-Type: application/json' \
--header 'Authorization: Token [MSP token]' \
--data '{
  "name": "AI target list",
  "targets": "test.com",
  "owner": "global",
  "category": "private",
  "notes": "Block for AI"
}'
```

# Fetch the target list and format it as a JSON array
targets=$(curl -s https://raw.githubusercontent.com/mbierman/AItargetlist/main/ai_short | jq -R -s -c 'split("\n") | map(select(length > 0))')

# Update target List
```
curl --location --request PATCH 'https://[mspname].firewalla.net/v2/target-lists/[targetlistid]' \
--header 'Content-Type: application/json' \
--header 'Authorization: Token 332232e9058b78a45fd4c74e12af8be0' \
--data '{
    "targets": '"$targets"',
}'
```
