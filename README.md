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
Find the target ID for the next step:

`GET https://msp_domain/v2/target-lists`

```
curl --location 'https://[mspname].firewalla.net/v2/target-lists' \
--header 'Authorization: Token [MSP token]'
```

Sample result
```
...
{
        "id": "TL-1228fab7-cd1d-4a99-bb48-frr79437e075",
        "name": "AI target list",
        "owner": "global",
        "targets": [
            "foo1.com"
        ],
        "category": "private",
        "notes": "Block for AI",
        "lastUpdated": 1718740702.459
    }
...
```

# Updating the target list

```
#!/bin/bash

# Fetch the target list and format it as a JSON array
# For the "short" list
targets=$(curl -s https://raw.githubusercontent.com/mbierman/AItargetlist/main/ai_short | jq -R -s -c 'split("\n") | map(select(length > 0))')
# for the "loing" list
targets=$(curl -s https://github.com/mbierman/AItargetlist/blob/main/ai_full.txt](https://raw.githubusercontent.com/mbierman/AItargetlist/main/ai_full.txt | jq -R -s -c 'split("\n") | map(select(length > 0))')


replace the [targetlistid] with the target list found above.
# Update target List
curl --location --request PATCH 'https://[mspname].firewalla.net/v2/target-lists/[targetlistid]' \
--header 'Content-Type: application/json' \
--header 'Authorization: Token [MSP token]' \
--data '{
    "targets": '"$targets"',
}'
```
