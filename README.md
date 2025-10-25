# Backyard Wizard

**Version:** 1.0.0  
**Date:** 2025-10-25  
**Author:** Doug Hesseltine  
**Copyright:** Technologist.services 2025

---

## Overview

Backyard Wizard is a map-first, AI-powered landscape maintenance platform that generates dynamic, hyperlocal maintenance schedules based on actual plants, microclimate, phenology, and trusted extension guidance—all using free, open data sources.

### Core Innovation

**Video-based plant identification + geospatial mapping + phenology-aware rule engine = personalized weekly task lists**

---

## The Problem

Homeowners with mixed landscapes (lawn, trees, shrubs, perennials, vegetable beds) don't know what to do, when, and where. Advice is fragmented and date-based, not tailored to their exact plants, microclimate, or soil.

## The Solution

Backyard Wizard identifies plants from on-site video, places them on a geospatial map of your property, and auto-generates a dynamic maintenance calendar driven by:

- Local weather and forecasts
- Plant phenology (biological timing)
- Soil characteristics
- Plant-specific best practices from trusted university extension sources

---

## Key Features

### Phase 0: Prototype (Weeks 1-4)
- Interactive property mapping with zone drawing
- Manual plant assignment from curated catalog
- Weather integration with Growing Degree Days
- Basic rule engine with 20 lawn/tree/shrub tasks
- Weekly task list generation

### Phase 1: MVP (Weeks 5-16)
- AI-powered plant identification from video
- Expanded catalog (200+ species)
- Phenology integration (USANPN)
- Advanced task management with calendar sync
- 150+ maintenance rules with extension citations

### Phase 2: Enrichment (Weeks 17-28)
- Soil characteristics integration
- Pest and disease models
- Seasonal PDF exports
- Regional customization

### Phase 3: Pro/Scale (Weeks 29+)
- Multi-property management
- Team collaboration
- Community templates
- Optional smart home integrations

---

## Technology Stack

### Frontend
- React 18+ with TypeScript
- Mapbox GL JS for mapping
- Vite build system
- Tailwind CSS

### Backend
- Python FastAPI
- PostgreSQL with PostGIS (spatial data)
- Redis (caching and task queue)
- Celery (async workers)

### ML Service
- PyTorch for plant identification
- FFmpeg for video processing
- ONNX Runtime for inference

### Data Sources (All Free)
- **Weather:** Open-Meteo, NOAA NWS
- **Phenology:** USA National Phenology Network
- **Plants:** USDA PLANTS, POWO
- **Soils:** USDA NRCS Soil Data Access
- **Extension Content:** Cornell, Clemson, UMN, Rutgers, Penn State, NCSU, UGA, Texas A&M

---

## Quick Start

See [SETUP.md](SETUP.md) for detailed installation instructions.

```bash
# Clone repository
git clone https://github.com/hesseltined/Backyard_Wizard.git
cd Backyard_Wizard

# Copy environment template
cp .env.example .env

# Start services with Docker Compose
docker-compose up -d

# Access the application
# Frontend: http://localhost:5173
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/docs
```

---

## Documentation

- [PROJECT_PLAN.md](PROJECT_PLAN.md) - Complete project plan and roadmap
- [SETUP.md](SETUP.md) - Development environment setup
- `docs/architecture/` - Architecture decision records
- `docs/api/` - API documentation
- `docs/guides/` - User and developer guides

---

## Success Metrics (MVP)

- **Setup time:** 80% of users complete setup in < 30 minutes
- **Plant ID accuracy:** 90% top-3 accuracy for common species
- **Task relevance:** 90% of tasks rated relevant
- **Engagement:** 60% of users complete 1+ task in first month
- **Coverage:** Support 80% of typical suburban yard plants

---

## Development Phases

### Current Phase: Phase 0 - Prototype
**Timeline:** Weeks 1-4  
**Goal:** Validate core technical stack and value proposition

**Week 1 Priorities:**
1. Initialize monorepo structure
2. Set up Docker Compose infrastructure
3. Create database schema with PostGIS
4. Build React app with Mapbox integration
5. Implement authentication
6. Set up FastAPI backend
7. Integrate Open-Meteo API
8. Create initial rule set

---

## Contributing

(To be added)

---

## License

(To be determined)

---

## Contact

**Author:** Doug Hesseltine  
**Copyright:** Technologist.services 2025

---

## Acknowledgments

Backyard Wizard relies on data and guidance from:

- University extension services (Cornell, Clemson, UMN, Penn State, Rutgers, NCSU, UGA, Texas A&M)
- USDA (PLANTS Database, NRCS Soil Data)
- USA National Phenology Network
- NOAA (Weather, climate data)
- Open-Meteo (Weather API)
- Plants of the World Online (POWO)

All extension content is paraphrased and cited according to fair use guidelines.
