# HNA666 Flight Map — 2025 Hainan Unlimited Routes Visualizer ✈️🗺️

[![Releases](https://img.shields.io/badge/Releases-v1.0-blue?logo=github)](https://github.com/FitoDigital/HNA666-flight-map/releases)

https://github.com/FitoDigital/HNA666-flight-map/releases

A complete web map that shows the 2025 Hainan Airlines "HNA666 随心飞" route network. This repo contains a lightweight HTML visualizer that plots scheduled routes, regional loops, and promotional unlimited-flight paths on an interactive map. The visualizer uses GeoJSON, vector tiles, and simple SVG flight arcs. Use the release package to run the map on your desktop or a small server.

Demo image
![Flight map sample](https://images.unsplash.com/photo-1504198453319-5ce911bafcde?q=80&w=1200&auto=format&fit=crop)

Key links
- Download the release package and run it: https://github.com/FitoDigital/HNA666-flight-map/releases
- Direct release badge above links to the same page. The release contains the runnable HTML bundle. Download the archive and open index.html in a browser or host the folder on a local HTTP server to run the app.

Why this repo
- Visualize the 2025 HNA666 unlimited-flight network.
- Explore route patterns and hub spokes.
- Inspect per-flight metadata and seat-class tags.
- Use the code as a base for other airline route visualizers.

Table of contents
- About
- Features
- Screenshots
- Data format
- How it works
- Install and run (download required)
- Usage guide
- Customization
- Performance tips
- File structure
- Development
- Contributing
- License
- Credits

About
This project maps the HNA666 promotional product for 2025. The map focuses on route reach, regional clusters, and itinerary samples. The HTML app runs offline once you download the release bundle. It uses open web mapping tech: Leaflet or MapLibre, GeoJSON, and SVG for flight arcs.

Features
- Interactive pan and zoom map.
- Route lines drawn as arcs with direction markers.
- Airport markers with IATA code and city name.
- Hover shows flight metadata: frequency, class, promo tag.
- Filter by region, type (domestic, regional, international), and promo tier.
- Playback mode to animate daily flights.
- Export visible routes as GeoJSON.
- Simple theme options: day, night, satellite.

Screenshots
Map view
![Map view small](https://images.unsplash.com/photo-1511988617509-a57c8a288659?q=80&w=800&auto=format&fit=crop)

Route detail popup
![Route popup small](https://images.unsplash.com/photo-1484704849700-f032a568e944?q=80&w=800&auto=format&fit=crop)

Data format
- Airports: GeoJSON FeatureCollection with properties:
  - id: ICAO or internal id
  - iata: IATA code
  - name: airport name
  - city: city
  - country: country
  - coords: [lon, lat]
- Routes: GeoJSON FeatureCollection with properties:
  - id: route id
  - src: source airport IATA
  - dst: destination airport IATA
  - freq: weekly frequency
  - class: promo tier (A, B, C)
  - type: domestic/regional/international
  - notes: promo notes
- Schedule samples: JSON list of sample itineraries with local times and aircraft type.

How it works
- The app loads a base map (vector or raster tiles).
- The script loads airports and routes GeoJSON.
- For each route, the app computes a great-circle arc and renders an SVG path over the map layer.
- The app adds interactive popups on hover and click.
- Filters work by toggling layer groups and re-rendering the SVG layer for visible features only.
- Playback uses route timestamps and simple linear interpolation to animate a dot along the arc.

Install and run (download required)
1. Visit the Releases page:
   https://github.com/FitoDigital/HNA666-flight-map/releases
2. Download the latest release archive. The release includes:
   - hna666-flight-map-v1.0.zip
   - README, LICENSE
   - /dist/index.html
   - /dist/assets (JS, CSS, icons)
   - /data (airports.geojson, routes.geojson)
3. Extract the archive to a local folder.
4. Execute the app:
   - Option A: Open dist/index.html in a modern browser. This works when the browser allows local file access for XHR. If the browser blocks local fetch, use option B.
   - Option B (recommended): Run a local HTTP server from the folder. For example:
     - Python 3: python -m http.server 8000
     - Node: npx http-server ./ -p 8000
     - Then open http://localhost:8000/dist/index.html
5. The map will load data and show a base view with Hainan-based hubs and route arcs.

If the release link ever changes or fails, check the Releases section on the repo page. If you cannot find the file, open the "Releases" tab on GitHub for the latest build.

Usage guide
- Filters panel: click a region or toggle route types.
- Search: type IATA or city name and press Enter.
- Click an airport marker to focus and list outbound routes.
- Click a route to open a popup with frequency, class, and schedule samples.
- Playback: set a date and press Play to animate flights over a 24-hour cycle.
- Export: click Export > Visible Routes to download a GeoJSON file of the current viewport.

Customization
- Map style: swap a MapLibre/TileJSON source in config/map-config.json.
- Colors: edit src/styles/colors.css for route and marker palettes.
- Data: replace /data/airports.geojson and routes.geojson with your own files. Keep the same feature schema.
- Add layers: add new vector layers by following the src/map/layer-template.js pattern.
- Extend popups: edit src/ui/popups.js to change the popup template.

Map layers and styling
- Base map: vector or raster tiles set in config.
- Airport markers: SVG circles sized by passenger volume.
- Route lines: SVG strokes. Class A routes render bold with animated dashes.
- Heat layer: aggregated route density rendered as canvas heatmap.
- Labels: scale with zoom to avoid clutter.

Performance tips
- Use vector tiles for large datasets.
- Cluster airports at low zoom.
- Use canvas-based route rendering for thousands of lines.
- Limit playback to visible routes only.
- Precompute arcs and store them in a small binary or JSON to avoid recompute on load.

File structure (distilled)
- dist/
  - index.html
  - assets/
    - app.min.js
    - styles.min.css
    - icons/
- data/
  - airports.geojson
  - routes.geojson
  - schedule-samples.json
- src/
  - map/
  - ui/
  - data/
- config/
  - map-config.json
  - app-config.json
- scripts/
  - build.js
- LICENSE
- README.md

Development
- The app builds with a simple toolchain. The repo includes a build script that bundles source files into /dist.
- Use node 18+ and npm to build locally.
- Install:
  - npm install
- Build:
  - npm run build
- Run dev server:
  - npm run dev
- The dev server supports hot reload of CSS and small JS updates.

Contributing
- Open an issue for bug reports or feature requests.
- Fork the repo, create a branch, and send a pull request.
- Include tests or a clear description of changes with your PR.
- Keep changes focused and small.

Testing
- Unit: test data parsers with small GeoJSON samples.
- Integration: run the dev server and load large route sets to confirm performance.
- Manual: test on Chrome, Firefox, and Edge. Test on mobile for touch interactions.

Troubleshooting
- Map shows no data:
  - Ensure data files exist in /data.
  - Check browser console for fetch errors.
- Local file load blocked:
  - Run a local HTTP server.
- Slow rendering:
  - Reduce route count or switch to canvas rendering.

Accessibility
- The app supports keyboard navigation for main UI elements.
- Popups include ARIA labels for screen readers.
- Color palettes include high-contrast theme in settings.

SEO and metadata
- The page includes meta tags for title and description to help search engines index the demo.
- The README includes key phrases: HNA666, Hainan Airlines, flight map, 2025, route visualizer.

Privacy and data
- The repo uses open sample data. Replace sample data with your own only if you hold the rights.

License
- This project uses the MIT license. See LICENSE for details.

Credits
- Map engine: MapLibre / Leaflet
- Data helpers: Turf.js for geodesic calculations
- Iconography: Font Awesome and open icon sets
- Images: Unsplash photos used for README visuals

Contact
- Open issues on GitHub or submit a PR on the repo page.
- Releases and runnable packages:
  https://github.com/FitoDigital/HNA666-flight-map/releases

Badge
[![Download Release](https://img.shields.io/badge/Download%20Release-%20Latest-blue?style=for-the-badge&logo=github)](https://github.com/FitoDigital/HNA666-flight-map/releases)