# mock-miner

After miner initializes for 1st time, it switches to Wi-Fi "access point" mode, so we shall connect to it via Wi-Fi to continue setup.

**Step #1** - scan Wi-Fi sources around, find one starting with "Chirp-" prefix, with some number following, e.g. `Chirp-123456789`.

There'll be some secret predefined password to connect to its Wi-Fi. For testing without real miner it's ok to connect to anything, e.g. your own
Wi-Fi router.

From now you'll be able to use several endpoints provided by miner itself to continue with setup. They'll be available with some local IP, like
`http://192.168.0.1:8080/api/...` but again for testing you'll be using `https://chirpwireless.github.io/mock-miner/api/...` instead.

**Step #2** - use `/api/status` endpoint to get various information about miner, let's look at example:

    {
       "nodeid":"7bbabd62-ed55-48ac-a179-a502448af10b",
       "model":"mm-2band-v1",
       "wifi":{
          "state":"down",
          "mac":"9C:65:F9:43:3B:76",
          "networks":[
             "MGTS_DEGU",
             "RT-WiFi-21",
             "TP-Link_33BD_EXT",
             "netis_186778"
          ],
          "SSID":"none",
          "proto":"none",
          "ip":"none",
          "mask":"none",
          "gw":"none",
          "dns":"none"
       },
       "lan":{
          "state":"up",
          "mac":"9C:65:F9:43:35:9A  \n",
          "proto":"dhcp",
          "ip":"10.10.0.28",
          "mask":"255.255.255.0",
          "gw":"10.10.0.254",
          "dns":[
             "172.31.0.2"
          ]
       }
    }

There could be more fields, but let's concentrate on several you need.

**Step #3** - to assign this miner to current user, you will use fields `model` and `lan/mac`. This step could be done immediately after receiving status. For this do the following:

- form the identification string by concatenating three parts, with spaces: value "1", model from the status, mac from the status (stripping any non-digits); e.g. `1 mm-2band-v1 9C65F943359A`
- encode this identification string into `hex`, e.g. `31206d6d2d3262616e642d763120394336354639343333353941` - this is the activation "key"
- make request to our server backend's "activate" endpoint, e.g. `GET {server-address}/api/nodes/activate/{key}

Use the substitute test host mentioned above and get the status here
[https://chirpwireless.github.io/mock-miner/api/status](https://chirpwireless.github.io/mock-miner/api/status)
it has corresponding node in the database, unassigned to any user. If you assign it and want to delete it from user, you'll find `DELETE /nodes/{id}` endpoint for this in server's swagger.

**To be continued**
