# Research Sources

This skill was built by analyzing world-champion GeoGuessr techniques, SOTA AI geolocalization
research, and practical agent implementation guides.

---

## Videos

### Pro Player Techniques & GeoGuessr Strategy

1. **Every Trick a Pro GeoGuessr Player Uses to Win (ft. RAINBOLT) | WIRED**
   - https://www.youtube.com/watch?v=u3sVtwexp0o
   - Rainbolt demos bollard recognition, utility pole styles, license plates, road quality, soil color, camera angle meta, and "vibe guesses".

2. **How I Identify Any Location in Seconds (Rainbolt)**
   - https://www.youtube.com/watch?v=H3L9bZ8s3yE
   - Rainbolt's speed-identification method. Core lesson: learn telephone poles, bollards, and license plates first.

3. **GeoGuessr Tips That Will Make You Better Instantly**
   - https://www.youtube.com/watch?v=WFvjQ6yEQv0
   - Google Street View camera generations (Gen 1-4), Google car color/roof rack variations, driving side elimination, sun/shadow hemisphere tricks.

4. **The Ultimate GeoGuessr Strategy Guide**
   - https://www.youtube.com/watch?v=0p5Eb4OSZCs&t=82s
   - Hierarchical elimination: continent to country to region to city. Road line colors, guardrail styles, domain extensions, phone number formats.

5. **GeoGuessr: From Beginner to Pro**
   - https://www.youtube.com/watch?v=Lnfwp9EGsAo
   - Structured progression guide from language on signs through to satellite dish direction and km markers.

6. **GeoGuessr Tips: Unveiling Hidden Geographic Clues**
   - https://www.youtube.com/watch?v=-2yVhB7q7l4
   - European-focused. France's blue utility pole stickers, Poland's "holy poles", Czech fluorescent orange bollard stripes.

7. **How To Start Playing GeoGuessr | Beginner Tips**
   - https://www.youtube.com/watch?v=CsXjzBN1s9g
   - Road sign analysis, sun position, soil color (red = Brazil), Google SV generations, Poland's green signs.

8. **Star Player Trevor Rainbolt's Tips for GeoGuessr Success | PBS News**
   - https://www.pbs.org/video/where-in-the-world-1726343188/
   - PBS interview where Rainbolt demonstrates using trash can logos, address numbers, Overpass Turbo + ChatGPT.

### AI Playing GeoGuessr / Geolocalization

9. **Code a Vision LLM Agent that Plays GeoGuessr (LangChain)**
   - https://www.youtube.com/watch?v=OyDfr0xIhss
   - Full tutorial: pyautogui for screenshots, base64 encoding, send to GPT-4o/Claude/Gemini via LangChain, parse lat/lon, Mercator projection conversion, click minimap. Cost: ~$0.50/100 rounds.

10. **OSINT at Home #4: Identify Location from Photo or Video (Benjamin Strick)**
    - Search: "OSINT at Home #4 Benjamin Strick geolocation" on YouTube
    - EXIF extraction, reverse image search (Google/TinEye/Yandex), shadow analysis, satellite imagery cross-referencing.

11. **How An Investigator Finds Your Location From One Photo**
    - Referenced at: https://craighays.com/how-an-investigator-can-find-your-location-from-one-photograph/
    - Full OSINT geolocation workflow: metadata, visual analysis, reverse image search, satellite comparison, Street View confirmation.

12. **Rainbolt Shows How to Find the Location of Any Photo in 2 Minutes**
    - Referenced at: https://petapixel.com/2023/07/28/youtuber-shows-how-to-find-the-location-of-any-photo-in-two-minutes/
    - Overpass Turbo (OpenStreetMap query tool) + ChatGPT to generate location queries, then Google Street View to verify.

13. **GeoGuessing with Deep Learning (Andrew Healey)**
    - https://healeycodes.com/geoguessing-with-deep-learning
    - ResNet50 hierarchical model trained on 5M Flickr images. 66.7% country-level accuracy vs 13.9% for humans.

14. **Watching o3 Guess a Photo's Location (Simon Willison)**
    - https://simonw.substack.com/p/watching-o3-guess-a-photos-location
    - o3 iteratively crops/zooms, reasons through vegetation + architecture + license plates, runs web search, converges. Got El Granada, CA correct.

---

## Articles

### SOTA AI Geolocalization

1. **Jerry Wei — Claude Plays GeoGuessr**
   - https://www.jerrywei.net/blog/claude-plays-geoguessr
   - Evaluated Claude on 210K images from OpenStreetView-5M. Chain-of-thought visual analysis to lat/lon.

2. **Auto-GeoGuessr: Enabling Knowledge Retrieval for Vision Tasks with Agents (Calzaretta)**
   - https://medium.com/@j.calzaretta.ai/auto-geoguessr-enabling-knowledge-retrieval-for-vision-tasks-with-agents-9c5ba9cddb7f
   - Compared 4 agent architectures: Prescribed Chain, Single Agent, Multi-Agent w/ Supervisor, ReAct Agent. ReAct was most accurate but hit recursion limits.

3. **Coding a GeoGuessr Auto AI Agent with Vision LLMs (Enric Domingo)**
   - https://medium.com/@enricdomingo/coding-a-geoguessr-autonomous-ai-bot-with-vision-llms-gpt-4o-claude-3-5-and-gemini-1-5-908faf3bc3c7
   - Full implementation with calibration system, Mercator projection, retry logic.

4. **ChatGPT Becomes a Formidable Geo-Guesser (Tom's Hardware)**
   - https://www.tomshardware.com/tech-industry/artificial-intelligence/chatgpt-becomes-a-formidable-geo-guesser-after-the-latest-model-updates
   - o3/o4-mini visual chain-of-thought: crop, rotate, zoom as part of reasoning.

5. **The Latest Viral ChatGPT Trend: Reverse Location Search (TechCrunch)**
   - https://techcrunch.com/2025/04/17/the-latest-viral-chatgpt-trend-is-doing-reverse-location-search-from-photos/
   - Privacy implications of AI photo geolocation.

### Academic Papers (SOTA Models)

6. **PIGEON: Predicting Image Geolocations (CVPR 2024)**
   - Paper: https://arxiv.org/abs/2307.05845
   - CLIP + semantic geocells via Voronoi tessellation. 400K GeoGuessr panoramas. 40%+ within 25km. Beat Rainbolt 6-0.

7. **GeoGuess: Multimodal Reasoning with Hierarchy of Visual Information (2025)**
   - https://arxiv.org/abs/2506.16633
   - Introduces GeoExplain (first expert-level explanation dataset) + SightSense 3-stage pipeline.

8. **GeoReasoner (ICML 2024)**
   - https://arxiv.org/abs/2406.18572
   - First VLM fine-tuned for street view localization. +25% country, +38% city over other LVLMs.

9. **GaGA: Interactive Global Geolocation Assistant (2024)**
   - https://arxiv.org/abs/2412.08907
   - 5M image-text pairs. Allows user interaction/correction during inference. SOTA on GWS15k.

10. **GeoChain: Multimodal Chain-of-Thought for Geographic Reasoning (2025)**
    - https://arxiv.org/abs/2506.00785
    - Benchmark: 1.46M images, 30M+ Q&A pairs, 21-step CoT sequences. Tested GPT-4.1, Claude 3.7, Gemini 2.5.

### OSINT & Techniques

11. **Geolocating Pictures via OSINT & AI (Ron Kaminsky)**
    - https://medium.com/@ronkaminskyy/geolocating-pictures-via-osint-ai-187117e773ea

12. **120 GeoGuessr Tips (Jarrod Hill)**
    - https://medium.com/@jarrodhill257/geoguessr-tips-d0d7f193b763

---

## Reference Databases

| Resource | URL | Contents |
|----------|-----|----------|
| GeoTips | https://geotips.net/ | Country-by-country clue guides |
| Geomastr | https://geomastr.com/ | Visual comparison: bollards, plates, signs |
| Geometas | https://geometas.com/ | Region-specific meta-clues with images |
| Plonk It Guide | https://www.plonkit.net/guide | Structured all-category guide |
| GeoSpy | https://geospy.ai/ | AI geolocalization API (1-25km standard, ~1m with SuperBolt) |
| Mapillary | https://www.mapillary.com/ | Open street-level imagery (CC BY-SA) |

---

## Agent-Friendly Map Tools

| Tool | URL | Capabilities |
|------|-----|-------------|
| Google Maps Grounding Lite MCP | https://developers.google.com/maps/ai/grounding-lite | 18 tools: geocode, reverse-geocode, search-places, static-map, elevation, etc. |
| Community Google Maps MCP | https://github.com/cablate/mcp-google-map | Full Google Maps API including Street View static images |
| Mapbox MCP Server | https://www.mapbox.com/blog/introducing-the-mapbox-model-context-protocol-mcp-server | Geocoding, POI search, routing, map rendering |
| Overpass Turbo | https://overpass-turbo.eu/ | Query OpenStreetMap for specific features |
| Google Street View Static API | https://developers.google.com/maps/documentation/streetview | Request panorama images at specific coords + heading |
