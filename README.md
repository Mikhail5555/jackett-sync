# Jackett-Sync

A quick & dirty extensible Jackett-to-Sonarr/Radarr/Lidarr/Whatever indexer synchronizer written in Node.js. PRs are welcome!

## Usage
Services declare required and optional parameters in the `services.js` file, you can supply them on the command line with `--<param name> <value>`. If the correct required params are detected, the line "Found config for \<service name>" will appear in the console. You can view all options with `--help`

# Example
Example in a docker enviroment (jackett-sync running on host), badasstorrents was down at the time
```
jackett-sync --url http://127.0.0.1:9117/jackett --apikey sen08b96xf8x1o3twv9n1aml8da8p7mp --alturl http://jackett:9117/jackett --sonarrurl http://127.0.0.1:8989/sonarr --sonarrkey b27b78ff3e994da1b4460f40e576a029 --sonarrcats 5000,5030,5040

Found config for jackett
Found config for sonarr
Adding rarbg, badasstorrents, kickasstorrent, torrentparadise
Added kickasstorrent successfully
Failed to add badasstorrents
Added torrentparadise successfully
Added rarbg successfully
Done
```
One of the created entries:
![RARBG](./images/rarbg.png)

## Extend the capabilities

### To add a new "indexer source" (e.g. Jackett, NZBHydra)
NOTE: this is not supported yet in the main app yet, but possible in the future.

Edit `services.js`, add a new service with the `get` method, and set the `source` property, and as long as you return an array with indexer objects following the schema described at the top (in the `schema` const variable), it should work!

### To add a new "indexer consumer" (e.g Sonarr, Radarr, Lidarr, LazyLibrarian)
Edit `services.js`, add a new service with the `get` (same as with indexer sources), `add` (consume a singular indexer object and add to the upstream service) and `shouldAdd` (use this to filter out indexers that don't provide the correct categories) methods.
Error handling is very rudimentary to say the least, but its good enough for the time being.