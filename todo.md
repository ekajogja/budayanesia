# Budayanesia Implementation Plan

## Technical Specifications

- **Frontend**: React 18 + Vite + Tailwind CSS
- **Backend**: Supabase (PostgreSQL, Auth, Storage)
- **Infrastructure**:
  - Docker for development/production
  - DigitalOcean Ubuntu 22.04 Droplet
- **Audio**: Howler.js for sound effects
- **State Management**: Zustand

## Docker Setup

### Development

```Dockerfile
# docker/dev.Dockerfile
FROM node:20
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5173
CMD ["npm", "run", "dev"]
```

### Production

```Dockerfile
# docker/prod.Dockerfile
FROM node:20 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

## DigitalOcean Deployment

1. Create Ubuntu 22.04 Droplet
2. Install Docker and Docker Compose
3. Configure firewall:

   ```bash
   ufw allow 80/tcp   # HTTP
   ufw allow 443/tcp  # HTTPS
   ```

4. Deployment script (`deploy.sh`):

   ```bash
   #!/bin/bash
   git pull origin main
   docker build -f docker/prod.Dockerfile -t budayanesia-prod .
   docker stop budayanesia || true
   docker rm budayanesia || true
   docker run -d -p 80:80 --name budayanesia budayanesia-prod
   ```

## Development Phases

### Phase 1: Environment Setup (2 days)

- [ ] Initialize React project
- [ ] Configure Docker development environment
- [ ] Set up Tailwind CSS
- [ ] Create basic component structure

### Phase 2: Core Functionality (10 days)

- [ ] Implement Supabase integration
- [ ] Build gamification system (XP, streaks)
- [ ] Create lesson progression logic
- [ ] Develop sound effects system
- [ ] Implement donation feature

### Phase 3: Content Development (Ongoing)

- [ ] Javanese culture modules (5 modules)
- [ ] Sundanese culture modules
- [ ] Batak culture modules
- [ ] Minang culture modules

### Phase 4: Testing & Deployment (3 days)

- [ ] Cross-browser testing
- [ ] Lighthouse optimization
- [ ] PWA validation
- [ ] DigitalOcean deployment

## Content Guidelines

1. Structure per culture:
   - Pengenalan (Introduction)
   - Bahasa (Language)
   - Tradisi (Traditions)
   - Filosofi (Philosophy)
   - Kontemporer (Contemporary)

2. Media specifications:
   - Images: WebP format, <500KB
   - Sound effects: <100KB each
   - Videos: Not included in MVP

## Team Responsibilities

| Role | Tasks |
|------|-------|
| Frontend | Components, UI, PWA |
| Backend | Supabase, API, DB |
| Content | Research, writing, localization |
| DevOps | Docker, Deployment, CI/CD |
