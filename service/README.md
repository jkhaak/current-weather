## Key files

- `route-configuration.camel.yaml` — exposes the REST endpoint
- `direct-fetchWeatherData.camel.yaml` — main processing route and external API calls
- `response.vm` — Velocity template used to build the response body
- `application.properties` — Camel runtime, health, metrics, Jib, and JKube configuration
- `deployment.jkube.yaml` — Kubernetes deployment snippet

## API

Request flow:

1. Accept a `POST` request on `/process`
2. Validate that the JSON payload contains `name` and `city`
3. Call the Open-Meteo geocoding API to resolve the city into latitude and longitude
4. Call the Open-Meteo forecast API to fetch `current_weather`
5. Return a JSON response rendered from a Velocity template

### Endpoint

- `POST /process`
- Consumes: `application/json`
- Produces: `application/json`

### Request body

```json
{
  "name": "Alice",
  "city": "Helsinki"
}
```

Both `name` and `city` are required.

### Successful response

The response is generated from `response.vm` and includes:

- `name`
- `city`
- `currentTemperature`
- `time`

Example shape:

```json
{
  "name": "Alice",
  "city": "Helsinki",
  "currentTemperature": "18.7",
  "time": "2026-06-14T10:00"
}
```

### Error response

If either `name` or `city` is missing, the route returns HTTP `400` with:

```json
{
  "error": "Bad Request",
  "message": "Fields 'name' and 'city' are required"
}
```
