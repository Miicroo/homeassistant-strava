# homeassistant-strava
This is a Strava component for HomeAssistant. It uses stravalib to generate statistics as sensors. The sensors are checked every other minute.

## How to setup

### Setting up a Strava app
1. Log in to https://developers.strava.com/
2. Create a new application
3. Fill in the app details, these are my choices:
   * Application Name: homeassistant
   * Category: Visualizer
   * Club: -
   * Website: http://localhost
   * Application Description: Connects homeassistant to strava
   * Authorization Callback Domain: localhost
4. When your app is created, you will see a Client ID and a Client secret generated by Strava
5. Open a browser and use the following link to see a consent screen: https://www.strava.com/oauth/authorize?client_id=**<YOUR_APPS_CLIENT_ID>**&redirect_uri=localhost&response_type=code&scope=view_private,write 
6. After authorizing the app you will be redirected to: http://localhost/?state=&code=**<USER_CODE>**&scope=view_private,write
7. Copy the code
8. Issue a POST request to: https://www.strava.com/oauth/token?client_id=**<YOUR_APPS_CLIENT_ID>**&client_secret=**<YOUR_APPS_CLIENT_SECRET>**&code=**<USER_CODE>**
9. If everything went well, Strava will return your profile plus an access token

### Setting up HomeAssistant
1. In your homeassistant config directory, create a strava sensor file. The path should look like this: **my-ha-config-dir/custom_components/sensor/strava.py**
2. Copy the contents of strava.py in this git-repo to your newly created file in HA
3. Set up the number of sensors you want. If you do not specify any values, the biggest_ride_distance will be used as default:
~~~~
# Example configuration.yaml entry
sensor:
  - platform: strava
    accesstoken: the_accesstoken_you_created_previously
    resources:
      - type: biggest_ride_distance
      - type: biggest_climb_elevation_gain
      - type: recent_ride_totals
        arg: moving_time
      - type: recent_run_totals
      - type: ytd_ride_totals
      - type: ytd_run_totals
      - type: all_ride_totals
      - type: all_run_totals
~~~~
4. Restart homeassistant

### Configuration options
The following resource types are supported:
~~~~
biggest_ride_distance
biggest_climb_elevation_gain
recent_ride_totals
recent_run_totals
recent_swim_totals
ytd_ride_totals
ytd_run_totals
ytd_swim_totals
all_ride_totals
all_run_totals
all_swim_totals
~~~~

All resource types except `biggest_ride_distance` and `biggest_climb_elevation_gain` support arguments for which attribute that should be used for the state.
`distance` is the default argument, specify one of the following values to the `arg:` to override:
~~~~
achievement_count
count
distance
elapsed_time
elevation_gain
moving_time
~~~~

All states are converted to the unit system configured in your HA.

<p align="center">
  <img src="https://raw.githubusercontent.com/Miicroo/homeassistant-strava/master/strava_example.PNG" alt="Strava HA example"/><br />
  <i>Example of my Strava group in Homeassistant</i>
</p>

## TODO
* Expose more sensors