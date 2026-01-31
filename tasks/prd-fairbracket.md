# PRD: FairBracket - Tournament Scheduling & Operations Platform

## Introduction

FairBracket is a web-based tournament operations platform that generates fair, constraint-aware schedules and brackets for any sport, format, or league structure. It combines advanced optimization algorithms (including simulated annealing) with real-world operational tooling to handle both player-pooled events and pre-formed team competitions.

The platform eliminates spreadsheet-driven chaos, reduces organizer workload, prevents fairness disputes, and allows tournaments to adapt dynamically when reality deviates from plan.

**Repository:** https://github.com/IQGLLC/FairBracket
**Domain:** fairbracket.com
**Deployment:** AWS (iqgllc account)

## Goals

- Generate schedules that organizers can defend, players can trust, and tournaments can run on time
- Support both player-pooled (draft-style) and pre-formed team tournaments
- Provide simulated annealing optimization for complex multi-constraint scheduling
- Deliver real-time tournament operations (live scoring, standings, bracket advancement)
- Enable fair sit-out rotation, teammate/opponent variety, and rest time equity
- Offer a SaaS multi-tenant platform with tiered monetization
- Deploy on AWS with production-grade infrastructure

## Technology Stack

- **Frontend:** React 18+ with TypeScript, Vite, TailwindCSS
- **Backend:** Node.js with TypeScript, Express/Fastify
- **Database:** PostgreSQL 15+ (RDS)
- **Optimization Service:** Python 3.11+ (for simulated annealing)
- **Infrastructure:** AWS (ECS Fargate, RDS, S3, CloudFront, Route 53)
- **IaC:** Terraform
- **CI/CD:** GitHub Actions

---

# Epic 1: Infrastructure & DevOps

## US-001: Initialize monorepo structure
**Description:** As a developer, I need a well-organized monorepo so that frontend, backend, and optimization services are manageable.

**Acceptance Criteria:**
- [ ] Create monorepo with `/apps/web`, `/apps/api`, `/apps/optimizer`, `/packages/shared`
- [ ] Configure pnpm workspaces
- [ ] Add root `package.json` with workspace scripts
- [ ] Add `.nvmrc` with Node 20 LTS
- [ ] Add `.python-version` with 3.11
- [ ] Typecheck passes
- [ ] ESLint configured and passes

---

## US-002: Configure TypeScript and shared types
**Description:** As a developer, I need shared TypeScript configuration and types so that all packages have consistent typing.

**Acceptance Criteria:**
- [ ] Create `/packages/shared` with common types
- [ ] Configure `tsconfig.base.json` at root
- [ ] Each app extends base config
- [ ] Export tournament, player, team, schedule types
- [ ] Typecheck passes across all packages

---

## US-003: Set up React frontend scaffold
**Description:** As a developer, I need a React frontend scaffold so that UI development can begin.

**Acceptance Criteria:**
- [ ] Initialize Vite + React + TypeScript in `/apps/web`
- [ ] Configure TailwindCSS with design system tokens
- [ ] Add React Router for client-side routing
- [ ] Create basic layout components (Header, Sidebar, Main)
- [ ] Add Tanstack Query for data fetching
- [ ] Typecheck passes
- [ ] Dev server runs without errors

---

## US-004: Set up Node.js API scaffold
**Description:** As a developer, I need a Node.js API scaffold so that backend development can begin.

**Acceptance Criteria:**
- [ ] Initialize Fastify + TypeScript in `/apps/api`
- [ ] Configure environment variable handling (dotenv)
- [ ] Add health check endpoint at `/health`
- [ ] Add structured logging (pino)
- [ ] Add request validation (zod)
- [ ] Typecheck passes
- [ ] API starts and responds to health check

---

## US-005: Set up Python optimization service scaffold
**Description:** As a developer, I need a Python optimization service so that simulated annealing can be developed.

**Acceptance Criteria:**
- [ ] Initialize Python project in `/apps/optimizer` with pyproject.toml
- [ ] Configure poetry or uv for dependency management
- [ ] Add FastAPI for HTTP interface
- [ ] Add health check endpoint at `/health`
- [ ] Add pytest configuration
- [ ] Type hints enabled (mypy passes)
- [ ] Service starts and responds to health check

---

## US-006: Configure Terraform for AWS infrastructure
**Description:** As a developer, I need Terraform configuration so that AWS infrastructure can be provisioned.

**Acceptance Criteria:**
- [ ] Create `/infra/terraform` directory structure
- [ ] Configure AWS provider for iqgllc account
- [ ] Create modules for: VPC, RDS, ECS, ALB, S3, CloudFront
- [ ] Configure remote state in S3 with DynamoDB locking
- [ ] Add `dev` and `prod` environment workspaces
- [ ] Terraform plan runs without errors

---

## US-007: Set up Route 53 and SSL for fairbracket.com
**Description:** As a developer, I need DNS and SSL configured so that the platform is accessible via fairbracket.com.

**Acceptance Criteria:**
- [ ] Create Route 53 hosted zone for fairbracket.com
- [ ] Request ACM certificate for `*.fairbracket.com` and `fairbracket.com`
- [ ] Configure DNS validation for certificate
- [ ] Output certificate ARN for use by ALB/CloudFront
- [ ] Terraform apply succeeds

---

## US-008: Provision RDS PostgreSQL database
**Description:** As a developer, I need a PostgreSQL database so that application data can be persisted.

**Acceptance Criteria:**
- [ ] Create RDS PostgreSQL 15 instance (db.t3.medium for dev)
- [ ] Configure in private subnet with security group
- [ ] Enable automated backups (7-day retention)
- [ ] Store credentials in AWS Secrets Manager
- [ ] Output connection string for API service
- [ ] Database is accessible from ECS tasks

---

## US-009: Configure ECS Fargate for API and Optimizer
**Description:** As a developer, I need ECS Fargate configuration so that backend services can be deployed.

**Acceptance Criteria:**
- [ ] Create ECS cluster for FairBracket
- [ ] Define task definitions for API and Optimizer services
- [ ] Configure ALB with path-based routing (`/api/*`, `/optimizer/*`)
- [ ] Set up auto-scaling policies
- [ ] Configure CloudWatch logging
- [ ] Services deploy and respond to health checks

---

## US-010: Configure CloudFront and S3 for frontend
**Description:** As a developer, I need CloudFront and S3 so that the React frontend can be deployed globally.

**Acceptance Criteria:**
- [ ] Create S3 bucket for static assets
- [ ] Configure CloudFront distribution with S3 origin
- [ ] Set up custom domain (fairbracket.com, www.fairbracket.com)
- [ ] Configure cache behaviors for static assets
- [ ] Enable HTTPS redirect
- [ ] Frontend is accessible via fairbracket.com

---

## US-011: Set up GitHub Actions CI/CD pipeline
**Description:** As a developer, I need CI/CD pipelines so that code changes are automatically tested and deployed.

**Acceptance Criteria:**
- [ ] Create `.github/workflows/ci.yml` for PR checks
- [ ] Run typecheck, lint, tests on all apps
- [ ] Create `.github/workflows/deploy-dev.yml` for dev branch
- [ ] Create `.github/workflows/deploy-prod.yml` for main branch
- [ ] Build and push Docker images to ECR
- [ ] Deploy to ECS and sync frontend to S3
- [ ] Pipeline runs successfully on push

---

# Epic 2: Core Data Model

## US-012: Design and implement database schema
**Description:** As a developer, I need a database schema so that tournament data can be persisted correctly.

**Acceptance Criteria:**
- [ ] Create migration system (using Prisma or Drizzle)
- [ ] Define tables: organizations, users, tournaments, teams, players, venues, courts, time_slots
- [ ] Define tables: games, rounds, pools, brackets, scores
- [ ] Add proper indexes and foreign keys
- [ ] Add multi-tenant organization_id to all relevant tables
- [ ] Migrations run successfully
- [ ] Typecheck passes

---

## US-013: Implement organization and user models
**Description:** As a developer, I need organization and user models so that multi-tenancy works correctly.

**Acceptance Criteria:**
- [ ] Organizations have: id, name, slug, subscription_tier, created_at
- [ ] Users have: id, email, name, password_hash, organization_id, role
- [ ] Users belong to one organization
- [ ] Role is enum: owner, admin, organizer, referee, captain, player
- [ ] Add unique constraints on email and org slug
- [ ] Typecheck passes

---

## US-014: Implement tournament model
**Description:** As a developer, I need a tournament model so that tournament configuration is stored.

**Acceptance Criteria:**
- [ ] Tournament has: id, organization_id, name, slug, sport, format, status
- [ ] Tournament has: tournament_type (player_pooled | team_based)
- [ ] Tournament has: start_date, end_date, timezone
- [ ] Tournament has: settings JSONB for format-specific config
- [ ] Status enum: draft, published, in_progress, completed, cancelled
- [ ] Typecheck passes

---

## US-015: Implement venue and court models
**Description:** As a developer, I need venue and court models so that facilities can be configured.

**Acceptance Criteria:**
- [ ] Venue has: id, organization_id, name, address, timezone
- [ ] Court has: id, venue_id, name, surface_type, indoor/outdoor flag
- [ ] Court has: available boolean, notes
- [ ] Time_slot has: id, court_id, start_time, end_time, is_blocked
- [ ] Courts can be grouped by tags
- [ ] Typecheck passes

---

## US-016: Implement team and player models
**Description:** As a developer, I need team and player models so that participants can be managed.

**Acceptance Criteria:**
- [ ] Player has: id, organization_id, name, email, skill_rating, gender
- [ ] Player has: phone, emergency_contact (optional)
- [ ] Team has: id, tournament_id, name, seed, pool_id (nullable)
- [ ] Team_player junction: team_id, player_id, is_captain, jersey_number
- [ ] For player-pooled: players are assigned to teams; for team-based: teams register with roster
- [ ] Typecheck passes

---

## US-017: Implement game and round models
**Description:** As a developer, I need game and round models so that schedules can be stored.

**Acceptance Criteria:**
- [ ] Round has: id, tournament_id, round_number, round_type (pool, bracket, swiss)
- [ ] Game has: id, round_id, court_id, time_slot_id, home_team_id, away_team_id
- [ ] Game has: status (scheduled, in_progress, completed, cancelled)
- [ ] Game has: home_score, away_score, winner_id
- [ ] Game has: scheduled_start, actual_start, actual_end
- [ ] Typecheck passes

---

## US-018: Implement pool and bracket models
**Description:** As a developer, I need pool and bracket models so that tournament structures are stored.

**Acceptance Criteria:**
- [ ] Pool has: id, tournament_id, name, teams (relation)
- [ ] Bracket has: id, tournament_id, type (single_elim, double_elim)
- [ ] Bracket_slot has: id, bracket_id, position, team_id (nullable), source_game_id
- [ ] Bracket_slot has: round_number, match_number
- [ ] Support winner/loser bracket for double elimination
- [ ] Typecheck passes

---

## US-019: Implement optimization weights model
**Description:** As a developer, I need an optimization weights model so that fairness preferences are configurable.

**Acceptance Criteria:**
- [ ] Optimization_config has: id, tournament_id
- [ ] Weights: skill_balance, seed_balance, teammate_repetition_penalty
- [ ] Weights: opponent_repetition_penalty, sit_out_equity, rest_time_variance
- [ ] Weights: court_utilization, gender_balance
- [ ] All weights are floats 0.0-1.0 with sensible defaults
- [ ] Typecheck passes

---

# Epic 3: Authentication & Authorization

## US-020: Implement user registration and login
**Description:** As a user, I want to register and log in so that I can access the platform.

**Acceptance Criteria:**
- [ ] POST `/api/auth/register` creates user and organization
- [ ] POST `/api/auth/login` returns JWT tokens (access + refresh)
- [ ] Passwords are hashed with bcrypt (cost 12)
- [ ] Access token expires in 15 minutes, refresh in 7 days
- [ ] Email validation required before full access
- [ ] Typecheck passes

---

## US-021: Implement JWT middleware and protected routes
**Description:** As a developer, I need JWT middleware so that routes can be protected.

**Acceptance Criteria:**
- [ ] Middleware validates JWT on protected routes
- [ ] Extracts user_id and organization_id from token
- [ ] Returns 401 for missing/invalid tokens
- [ ] Returns 403 for insufficient permissions
- [ ] Add user context to request object
- [ ] Typecheck passes

---

## US-022: Implement role-based access control
**Description:** As an organizer, I need role-based permissions so that users have appropriate access.

**Acceptance Criteria:**
- [ ] Define permission matrix for all roles
- [ ] Owner: full access to organization
- [ ] Admin: manage tournaments, users, venues
- [ ] Organizer: manage assigned tournaments
- [ ] Referee: score games
- [ ] Captain: view team, submit scores
- [ ] Player: read-only access
- [ ] Typecheck passes

---

## US-023: Implement organization invitations
**Description:** As an admin, I want to invite users to my organization so that my team can collaborate.

**Acceptance Criteria:**
- [ ] POST `/api/org/invitations` creates invitation with role
- [ ] Sends email with invitation link
- [ ] Invitation expires in 7 days
- [ ] Accept invitation links user to organization
- [ ] List and revoke pending invitations
- [ ] Typecheck passes

---

# Epic 4: Tournament Configuration

## US-024: Create tournament wizard - basic info
**Description:** As an organizer, I want to create a tournament with basic info so that I can start configuration.

**Acceptance Criteria:**
- [ ] Form fields: name, sport, dates, timezone, tournament_type
- [ ] Tournament type selection: player_pooled or team_based
- [ ] Sport is free text with autocomplete suggestions
- [ ] Validate date range (end >= start)
- [ ] Save creates tournament in draft status
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-025: Create tournament wizard - format selection
**Description:** As an organizer, I want to select tournament format so that the structure is defined.

**Acceptance Criteria:**
- [ ] Format options: round_robin, pool_play, pool_to_bracket, single_elim, double_elim, swiss
- [ ] Show format descriptions and diagrams
- [ ] Pool play config: number of pools, teams per pool, games per pool matchup
- [ ] Bracket config: seeding method (manual, random, by_record)
- [ ] Swiss config: number of rounds, pairing method
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-026: Create tournament wizard - venue and courts
**Description:** As an organizer, I want to configure venues and courts so that games can be scheduled.

**Acceptance Criteria:**
- [ ] Select existing venue or create new
- [ ] Add/remove courts with names and attributes
- [ ] Define time slots with start/end times
- [ ] Support multiple days with different slots
- [ ] Mark blackout periods (lunch, setup, etc.)
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-027: Create tournament wizard - rules and constraints
**Description:** As an organizer, I want to configure rules so that the schedule respects real-world constraints.

**Acceptance Criteria:**
- [ ] Minimum rest time between games (minutes)
- [ ] Maximum games per team per day
- [ ] Gender composition rules (for player-pooled)
- [ ] Team size (min/max players per team)
- [ ] Referee assignment rules (optional)
- [ ] Custom rules as key-value pairs
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-028: Create tournament wizard - optimization weights
**Description:** As an organizer, I want to set optimization weights so that fairness priorities are clear.

**Acceptance Criteria:**
- [ ] Slider controls for each weight (0-100 scale, maps to 0.0-1.0)
- [ ] Presets: "Balanced", "Maximum Fairness", "Fast Schedule", "Custom"
- [ ] Show explanations for each weight
- [ ] Preview estimated optimization time
- [ ] Save configuration with tournament
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-029: Tournament review and publish
**Description:** As an organizer, I want to review and publish my tournament so that it becomes active.

**Acceptance Criteria:**
- [ ] Review page shows all configuration summary
- [ ] Validation checks (has courts, has time slots, etc.)
- [ ] Publish button changes status to "published"
- [ ] Published tournament has public URL
- [ ] Cannot modify critical settings after publish without warning
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

# Epic 5: Player-Pooled Tournament Support

## US-030: Player registration for pooled tournaments
**Description:** As a player, I want to register for a pooled tournament so that I can be assigned to a team.

**Acceptance Criteria:**
- [ ] Public registration page at `/t/{slug}/register`
- [ ] Collect: name, email, phone, skill self-rating, gender
- [ ] Skill rating: beginner, intermediate, advanced, expert (or 1-10)
- [ ] Optional: teammate preferences, availability constraints
- [ ] Confirmation email sent
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-031: Player management dashboard
**Description:** As an organizer, I want to manage registered players so that I can prepare for team formation.

**Acceptance Criteria:**
- [ ] List all registered players with filters (skill, gender, status)
- [ ] Edit player details
- [ ] Adjust skill ratings
- [ ] Mark players as confirmed, waitlisted, or cancelled
- [ ] Import players from CSV
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-032: Team formation algorithm
**Description:** As an organizer, I want teams auto-formed fairly so that skill and gender balance is achieved.

**Acceptance Criteria:**
- [ ] Algorithm inputs: players list, team size, gender rules, skill distribution goal
- [ ] Distributes skill ratings evenly across teams
- [ ] Respects gender composition rules (e.g., min 2 women per team)
- [ ] Considers teammate preferences when possible
- [ ] Returns team assignments with balance metrics
- [ ] Typecheck passes

---

## US-033: Team formation UI
**Description:** As an organizer, I want to review and adjust auto-formed teams so that I can make manual corrections.

**Acceptance Criteria:**
- [ ] Generate teams button calls formation algorithm
- [ ] Display teams with player cards showing skill/gender
- [ ] Drag-and-drop to move players between teams
- [ ] Show team balance scores (skill variance, gender count)
- [ ] Save/lock teams when satisfied
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-034: Sit-out tracking and rotation
**Description:** As an organizer, I want sit-outs tracked fairly so that all players get equal playing time.

**Acceptance Criteria:**
- [ ] Track sit-out count per player per round
- [ ] Optimization penalizes unequal sit-outs
- [ ] Dashboard shows sit-out distribution
- [ ] Alert if sit-out variance exceeds threshold
- [ ] Support substitute player management
- [ ] Typecheck passes

---

# Epic 6: Team-Based Tournament Support

## US-035: Team registration for team-based tournaments
**Description:** As a team captain, I want to register my team so that we can participate.

**Acceptance Criteria:**
- [ ] Public registration page at `/t/{slug}/register`
- [ ] Collect: team name, captain info, roster (names)
- [ ] Optional: team logo upload, seed preference
- [ ] Roster minimum/maximum validation
- [ ] Confirmation email to captain
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-036: Team management dashboard
**Description:** As an organizer, I want to manage registered teams so that I can finalize the participant list.

**Acceptance Criteria:**
- [ ] List all teams with status (registered, confirmed, waitlisted)
- [ ] View/edit team details and roster
- [ ] Set team seed (manual seeding)
- [ ] Approve/reject team registrations
- [ ] Import teams from CSV
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-037: Pool assignment for team tournaments
**Description:** As an organizer, I want to assign teams to pools so that pool play can be scheduled.

**Acceptance Criteria:**
- [ ] Auto-generate pool assignments based on seeds
- [ ] Snake draft seeding (1,4,5,8 in pool A; 2,3,6,7 in pool B)
- [ ] Manual drag-and-drop adjustments
- [ ] Show pool balance metrics
- [ ] Lock pools when finalized
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-038: Bracket seeding and generation
**Description:** As an organizer, I want to seed teams into brackets so that elimination rounds are fair.

**Acceptance Criteria:**
- [ ] Auto-seed from pool play results (top N from each pool)
- [ ] Manual seed override option
- [ ] Generate single or double elimination bracket
- [ ] Standard bracket positioning (1v8, 4v5, etc.)
- [ ] Visualize bracket structure
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

# Epic 7: Scheduling Engine

## US-039: Deterministic schedule generator
**Description:** As an organizer, I want fast schedule generation for simple tournaments so that I don't wait for optimization.

**Acceptance Criteria:**
- [ ] Round robin: standard rotation algorithm
- [ ] Pool play: balanced home/away within pools
- [ ] Bracket: sequential scheduling with rest time
- [ ] Respects court availability and time slots
- [ ] Completes in < 1 second for small tournaments
- [ ] Typecheck passes

---

## US-040: Schedule conflict detection
**Description:** As a developer, I need conflict detection so that invalid schedules are prevented.

**Acceptance Criteria:**
- [ ] Detect team playing two games at same time
- [ ] Detect court double-booked
- [ ] Detect insufficient rest time violations
- [ ] Detect games outside time slot bounds
- [ ] Return list of conflicts with details
- [ ] Typecheck passes

---

## US-041: Simulated annealing optimizer - core algorithm
**Description:** As a developer, I need the SA optimizer core so that complex schedules can be optimized.

**Acceptance Criteria:**
- [ ] Implement in Python optimization service
- [ ] Accept schedule and optimization config as input
- [ ] Calculate multi-objective cost function
- [ ] Implement temperature decay schedule
- [ ] Implement neighbor generation (swap games, move time slots)
- [ ] Return best schedule with cost breakdown
- [ ] Typecheck passes (mypy)

---

## US-042: Simulated annealing - cost functions
**Description:** As a developer, I need cost functions so that schedule quality is measurable.

**Acceptance Criteria:**
- [ ] Skill balance cost (for player-pooled)
- [ ] Seed balance cost (matchup fairness)
- [ ] Teammate repetition penalty
- [ ] Opponent repetition penalty
- [ ] Sit-out equity variance
- [ ] Rest time variance
- [ ] Court utilization efficiency
- [ ] All costs normalized to 0.0-1.0 range
- [ ] Typecheck passes

---

## US-043: Simulated annealing - configuration and tuning
**Description:** As a developer, I need SA configuration so that optimization is tunable per tournament size.

**Acceptance Criteria:**
- [ ] Configurable initial temperature
- [ ] Configurable cooling rate (linear, exponential, adaptive)
- [ ] Early stopping when improvement plateaus
- [ ] Max iterations limit
- [ ] Deterministic seed option for reproducibility
- [ ] Log optimization progress
- [ ] Typecheck passes

---

## US-044: Schedule generation API endpoint
**Description:** As a frontend, I need a schedule generation API so that organizers can trigger optimization.

**Acceptance Criteria:**
- [ ] POST `/api/tournaments/{id}/generate-schedule`
- [ ] Accept mode parameter: "fast" (deterministic) or "optimize" (SA)
- [ ] Return job ID for async processing
- [ ] GET `/api/tournaments/{id}/schedule-job/{jobId}` for status
- [ ] WebSocket updates for progress
- [ ] Return schedule with fairness metrics when complete
- [ ] Typecheck passes

---

## US-045: Schedule generation UI
**Description:** As an organizer, I want to generate and view schedules so that I can review before publishing.

**Acceptance Criteria:**
- [ ] Generate Schedule button with mode selection
- [ ] Progress indicator during optimization
- [ ] Display schedule in grid (courts x time slots)
- [ ] Show fairness score breakdown
- [ ] Highlight any constraint violations
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-046: Schedule explanation feature
**Description:** As an organizer, I want to understand why a schedule was generated so that I can justify it to teams.

**Acceptance Criteria:**
- [ ] "Why this schedule?" button
- [ ] Display optimization weights used
- [ ] Show cost breakdown per constraint
- [ ] Highlight trade-offs made
- [ ] Exportable as PDF for sharing
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

# Epic 8: Live Tournament Operations

## US-047: Public tournament overview page
**Description:** As a visitor, I want to see tournament overview so that I understand the event.

**Acceptance Criteria:**
- [ ] Public page at `/{org-slug}/{tournament-slug}`
- [ ] Show tournament name, dates, venue, format
- [ ] Display current status (upcoming, in progress, completed)
- [ ] Links to schedule, standings, bracket
- [ ] No login required
- [ ] Mobile responsive
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-048: Public schedule view
**Description:** As a visitor, I want to see the tournament schedule so that I know when games are played.

**Acceptance Criteria:**
- [ ] View by: full schedule, by round, by court, by team
- [ ] Show game time, court, teams, status
- [ ] Highlight current/next games
- [ ] Filter and search functionality
- [ ] Printable view
- [ ] Mobile responsive
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-049: Public standings view
**Description:** As a visitor, I want to see standings so that I know team rankings.

**Acceptance Criteria:**
- [ ] Show standings by pool (for pool play)
- [ ] Display: rank, team, W-L, points for/against, differential
- [ ] Tiebreaker explanation
- [ ] Update in real-time as scores are entered
- [ ] Mobile responsive
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-050: Public bracket view
**Description:** As a visitor, I want to see the bracket so that I can follow elimination rounds.

**Acceptance Criteria:**
- [ ] Visual bracket display (single and double elimination)
- [ ] Show completed game scores
- [ ] Highlight upcoming games
- [ ] Show advancement path
- [ ] Zoomable and scrollable on mobile
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-051: Team detail page
**Description:** As a visitor, I want to see team details so that I can follow my team.

**Acceptance Criteria:**
- [ ] Public page at `/{org-slug}/{tournament-slug}/teams/{team-id}`
- [ ] Show team name, roster, record
- [ ] List all games (past and upcoming)
- [ ] Show scores for completed games
- [ ] Shareable URL
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-052: Organizer dashboard - tournament control
**Description:** As an organizer, I want a dashboard so that I can manage the live tournament.

**Acceptance Criteria:**
- [ ] Overview: games in progress, upcoming, recently completed
- [ ] Quick actions: start game, end game, enter score
- [ ] Status indicators for schedule adherence
- [ ] Notifications for scoring conflicts
- [ ] Refresh with real-time updates
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-053: Drag-and-drop schedule adjustments
**Description:** As an organizer, I want to drag-and-drop adjust schedules so that I can handle real-time changes.

**Acceptance Criteria:**
- [ ] Drag games to different time slots
- [ ] Drag games to different courts
- [ ] Swap two games
- [ ] Conflict detection on drop
- [ ] Undo/redo functionality
- [ ] Audit log of changes
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-054: Lock and re-optimize feature
**Description:** As an organizer, I want to lock played rounds and re-optimize remaining so that the schedule adapts.

**Acceptance Criteria:**
- [ ] Lock individual games or entire rounds
- [ ] Re-optimize button respects locks
- [ ] Preview changes before applying
- [ ] Show what changed from previous schedule
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-055: Manual override with audit trail
**Description:** As an organizer, I want to make manual overrides tracked so that changes are documented.

**Acceptance Criteria:**
- [ ] Override game result (for scoring errors)
- [ ] Override team advancement (bracket)
- [ ] Require reason text for overrides
- [ ] Store who made override and when
- [ ] Display override history
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

# Epic 9: Scoring System

## US-056: Score entry for organizers
**Description:** As an organizer, I want to enter scores so that standings are updated.

**Acceptance Criteria:**
- [ ] Select game from schedule
- [ ] Enter scores for each team
- [ ] Mark game as final
- [ ] Auto-update standings and bracket advancement
- [ ] Validate score format
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-057: Referee scoring interface
**Description:** As a referee, I want a simple scoring interface so that I can enter results quickly.

**Acceptance Criteria:**
- [ ] Mobile-optimized interface
- [ ] Show only games assigned to me
- [ ] Large touch targets for score buttons
- [ ] Start/pause/end game controls
- [ ] Submit score with one tap
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-058: Captain score submission
**Description:** As a captain, I want to submit scores so that games without referees are recorded.

**Acceptance Criteria:**
- [ ] Captain-only access to team's games
- [ ] Submit score for completed games
- [ ] Require both captains to confirm (optional setting)
- [ ] Dispute flag if scores don't match
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-059: Score conflict resolution
**Description:** As an organizer, I want to resolve score conflicts so that disputes are handled.

**Acceptance Criteria:**
- [ ] Alert when captain scores conflict
- [ ] Show both submitted scores
- [ ] Organizer selects correct score
- [ ] Record resolution in audit log
- [ ] Notify captains of resolution
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-060: Multi-set/period scoring
**Description:** As an organizer, I want to configure scoring format so that different sports are supported.

**Acceptance Criteria:**
- [ ] Configure: single score, sets (volleyball), periods (hockey), quarters (basketball)
- [ ] Enter scores per set/period
- [ ] Calculate winner based on format rules
- [ ] Display set scores in public views
- [ ] Typecheck passes

---

# Epic 10: Notifications

## US-061: Email notification system
**Description:** As a user, I want email notifications so that I stay informed about the tournament.

**Acceptance Criteria:**
- [ ] Transactional emails: registration confirmation, schedule published, game reminder
- [ ] Configure notification preferences
- [ ] Use SES for email delivery
- [ ] Track delivery and open rates
- [ ] Unsubscribe link in all emails
- [ ] Typecheck passes

---

## US-062: In-app notifications
**Description:** As a user, I want in-app notifications so that I see updates while using the platform.

**Acceptance Criteria:**
- [ ] Notification bell icon with badge count
- [ ] Dropdown list of recent notifications
- [ ] Mark as read/unread
- [ ] Click to navigate to relevant page
- [ ] Clear all functionality
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-063: Real-time updates via WebSocket
**Description:** As a user, I want real-time updates so that I see changes without refreshing.

**Acceptance Criteria:**
- [ ] WebSocket connection for authenticated users
- [ ] Broadcast score updates to tournament viewers
- [ ] Update standings and brackets in real-time
- [ ] Connection status indicator
- [ ] Graceful reconnection on disconnect
- [ ] Typecheck passes

---

# Epic 11: Monetization

## US-064: Subscription tier model
**Description:** As a business, we need subscription tiers so that we can monetize the platform.

**Acceptance Criteria:**
- [ ] Tiers: Free, Starter, Pro, Enterprise
- [ ] Free: 1 tournament/month, 16 teams max, basic features
- [ ] Starter: 5 tournaments/month, 32 teams, live scoring
- [ ] Pro: Unlimited tournaments, 128 teams, full optimization, API access
- [ ] Enterprise: White-label, custom limits, SLA
- [ ] Tier stored in organization model
- [ ] Typecheck passes

---

## US-065: Feature gating by tier
**Description:** As a developer, I need feature gating so that paid features are protected.

**Acceptance Criteria:**
- [ ] Check organization tier before feature access
- [ ] Gate: simulated annealing optimization (Pro+)
- [ ] Gate: live scoring (Starter+)
- [ ] Gate: API access (Pro+)
- [ ] Gate: notifications (Starter+)
- [ ] Show upgrade prompts for gated features
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-066: Stripe integration for payments
**Description:** As an organization owner, I want to subscribe via Stripe so that I can access paid features.

**Acceptance Criteria:**
- [ ] Stripe Checkout for subscription signup
- [ ] Support monthly and annual billing
- [ ] Handle subscription upgrades/downgrades
- [ ] Handle cancellations
- [ ] Webhook handlers for payment events
- [ ] Invoice history in account settings
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-067: Usage metering and limits
**Description:** As a business, I need usage metering so that limits are enforced.

**Acceptance Criteria:**
- [ ] Track tournaments created per month
- [ ] Track teams per tournament
- [ ] Enforce limits based on tier
- [ ] Show usage dashboard in account settings
- [ ] Warning at 80% of limit
- [ ] Block at 100% with upgrade prompt
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-068: Per-tournament pricing option
**Description:** As an organizer, I want to pay per-tournament so that I don't need a subscription.

**Acceptance Criteria:**
- [ ] One-time payment option at tournament creation
- [ ] Tiered by team count (8/16/32/64/128+)
- [ ] Unlocks all features for that tournament
- [ ] Payment via Stripe Checkout
- [ ] Receipt and invoice provided
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

# Epic 12: Admin & Analytics

## US-069: Organization settings page
**Description:** As an owner, I want organization settings so that I can configure my account.

**Acceptance Criteria:**
- [ ] Edit organization name, logo
- [ ] View current subscription tier and usage
- [ ] Manage payment methods
- [ ] View team members and roles
- [ ] Download invoices
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-070: User management for organization
**Description:** As an owner, I want to manage users so that I control access.

**Acceptance Criteria:**
- [ ] List all organization members
- [ ] Change user roles
- [ ] Remove users from organization
- [ ] View last login and activity
- [ ] Transfer ownership
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-071: Tournament analytics dashboard
**Description:** As an organizer, I want tournament analytics so that I can measure success.

**Acceptance Criteria:**
- [ ] Registration conversion rate
- [ ] Schedule adherence (on-time starts)
- [ ] Fairness metrics achieved
- [ ] Scoring completion rate
- [ ] Public page views
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

---

## US-072: Platform admin dashboard (internal)
**Description:** As a platform admin, I need an admin dashboard so that I can manage the SaaS.

**Acceptance Criteria:**
- [ ] View all organizations
- [ ] View platform-wide metrics (tournaments, users, revenue)
- [ ] Impersonate user for support
- [ ] Feature flag management
- [ ] Manual tier override
- [ ] Typecheck passes

---

---

# Functional Requirements

## Infrastructure
- FR-1: The system must deploy to AWS ECS Fargate for backend services
- FR-2: The system must use RDS PostgreSQL for data persistence
- FR-3: The system must use CloudFront + S3 for frontend hosting
- FR-4: The system must use Route 53 for DNS with fairbracket.com
- FR-5: The system must support HTTPS via ACM certificates
- FR-6: The system must use GitHub Actions for CI/CD

## Authentication & Authorization
- FR-7: The system must use JWT for stateless authentication
- FR-8: The system must support multi-tenant organizations
- FR-9: The system must enforce role-based access control
- FR-10: The system must hash passwords with bcrypt

## Tournament Models
- FR-11: The system must support player-pooled tournaments with dynamic team formation
- FR-12: The system must support pre-formed team tournaments with fixed rosters
- FR-13: The system must support formats: round robin, pool play, pool→bracket, single elimination, double elimination, swiss

## Scheduling & Optimization
- FR-14: The system must provide deterministic scheduling for simple tournaments
- FR-15: The system must provide simulated annealing optimization for complex tournaments
- FR-16: The system must allow configurable optimization weights
- FR-17: The system must detect and prevent schedule conflicts
- FR-18: The system must support schedule adjustments during live tournaments
- FR-19: The system must track fairness metrics (sit-outs, rest time, opponent variety)

## Facilities
- FR-20: The system must model venues, courts, and time slots
- FR-21: The system must support blackout periods
- FR-22: The system must optimize court utilization

## Live Operations
- FR-23: The system must provide public web pages without login
- FR-24: The system must support real-time score entry
- FR-25: The system must auto-advance brackets based on results
- FR-26: The system must provide drag-and-drop schedule adjustments
- FR-27: The system must maintain audit trail for manual overrides

## Monetization
- FR-28: The system must support subscription tiers with feature gating
- FR-29: The system must integrate with Stripe for payments
- FR-30: The system must enforce usage limits by tier

---

# Non-Goals (Out of Scope)

- Native mobile apps (web-first, responsive design only)
- Fantasy sports or betting features
- Social networking features
- Video streaming or live game broadcast
- Equipment or merchandise sales
- Generic calendar or event management
- Single-sport optimization (must remain sport-agnostic)
- Offline mode (requires internet connection)
- White-label mobile apps
- Integration with specific league management systems (generic API only)

---

# Design Considerations

- **Mobile-first responsive design** for public pages
- **Desktop-optimized** for organizer dashboard
- **Accessible** (WCAG 2.1 AA compliance)
- **Dark mode** support
- **Printable views** for schedules and brackets
- **Shareable URLs** for all public content

---

# Technical Considerations

- **Optimization service** runs as separate Python microservice for CPU isolation
- **WebSocket** for real-time updates (score changes, schedule updates)
- **Background jobs** for email, notifications, and async optimization
- **Database versioning** with migrations for safe schema changes
- **Multi-tenant data isolation** at query level (organization_id on all tables)
- **Rate limiting** on public APIs
- **Caching** for public pages (standings, brackets)

---

# Success Metrics

- Organizers can create and publish a tournament in under 30 minutes
- Schedule optimization completes within 60 seconds for tournaments up to 32 teams
- Public pages load in under 2 seconds on 3G connection
- Zero fairness complaints from schedule decisions (explainable optimization)
- 90%+ of games start within 5 minutes of scheduled time
- Customer subscription retention rate > 80%

---

# Open Questions

1. Should we integrate with third-party registration/payment platforms (e.g., Eventbrite)?
2. What is the maximum tournament size we need to support initially (64? 128? 256 teams)?
3. Should the optimizer support multi-day tournaments with different constraints per day?
4. Do we need support for referee payments/tracking?
5. Should standings/brackets embed in external websites (iframe/widget)?
6. What SLA is expected for Enterprise tier?
