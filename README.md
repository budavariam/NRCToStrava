# NRCToStrava

Script to export/convert run activities from Nike Run Club

## Getting started

- Log in [here](https://unite.nike.com/s3/unite/mobile.html?androidSDKVersion=3.1.0&corsoverride=https://unite.nike.com&uxid=com.nike.sport.running.droid.3.8&locale=en_US&backendEnvironment=identity&view=login&clientId=WLr1eIG5JSNNcBJM3npVa6L76MK8OBTt&facebookAppId=84697719333&wechatAppId=wxde7d0246cfaf32f7#%7B%22event%22:%22gigyaKey%22,%22apiKey%22:%22gigyaKeyNotSupported%22,%22ts%22:1607938711083%7D)
- Use this snippet: `copy(JSON.parse(localStorage["com.nike.commerce.nikedotcom.web.credential"]).access_token)`
- Save it to `.token`
- Run script

  ```bash
  bash NRCToStrava.bash $(cat .token)
  ```

- Pretty print xml: `xmllint --format TCX_exports/NikePlus_*.tcx | bat`

## Requirements

- **Python 3**: All requirements are set in [requirements.txt](https://github.com/opierre/NRCToStrava/tree/master/requirements.txt)

- **Command-line JSON processor**: download and install [jq](https://github.com/stedolan/jq/releases) (_version >= 1.6_)

- **Strava**:
  - In order to upload NRC running data to Strava, it is required to make an application as explained
    [here](https://developers.strava.com/docs/getting-started/#account) and fulfill all necessary fields.

## Prerequisites

### NikeRunClub

All Nike+ API is explained in
[Yoshimasa Niwa's gist](https://gist.github.com/niw/858c1ecaef89858893681e46db63db66). Fetching NRC activities requires
a bearer token from their website:

- Go to [Nike Membership page](https://www.nike.com/us/en_us/e/nike-plus-membership) and login
- Open the developer tools in your browser and filter all "_api.nike.com_" requests
- You should be able to find such request: "_/engage/invites/v1..._", then click on it
- In Request Headers there should be an _Authorization_ header with a _Bearer_ value
- Copy this value somewhere, it will be asked during **NRCToStrava.bash** execution

### Strava

Uploading a run to Strava requires your Client ID, Client Secret and an access token with write permission:

- Client Secret and Client ID can be found in _My API Application>Client Secret/Client ID_ and should be written in
  [.stravaupload.cfg](https://github.com/opierre/NRCToStrava/tree/master/scripts/.stravaupload.cfg) next to
  _clientsecret_ and _clientid_ keys.

- In order to generate access token:
  * Get your Client ID at: *My API Application>Client ID\*
  * Open a webbrowser at (*replace [CLIENT_ID] with your client ID*):
  <http://www.strava.com/oauth/authorize?client_id=[CLIENT_ID]&response_type=code&redirect_uri=http://localhost/exchange_token&approval_prompt=force&scope=profile:write,activity:write>
  * Login, tick each required authorization and click _Authorize_
  * On next page you will get an error but in the URL there is a code:
  *exchange_token?state=&code=**e2989f17559e073d84ec532db782121519f4f03d**&scope=read,activity:write,profile:write\*
  * Copy that code in
  [.stravaupload.cfg](https://github.com/opierre/NRCToStrava/tree/master/scripts/.stravaupload.cfg) next to
  *token\* key.

### Run bash script

Run script:

```bash
$ sh NRCToStrava.bash <bearer_token>
```

Paste NRC bearer value and press enter.

## Compatibility

This script has been tested on runs from 2014 to 2019.

## Notes

You can "_Edit distance_" on your run in Strava if the difference between NRC and Strava data is too big.

## Credits

- Started out from [opierre](https://github.com/opierre/)'s [NRCToStrava](https://github.com/opierre/NRCToStrava.git)
- _NRCToStrava.bash_ is adapted from [Yoshimasa Niwa's gist](https://gist.github.com/niw/858c1ecaef89858893681e46db63db66)
  in order to convert JSON data to TCX and upload to Strava
- _stravaupload.py_ is adapted from [Eirik Marthinsen github](https://github.com/marthinsen/stravaupload/blob/master/stravaupload.py)
  in order to work with Python 3 and current Strava API
