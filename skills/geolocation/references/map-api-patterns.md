# Map API Patterns for Agents

This reference covers how to use map APIs programmatically for geolocation verification —
no click-and-drag required.

---

## Google Maps Grounding Lite MCP

The recommended integration. Provides 18 tools via Model Context Protocol.

### Setup
1. Create a Google Cloud project
2. Enable the Maps Grounding Lite API service
3. Get an API key or OAuth client ID
4. Add the MCP server to your agent's configuration

### Key Tools for Geolocation

**`reverse-geocode`** — Validate a coordinate guess
```
Input: lat=48.8566, lon=2.3522
Output: "Paris, France" + full address breakdown
```
Use after forming your best hypothesis to check if the area name matches what you expect.

**`search-nearby`** — Find matching landmarks/businesses
```
Input: location=(lat, lon), radius=500m, keyword="restaurant name from photo"
Output: List of matching places with exact coordinates
```
Use when you spot a business name or landmark in the photo.

**`search-places`** — Text-based place search
```
Input: "Hotel Marques de Pombal Lisbon"
Output: Place details with coordinates, photos, reviews
```
Use when you can read a business name or hotel name in the photo.

**`static-map`** — Generate a map image at coordinates
```
Input: center=(lat, lon), zoom=15, size=600x400
Output: Map image for visual comparison
```
Use to check if the road layout / terrain at your candidate location matches the photo.

**`elevation`** — Check terrain profile
```
Input: locations=[(lat1, lon1), (lat2, lon2), ...]
Output: Elevation in meters for each point
```
Use when the photo shows distinctive terrain (mountain, valley, coastline) to verify altitude matches.

**`geocode`** — Convert address to coordinates
```
Input: "123 Main Street, Springfield, IL"
Output: Latitude, longitude
```
Use when you can read an address in the photo.

---

## Google Street View Static API

Lets the agent "look around" at a location without browser interaction.

### Request Format
```
https://maps.googleapis.com/maps/api/streetview
  ?size=600x400
  &location=LAT,LON
  &heading=DEGREES    (0=North, 90=East, 180=South, 270=West)
  &pitch=0            (0=horizontal, 90=up, -90=down)
  &fov=90             (field of view, default 90)
  &key=API_KEY
```

### Agent Pattern: 4-Direction Scan
To "look around" at a candidate location, request images at 4 headings:
```
heading=0    (North)
heading=90   (East)
heading=180  (South)
heading=270  (West)
```
Compare each against the source photo to find the matching direction.

### Agent Pattern: Approach Verification
If you know the road direction, request Street View images along the road at intervals:
```
Location 1: (lat, lon)        — starting point
Location 2: (lat+0.001, lon)  — ~111m north
Location 3: (lat+0.002, lon)  — ~222m north
```
Look for the specific building/sign/landmark from the source photo.

---

## Overpass Turbo (OpenStreetMap)

Query OpenStreetMap's database for specific features. Especially useful for finding region-specific
objects that pros use for pinpointing (fire hydrants, trash cans, postboxes).

### Example: Find all fire hydrants within 500m of coordinates
```
[out:json][timeout:25];
(
  node["emergency"="fire_hydrant"](around:500,LAT,LON);
);
out body;
```

### Example: Find restaurants matching a name
```
[out:json][timeout:25];
(
  node["amenity"="restaurant"]["name"~"RESTAURANT_NAME",i](around:5000,LAT,LON);
  way["amenity"="restaurant"]["name"~"RESTAURANT_NAME",i](around:5000,LAT,LON);
);
out body;
```

### Example: Find gas stations of a specific brand
```
[out:json][timeout:25];
(
  node["amenity"="fuel"]["brand"~"Pemex",i](around:10000,LAT,LON);
);
out body;
```

### Using via API
```
https://overpass-api.de/api/interpreter?data=[QUERY]
```
URL-encode the query. Results come back as JSON with coordinates for each matching feature.

---

## Mapillary API

Open-source street-level imagery. Useful as a free alternative to Google Street View.

### Key Endpoints
- **Image search**: Find street-level photos near coordinates
- **Image metadata**: Get capture date, camera info, compass angle
- **Sequence**: Get a sequence of images along a road

### Access
- Free tier available
- API key required (register at mapillary.com)
- Images are CC BY-SA licensed

### Useful for
- Verifying locations in countries with poor Google Street View coverage
- Getting multiple angles of a location
- Checking historical imagery for changes

---

## Mapbox MCP Server

Alternative to Google Maps. Good for geocoding and POI lookup.

### Key Capabilities
- **Forward geocoding**: Address/place name to coordinates
- **Reverse geocoding**: Coordinates to address
- **POI search**: Find businesses/landmarks near coordinates
- **Directions**: Calculate routes (useful for verifying road connections)
- **Static maps**: Generate map images

### Setup
MCP server available at: https://www.mapbox.com/blog/introducing-the-mapbox-model-context-protocol-mcp-server

---

## Recommended Verification Workflow

When you have a candidate location, verify in this order:

1. **reverse-geocode** the coordinates — does the country/region match your visual analysis?
2. **search-nearby** for any specific businesses/landmarks you identified in the photo
3. **static-map** at the coordinates — does the road layout match?
4. **Street View** at the coordinates — does the street-level view match the source photo?
5. **elevation** — does the altitude match the terrain you see?

If step 1 fails (wrong country), go back to visual analysis and reconsider.
If steps 2-5 fail, adjust your coordinates within the same region and try again.
