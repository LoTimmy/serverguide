`HomeKit support for the impatient`

```
shell> apt-get install libavahi-compat-libdnssd-dev
shell> npm install -g --unsafe-perm homebridge
shell> npm install -g --unsafe-perm homebridge-weather
shell> homebridge
```

`.homebridge/config.json`
```json
{
  "bridge": {
    "name": "Homebridge",
    "username": "CC:22:3D:E3:CE:30",
    "port": 51826,
    "pin": "031-45-154"
  }
}
```

---

```json
{
  "bridge": {
    "name": "Boojau5v",
    "username": "B8:27:EB:6E:88:A3",
    "port": 51826,
    "pin": "031-45-154"
  },
  "accessories": [
    {
      "accessory": "Weather",
      "apikey": "45a4a4c50e5f3422ebcfe8178e13ac20",
      "locationById": "1668341",
      "name": "OpenWeatherMap Temperature"
    }
  ]
}
```

#### :books: 參考網站：
- https://www.npmjs.com/package/homebridge
- https://www.npmjs.com/package/homebridge-openweathermap-temperature
- https://www.npmjs.com/package/homebridge-weather
- https://www.npmjs.com/package/homebridge-raspberrypi-temperature
- https://www.npmjs.com/package/homebridge-http
- https://home.openweathermap.org/users/sign_in
- https://github.com/home-assistant/homebridge-homeassistant
- https://www.npmjs.com/package/homebridge-bravia
