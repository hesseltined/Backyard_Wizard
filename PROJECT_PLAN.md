# Backyard Wizard - Project Plan

**Version:** 1.0.0  
**Date:** 2025-10-25  
**Author:** Doug Hesseltine  
**Copyright:** Technologist.services 2025

---

## Executive Summary

Backyard Wizard is a map-first, AI-powered landscape maintenance platform that generates dynamic, hyperlocal maintenance schedules based on actual plants, microclimate, phenology, and trusted extension guidance—all using free, open data sources.

**Core Innovation:** Video-based plant identification + geospatial mapping + phenology-aware rule engine = personalized weekly task lists.

---

## Phase 0: Prototype (Weeks 1-4)

### Goal
Validate core technical stack and prove the value proposition with minimal plant ID and manual plant assignment.

### Deliverables

#### Week 1: Project Setup & Infrastructure
- [ ] Initialize monorepo structure (backend, frontend, ml-service, docs)
- [ ] Set up Docker Compose with Postgres + PostGIS + Redis
- [ ] Configure CI/CD pipeline (GitHub Actions)
- [ ] Set up AWS S3 or MinIO for object storage
- [ ] Create initial database schema with migrations
- [ ] Set up environment configuration and secrets management

#### Week 2: Frontend Foundation
- [ ] Initialize React app with TypeScript
- [ ] Integrate Mapbox GL JS with basic map display
- [ ] Implement address search and geocoding
- [ ] Build polygon drawing tools (lawn areas, beds)
- [ ] Build point placement tools (trees, shrubs)
- [ ] Create zone editing UI (drag vertices, delete, label)
- [ ] Implement basic authentication (email/password)

#### Week 3: Backend & Weather Integration
- [ ] Build FastAPI service with auth endpoints
- [ ] Implement property CRUD endpoints
- [ ] Implement map_features CRUD endpoints
- [ ] Integrate Open-Meteo API client
- [ ] Build GDD calculator (base 32, 50°F)
- [ ] Build soil temperature proxy calculator
- [ ] Fetch and cache USDA hardiness zone data
- [ ] Create weather_daily table and sync job

#### Week 4: Initial Rule Engine
- [ ] Create rules table and seed with 20 base rules:
  - 5 cool-season lawn tasks (pre-emergent, fertilizer, mowing height)
  - 5 warm-season lawn tasks
  - 10 common tree/shrub tasks (pruning windows, watering)
- [ ] Build rule evaluation engine
- [ ] Implement task generation pipeline
- [ ] Create tasks table and management endpoints
- [ ] Build simple weekly task list UI
- [ ] Manual plant catalog (50 species) with dropdown assignment

**Milestone:** User can map property, assign plants manually, see 10-15 relevant tasks for next 4 weeks.

---

## Phase 1: MVP (Weeks 5-16)

### Goal
Full video-based plant identification, expanded rule library, phenology integration, and complete task management.

### Deliverables

#### Weeks 5-6: Media Upload Pipeline
- [ ] Build video/photo upload UI with progress bars
- [ ] Implement chunked upload with retry logic
- [ ] Set up Celery/RQ worker infrastructure
- [ ] Build FFmpeg-based frame extraction service
- [ ] Create media storage and thumbnail generation
- [ ] Build async job status tracking API

#### Weeks 7-9: Plant Identification ML Pipeline
- [ ] Research and select base model (EfficientNet/ViT on PlantCLEF)
- [ ] Build ML microservice (FastAPI + PyTorch/TensorFlow)
- [ ] Implement frame extraction with motion-adaptive sampling
- [ ] Build plant detection and classification inference
- [ ] Implement geospatial clustering algorithm
- [ ] Create candidate ranking with USDA PLANTS priors
- [ ] Build plant_aliases and plants tables
- [ ] Create plant review and confirmation UI
- [ ] Add "unknown" plant workflow with re-shot suggestions

#### Weeks 10-11: Taxonomy & Plant Catalog
- [ ] Integrate USDA PLANTS database (download and import)
- [ ] Integrate POWO (Plants of the World Online) API
- [ ] Build species normalization service (synonyms, accepted names)
- [ ] Expand plant catalog to 200+ species:
  - Top 100 ornamental trees/shrubs
  - Top 50 perennials
  - Top 40 vegetables
  - Common cultivar groups (hydrangea, clematis, rose)
- [ ] Add plant attributes schema (pruning group, bloom time, etc.)
- [ ] Build plant search and autocomplete

#### Weeks 12-13: Expanded Rule Library
- [ ] Create rule authoring template and validation
- [ ] Build 150+ additional rules with extension citations:
  - 30 turf rules (fertilization, pest timing, mowing)
  - 40 tree/shrub pruning rules by type
  - 30 perennial maintenance rules
  - 25 vegetable planting/care rules
  - 25 pest/disease monitoring rules
- [ ] Implement rule versioning
- [ ] Add confidence scoring per rule
- [ ] Build rule testing framework
- [ ] Create citation tracking and attribution

#### Weeks 14-15: Phenology Integration
- [ ] Integrate USANPN API for Spring Index
- [ ] Implement lilac phenophase models
- [ ] Add phenophase triggers to rule engine
- [ ] Fetch and cache NOAA frost date probabilities
- [ ] Build frost window calculator
- [ ] Add "relative to frost" timing triggers
- [ ] Implement pest model staging (degree-day based)

#### Week 16: Task Management & UX Polish
- [ ] Build task status management (done/snooze/skip)
- [ ] Add task notes and photo attachments
- [ ] Implement weekly digest batching by materials
- [ ] Build calendar view (week/month)
- [ ] Create ICS export for calendar sync
- [ ] Add email/push notification system
- [ ] Build task detail view with how-to steps
- [ ] Add materials checklist and time estimates
- [ ] Create safety notes display

**Milestone:** User uploads video, confirms 80%+ plants via AI suggestions, receives 20-30 phenology-tuned tasks with full details.

---

## Phase 2: Enrichment (Weeks 17-28)

### Goal
Add soils integration, pest models, bulk operations, and export capabilities.

### Deliverables

#### Weeks 17-18: Soils Integration
- [ ] Integrate USDA NRCS Soil Data Access (SDA)
- [ ] Fetch soil series by lat/lon
- [ ] Extract drainage class, texture, pH estimates
- [ ] Add soils table and per-property caching
- [ ] Update rule engine to use soil constraints
- [ ] Add irrigation frequency recommendations
- [ ] Adjust fertilizer timing based on drainage

#### Weeks 19-20: Pest & Disease Models
- [ ] Implement common pest degree-day models:
  - Emerald Ash Borer
  - Japanese Beetle
  - Spotted Lanternfly
  - Black Vine Weevil
  - Boxwood Blight risk
- [ ] Add pest monitoring tasks with phenology triggers
- [ ] Build disease risk alerts (powdery mildew, fire blight)
- [ ] Create integrated pest management (IPM) guidance

#### Weeks 21-22: Data Management & Export
- [ ] Build bulk zone import (GeoJSON/KML)
- [ ] Add property duplication and templates
- [ ] Create seasonal PDF export (12-week plan)
- [ ] Build plant inventory export (CSV)
- [ ] Add photo gallery per property
- [ ] Implement data backup and restore

#### Weeks 23-24: Regional Customization
- [ ] Add state-specific extension content
- [ ] Build region-specific rule packs
- [ ] Add local variety recommendations
- [ ] Implement microclimate adjustments (urban heat island, elevation)
- [ ] Add custom zone creation (USDA + user override)

#### Weeks 25-26: Analytics & Insights
- [ ] Build task completion tracking
- [ ] Add seasonal insights dashboard
- [ ] Create plant performance notes
- [ ] Implement garden journal
- [ ] Add year-over-year comparisons
- [ ] Build maintenance hour tracking

#### Weeks 27-28: Performance & Scale
- [ ] Optimize rule evaluation (caching, indexing)
- [ ] Add CDN for static assets
- [ ] Implement background job prioritization
- [ ] Add database query optimization
- [ ] Build load testing suite
- [ ] Implement rate limiting and abuse prevention

**Milestone:** Complete platform with soils, pests, exports, and regional customization. Ready for 100+ beta users.

---

## Phase 3: Pro/Scale (Weeks 29+)

### Goal
Multi-property management, team features, community content, and optional integrations.

### Deliverables

#### Multi-Property Management
- [ ] Build property portfolio view
- [ ] Add team roles and permissions
- [ ] Create client management system
- [ ] Build per-property task delegation
- [ ] Add cross-property reporting
- [ ] Implement bulk task scheduling

#### Community & Content
- [ ] Build community plant templates
- [ ] Add user-submitted rule suggestions
- [ ] Create regional knowledge base
- [ ] Implement plant ID improvement feedback loop
- [ ] Add community photo gallery
- [ ] Build extension content updates pipeline

#### Integrations
- [ ] Rachio irrigation integration (optional)
- [ ] Hydrawise integration (optional)
- [ ] Weather station hardware support
- [ ] Calendar app integrations (Google, Apple, Outlook)
- [ ] Smart speaker task reading

#### Monetization (Pro Tier)
- [ ] Multi-property support (5+ properties)
- [ ] Team collaboration features
- [ ] Priority ML processing
- [ ] Advanced reporting
- [ ] White-label PDF exports
- [ ] API access for partners

---

## Technical Architecture

### Frontend Stack
- **Framework:** React 18+ with TypeScript
- **State Management:** Redux Toolkit or Zustand
- **Mapping:** Mapbox GL JS
- **UI Components:** Tailwind CSS + Headless UI or shadcn/ui
- **Forms:** React Hook Form + Zod validation
- **Data Fetching:** React Query
- **Build:** Vite

### Backend Stack
- **API Framework:** Python FastAPI
- **Database:** PostgreSQL 15+ with PostGIS
- **Cache/Queue:** Redis
- **Task Queue:** Celery with Redis broker
- **Object Storage:** AWS S3 or MinIO
- **Auth:** JWT with refresh tokens
- **ORM:** SQLAlchemy 2.0

### ML Service
- **Framework:** PyTorch or TensorFlow
- **Server:** FastAPI microservice
- **Image Processing:** OpenCV, Pillow
- **Video Processing:** FFmpeg
- **Inference:** ONNX Runtime for production

### Infrastructure
- **Containerization:** Docker + Docker Compose
- **Orchestration:** Kubernetes (future) or AWS ECS
- **CI/CD:** GitHub Actions
- **Monitoring:** Prometheus + Grafana
- **Logging:** ELK Stack or CloudWatch
- **CDN:** CloudFront or Cloudflare

### Data Sources (All Free)
- **Weather:** Open-Meteo, NOAA NWS API
- **Phenology:** USANPN API
- **Taxonomy:** USDA PLANTS, POWO
- **Soils:** USDA NRCS SDA
- **Climate:** NOAA NCEI, ACIS
- **Maps:** OpenStreetMap, Mapbox (free tier)
- **Extension Content:** University extension websites (paraphrased with attribution)

---

## Database Schema Summary

### Core Tables
- **users:** Authentication and profile
- **properties:** User properties with location
- **map_features:** Spatial zones and points (PostGIS geometry)
- **plants:** Individual plant instances
- **plant_aliases:** AI detection history
- **tasks:** Generated maintenance tasks
- **rules:** Rule definitions with triggers
- **weather_daily:** Cached weather and computed metrics
- **phenology_status:** Phenological indicators
- **soils:** Soil characteristics per property
- **citations:** Extension source tracking

---

## Rule Engine Design

### Trigger Types
1. **Date-based:** Fixed calendar dates or DOY ranges
2. **GDD-based:** Accumulated growing degree days
3. **Soil temperature:** Proxy or actual soil temps
4. **Phenophase:** USANPN events or indicators
5. **Frost-relative:** Days before/after frost dates
6. **Weather-event:** Recent rain, drought periods

### Rule Evaluation Flow
1. Load active rules filtered by plant types on property
2. Apply region constraints (zone, state)
3. Check trigger conditions against current metrics
4. Evaluate windows (earliest, latest, optimal)
5. Check recurrence rules and history
6. Generate task with priority and confidence
7. Batch similar tasks by materials
8. Suppress duplicates and respect user overrides

### Example Rule Structure
```json
{
  "id": "hydrangea_macrophylla_prune_avoid",
  "species_selector": {
    "genus": "Hydrangea",
    "species": "macrophylla"
  },
  "region_constraints": {
    "zones": ["5-9"]
  },
  "trigger": {
    "type": "date_range",
    "start": "DOY_60",
    "end": "DOY_150"
  },
  "outputs": {
    "title": "Avoid pruning Hydrangea macrophylla",
    "description": "Bigleaf hydrangeas bloom on old wood. Prune only after flowering or you'll remove flower buds.",
    "task_type": "reminder",
    "priority": "high",
    "estimated_minutes": 5
  },
  "citations": [
    {
      "source": "Clemson Extension",
      "url": "https://hgic.clemson.edu/factsheet/hydrangea/"
    }
  ],
  "confidence": 0.95,
  "version": 1,
  "active": true
}
```

---

## Plant Identification Pipeline

### Input Processing
1. User uploads video or series of photos
2. Extract metadata (GPS, timestamp, device orientation)
3. Extract frames at 1-2 FPS with motion detection
4. Quality filtering (blur detection, lighting)

### ML Inference
1. Run object detection to isolate plants
2. Classify detected plants with species model
3. Generate top-3 candidates with confidence scores
4. Apply geographic priors (USDA county distribution + zone)
5. Re-rank candidates with ensemble scoring

### Geospatial Clustering
1. Group frames by temporal proximity
2. Cluster by GPS coordinates (if available)
3. Use visual SLAM features for indoor/GPS-denied
4. Output "spots" with representative frames

### Human Review
1. Display spots on map with thumbnails
2. Show top-3 species suggestions per spot
3. User confirms, selects alternate, or marks unknown
4. Collect correction feedback for model improvement

### Accuracy Targets
- **Top-1 accuracy:** 75%+ for common species
- **Top-3 accuracy:** 90%+ for common species
- **User confirmation rate:** 80%+ in first pass

---

## Task Generation Pipeline

### Daily Job (Per Property)
```
1. FETCH WEATHER
   - Get 16-day forecast from Open-Meteo
   - Pull recent observations from NWS/Meteostat
   - Update weather_daily table

2. COMPUTE METRICS
   - Calculate GDD for all relevant bases (32, 39, 45, 50, 55°F)
   - Compute soil temperature proxy (5cm, 10cm)
   - Update running totals since January 1

3. FETCH PHENOLOGY
   - Query USANPN for Spring Index
   - Get regional phenophase status
   - Update phenology_status table

4. EVALUATE RULES
   - Load active rules matching property plants/zones
   - Check region constraints
   - Evaluate triggers against current metrics
   - Apply windows and recurrence logic
   - Calculate task priority and confidence

5. GENERATE TASKS
   - Create new tasks for triggered rules
   - Update existing task windows if metrics change
   - Batch similar tasks by materials and location
   - Suppress duplicates and completed tasks

6. CREATE DIGEST
   - Group tasks by week and priority
   - Calculate total time and materials
   - Generate email/push notifications if enabled
```

---

## Security & Privacy

### Data Protection
- TLS 1.3 for all API communication
- AES-256 encryption at rest for media
- Bcrypt/Argon2 for password hashing
- JWT with short expiry and refresh tokens

### Media Handling
- Retain raw video only during processing (7 days max)
- Store extracted frames and thumbnails only
- User opt-in for anonymized data sharing
- Clear consent for model training usage

### Privacy Controls
- No third-party tracking/analytics without consent
- Option to disable location sharing
- Bulk data export and account deletion
- GDPR/CCPA compliance

---

## Testing Strategy

### Unit Tests
- Rule evaluation logic (100+ test cases)
- GDD and soil temp calculations
- Phenology window computation
- Spatial clustering algorithms

### Integration Tests
- End-to-end task generation pipeline
- Weather API integration and fallbacks
- ML inference pipeline
- Database migrations

### User Acceptance Testing
- 20 properties across 3 climate zones
- Measure setup time, species accuracy, task relevance
- A/B test phenology vs date-only scheduling
- Survey user satisfaction and completion rates

### Performance Testing
- Load test with 1000 concurrent users
- Test task generation for 10,000 properties
- ML inference throughput (frames/sec)
- API response times (p95 < 200ms)

---

## Success Metrics (MVP)

### Setup Experience
- **Target:** 80% of users complete setup in < 30 minutes
- **Measure:** Onboarding completion rate and time tracking

### Plant Identification
- **Target:** 90% top-3 accuracy for common species
- **Measure:** User confirmation rate, correction data

### Task Relevance
- **Target:** 90% of tasks rated "relevant" or "very relevant"
- **Measure:** In-app rating after task completion

### Engagement
- **Target:** 60% of users complete 1+ task in first month
- **Measure:** Task completion tracking, weekly active users

### Coverage
- **Target:** Support 80% of typical suburban yard plants
- **Measure:** Plant catalog size, user "not found" reports

---

## Risk Management

### Technical Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| ML accuracy too low | High | Start with manual entry fallback; improve model iteratively |
| Open-Meteo API limits | Medium | Cache aggressively; add NWS fallback |
| Rule complexity explosion | High | Strict rule schema; automated testing; versioning |
| Geospatial clustering fails | Medium | Allow manual pin placement; improve with user feedback |

### Business Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| Insufficient plant coverage | High | Prioritize top 200 species; crowdsource additions |
| Extension content changes | Medium | Version rules; monitor source URLs |
| User adoption slow | High | Pilot with engaged gardeners; iterate on feedback |
| Scaling costs | Medium | Optimize API calls; use CDN; efficient caching |

---

## Documentation Requirements

### User Documentation
- [ ] Getting started guide with video walkthrough
- [ ] Plant identification best practices
- [ ] Task management how-to
- [ ] FAQ and troubleshooting
- [ ] Extension source attribution page

### Developer Documentation
- [ ] Architecture decision records (ADRs)
- [ ] API documentation (OpenAPI/Swagger)
- [ ] Database schema documentation
- [ ] Rule authoring guide
- [ ] Deployment runbook

### Compliance Documentation
- [ ] Privacy policy
- [ ] Terms of service
- [ ] Data retention policy
- [ ] Extension content attribution and licensing
- [ ] Open source dependency licenses

---

## Next Steps (Immediate)

### Week 1 Priorities
1. Set up GitHub repository structure
2. Initialize Docker Compose with Postgres + PostGIS + Redis
3. Create initial database migrations
4. Build React app with Mapbox integration
5. Implement basic authentication
6. Set up FastAPI backend with first endpoints
7. Integrate Open-Meteo API client
8. Create first 5 lawn care rules

### Team Structure (Recommended)
- **Full-stack engineer:** Frontend + Backend (1-2 people)
- **ML engineer:** Plant ID pipeline (1 person, part-time initially)
- **Data engineer:** Rule authoring, data integration (1 person, part-time)
- **Designer/UX:** UI/UX design and user testing (1 person, part-time)

---

## Appendix A: Extension Content Sources

### Primary Sources
- **Cornell University:** Turf, perennials, vegetables
- **UC ANR/UC IPM:** California-specific, pest management
- **UMN Extension:** Cold climate, hydrangeas, clematis
- **Clemson Extension:** Southeast, shrubs, hydrangeas
- **Penn State Extension:** Mid-Atlantic, turf, trees
- **Rutgers NJAES:** Turf program, vegetables
- **NCSU Extension:** Turf, ornamentals, vegetables
- **UGA Extension:** Warm-season turf, crape myrtle
- **Texas A&M AgriLife:** Hot climate, drought-tolerant

### Content Guidelines
- Paraphrase guidance; never copy verbatim
- Always cite source with URL
- Update citations when content changes
- Track rule version with source version
- Regular audits (quarterly) for broken links

---

## Appendix B: API Endpoints (MVP)

### Authentication
- `POST /auth/register`
- `POST /auth/login`
- `POST /auth/refresh`
- `POST /auth/logout`

### Properties
- `GET /properties`
- `POST /properties`
- `GET /properties/{id}`
- `PATCH /properties/{id}`
- `DELETE /properties/{id}`

### Map Features
- `GET /properties/{id}/features`
- `POST /properties/{id}/features`
- `PATCH /features/{id}`
- `DELETE /features/{id}`

### Plants
- `GET /properties/{id}/plants`
- `POST /features/{id}/plants`
- `PATCH /plants/{id}`
- `DELETE /plants/{id}`
- `GET /plants/search?q={query}`

### Media Upload
- `POST /media/upload/video`
- `POST /media/upload/photos`
- `GET /media/jobs/{id}`

### Plant Identification
- `POST /identify/start`
- `GET /identify/{job_id}/status`
- `GET /identify/{job_id}/results`
- `POST /identify/{job_id}/confirm`

### Tasks
- `GET /properties/{id}/tasks`
- `GET /tasks/week/{date}`
- `PATCH /tasks/{id}`
- `POST /tasks/{id}/complete`
- `POST /tasks/{id}/snooze`
- `GET /tasks/calendar.ics`

### Weather
- `GET /properties/{id}/weather`
- `GET /properties/{id}/metrics`

---

## Appendix C: Technology Evaluation

### Map Libraries
| Option | Pros | Cons | Cost |
|--------|------|------|------|
| Mapbox GL JS | Excellent drawing tools, performance | Requires API key, free tier limited | $0-5/mo MVP |
| Leaflet + plugins | Free, flexible | Less polished drawing UX | $0 |
| Google Maps | Familiar to users | Expensive, less customizable | $200+/mo |

**Decision:** Mapbox GL JS with fallback to Leaflet if costs grow.

### ML Frameworks
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| PyTorch | Flexible, large community | Larger models | **Selected** |
| TensorFlow | Production-ready, TF Lite | Steeper learning curve | Backup |
| ONNX | Cross-platform, fast | Conversion complexity | Deployment format |

### Task Queue
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| Celery | Mature, well-documented | Python-specific | **Selected** |
| RQ | Simpler than Celery | Less features | Alternative |
| BullMQ | Node.js native | Requires Node backend | Not suitable |

---

**End of Project Plan**
