# Current Weather

A small Apache Camel service that accepts a person name and city, looks up the city coordinates using the Open-Meteo geocoding API, fetches the current weather for that location from the Open-Meteo forecast API, and returns a compact JSON response.

## Overview

This repository contains a Camel route-based service under `service/`.

## External dependencies

The service makes outbound HTTP calls to:

- Open-Meteo Geocoding API: `https://geocoding-api.open-meteo.com/v1/search`
- Open-Meteo Forecast API: `https://api.open-meteo.com/v1/forecast`

Because of this, the service requires network access to those endpoints at runtime.

### Example

```sh
curl -X POST http://localhost:8080/process \
  -H 'Content-Type: application/json' \
  -d '{"name":"Alice","city":"Helsinki"}'
```

## Notes

- The service currently resolves the first geocoding match returned by Open-Meteo.
- The route logs the input name/city and the resolved forecast location.
