# FairBracket

Web-Based Tournament Scheduling & Operations Platform

## Overview

FairBracket generates fair, constraint-aware schedules and brackets for any sport, format, or league structure. It combines advanced optimization algorithms (including simulated annealing) with real-world operational tooling.

## Features

- **Player-Pooled Tournaments** - Dynamic team formation with skill balancing
- **Team-Based Tournaments** - Traditional brackets, pools, and seeding
- **Simulated Annealing Optimization** - Multi-objective fairness optimization
- **Live Tournament Operations** - Real-time scoring, standings, and bracket advancement
- **Public Web Pages** - No-login access for spectators

## Technology Stack

- **Frontend:** React 18+ with TypeScript, Vite, TailwindCSS
- **Backend:** Node.js with TypeScript, Fastify
- **Optimization:** Python 3.11+ with FastAPI
- **Database:** PostgreSQL 15+
- **Infrastructure:** AWS (ECS Fargate, RDS, CloudFront)

## Project Structure

```
/apps
  /web        # React frontend
  /api        # Node.js API
  /optimizer  # Python optimization service
/packages
  /shared     # Shared TypeScript types
/infra
  /terraform  # Infrastructure as Code
```

## Getting Started

See the [PRD](./tasks/prd-fairbracket.md) for detailed requirements.

## License

Proprietary - IQGLLC
