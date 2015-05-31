Using the Flood API and some simple bash scripting techniques it's possible to automate load testing from within your Continuous Integration pipeline. Following is a structured example or see [here](https://gist.github.com/90kts/0d3f655a89b34000d73d) for a more detailed approach:

```
#!/bin/bash
set -e

# Check we have the jq binary to make parsing JSON responses a bit easier
command -v jq >/dev/null 2>&1 || \
{ echo >&2 "Please install http://stedolan.github.io/jq/download/  Aborting."; exit 1; }

# Start a flood
echo
echo "[$(date +%FT%T)+00:00] Starting flood"
flood_uuid=$(curl -u abc123: -X POST https://api.flood.io/floods \
 -F "flood[tool]=jmeter-2.13" \
 -F "flood[threads]=10" \
 -F "flood[privacy]=public" \
 -F "flood[name]=MyTest" \
 -F "flood[tag_list]=ci,shakeout" \
 -F "flood[meta]=$meta" \
 -F "flood_files[]=@jmeter-with-plugins.jmx" \
 -F "flood[grids][][infrastructure]=demand" \
 -F "flood[grids][][instance_quantity]=1" \
 -F "flood[grids][][region]=ap-southeast-2" \
 -F "flood[grids][][instance_type]=m3.2xlarge" \
 -F "flood[grids][][stop_after]=60" | jq -r ".uuid")

# Wait for flood to finish
echo "[$(date +%FT%T)+00:00] Waiting for flood $flood_uuid"
while [ $(curl --silent --user $FLOOD_API_TOKEN: https://api.flood.io/floods/$flood_uuid | \
  jq -r '.status == "finished"') = "false" ]; do
  echo -n "."
  sleep 3
done

# Get the summary report
flood_report=$(curl --silent --user $FLOOD_API_TOKEN: https://api.flood.io/floods/$flood_uuid/report | \
  jq -r ".summary")

echo
echo "[$(date +%FT%T)+00:00] Detailed results at https://flood.io/$flood_uuid"
echo "$flood_report"

# Optionally store the CSV results
echo
echo "[$(date +%FT%T)+00:00] Storing CSV results at resuts.csv"
curl --silent --user $FLOOD_API_TOKEN: https://api.flood.io/floods/$flood_uuid/result.csv > result.csv
```