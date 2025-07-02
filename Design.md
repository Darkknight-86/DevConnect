# Sprint Architecture & Development Plan

## Project Structure

### Frontend (SvelteKit)
```
frontend/
├── src/
│   ├── routes/
│   │   ├── +layout.svelte       # Main layout
│   │   ├── +page.svelte         # Home
│   │   ├── login/               # Local + OAuth
│   │   │   └── +page.svelte     # Login/Register + OAuth buttons
│   │   ├── profile/             # Profile pages
│   │   ├── search/              # Search/results
│   │   ├── details/             # Details pages
│   │   └── agents/              # MCP agent interfaces
│   │       └── +page.svelte     # Agent chat & orchestration
│   ├── components/              # Reusable UI components
│   │   ├── SearchBar.svelte
│   │   ├── ContentCard.svelte
│   │   ├── AgentChat.svelte
│   │   ├── AgentSummary.svelte
│   │   └── AgentRecommendations.svelte
│   └── lib/
│       ├── api.ts               # HTTP wrapper for all /api calls
│       └── stores.ts            # Svelte stores (auth, global state)
└── svelte.config.js             # SvelteKit config
```

### Backend (Deno + Oak)
```
backend/
├── deps.ts                      # Centralized imports
├── main.ts                      # Server bootstrap
├── routes/
│   ├── auth.ts                  # Local & OAuth endpoints
│   ├── search.ts                # /api/search
│   ├── details.ts               # /api/details/:source/:id
│   ├── content.ts               # /api/content/bookmark, /comment
│   └── agents.ts                # /api/agents/* (MCP endpoints)
├── controllers/
│   ├── AuthController.ts        # login/register + OAuth handlers
│   ├── SearchController.ts
│   ├── DetailsController.ts
│   ├── ContentController.ts
│   └── AgentController.ts       # MCP agent orchestration
├── services/
│   ├── OAuthService.ts          # Code exchange & profile fetch
│   ├── GitHubService.ts         # Content fetch
│   ├── YouTubeService.ts        # Content fetch
│   ├── ContentService.ts        # DB CRUD for bookmarks/comments
│   └── agents/                  # MCP agent services
│       ├── AgentOrchestrator.ts # Agent workflow coordination
│       ├── GitHubAgent.ts       # GitHub content analysis
│       ├── YouTubeAgent.ts      # YouTube metadata analysis
│       └── RecommendationAgent.ts # Multi-source recommendations
├── models/
│   ├── User.ts                  # + githubId?, googleId?, bookmarks[]
│   ├── Bookmark.ts              # Bookmark schema
│   └── Comment.ts               # Comment schema
├── middleware/
│   ├── authMiddleware.ts        # JWT validation
│   └── errorHandler.ts          # Global error handler
└── config.ts                    # Env vars (JWT_SECRET, API keys, MCP config)
```

## Sprint Development Plan

### Sprint 1: Core MVP (Assignment Requirements)
**Goal: Build a complete, assignment-compliant web application**

#### Database Models & Relationships
**User Model**
- Username, email, password hash
- Role field: "member" or "curator"
- Profile information (avatar, bio)
- Following/followers arrays (many-to-many relationship)

**Bookmark Model**
- User reference (one-to-many: User -> Bookmarks)
- Content source and ID
- Metadata (title, description, URL)

**Comment Model**
- User and content references
- Comment text and timestamps

#### Core Pages (All 6 Required)
- **Home (/)**: Dynamic content based on user role and login status
- **Login/Register (/login, /register)**: Local authentication with role selection
- **Profile (/profile, /profile/:id)**: Personal info management with privacy controls
- **Search (/search)**: GitHub repository search with live API data
- **Details (/details/github/:id)**: Repository details with user comments/bookmarks

#### Authentication System
- Local username/password authentication
- JWT tokens in HTTP-only cookies
- Role-based middleware protection
- UI adaptation based on user role

#### GitHub API Integration
[Github API Docs](https://docs.github.com/en/rest/search/search?apiVersion=2022-11-28)
- Repository search functionality
- Repository details retrieval
- Rate limiting and error handling

#### User Features
- User registration and login
- Content bookmarking
- User following/followers
- Basic commenting system
- Role-based UI differences (member vs curator)

#### Documentation
- Create UML's for each sprint
- Update Design Doc
- Create Feature explanation Doc

#### API Endpoints (Sprint 1)
```
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET /api/search?source=github&q=term
GET /api/details/github/:id
POST /api/content/bookmark
POST /api/content/comment
GET /api/users/:id/profile
POST /api/users/:id/follow
```

### Sprint 2: Enhanced API & Features
**Goal: Expand content sources and improve user experience**

#### YouTube API Integration
[YouTube API Docs](https://developers.google.com/youtube/v3/docs)
- Video search functionality
- Video details and metadata
- Unified search interface for both GitHub and YouTube

#### Enhanced User Features
- User activity feeds
- Advanced profile features
- Content recommendations (basic algorithm)
- Search filters and sorting

#### UI/UX Improvements
- Responsive design refinement
- Loading states and error handling
- Search result pagination
- Better mobile experience

### Sprint 3: OAuth & Social Features
**Goal: Add OAuth authentication and advanced social features**

#### OAuth Integration
- GitHub OAuth flow
- Google OAuth flow
- Account linking for existing users

#### Advanced Social Features
- User discovery and search
- Content sharing capabilities
- Activity notifications
- Enhanced following/follower features

#### Performance & Security
- API caching strategies
- Rate limiting implementation
- Security hardening

### Sprint 4: AI Integration (Stretch Goal)
**Goal: Implement MCP-based AI agent system**

#### MCP Agent Framework
- GitHub content analysis agent
- YouTube metadata analysis agent
- User behavior analysis agent
- Recommendation synthesis agent

#### Agent Orchestration
- Multi-agent workflow coordination
- Context sharing between agents
- Tool-calling capabilities for data access

#### AI-Enhanced UI Components
- Agent-powered summary panels
- Multi-source recommendation widgets
- Conversational interface with agent delegation

#### Advanced AI Features
- Cross-platform content correlation
- Personalized agent recommendations
- Natural language queries routed to appropriate agents

## User Roles & Capabilities

### Member (Basic User)
- Search and view content
- Bookmark repositories and videos
- Follow other users
- Comment on content
- Basic profile management

### Curator (Advanced User)
- All member capabilities
- Create and manage curated content lists
- Moderate comments and content
- Access to user analytics
- Advanced recommendation tools

## Technical Architecture

### Authentication Flow
- Local registration with role selection
- JWT-based session management
- Role-based route protection
- OAuth integration for external providers

### Content Integration
- Unified search interface across GitHub and YouTube APIs
- Local database augmentation of external content
- User-generated content (bookmarks, comments)

### AI Integration
- MCP (Model Context Protocol) agent framework
- Specialized agents for GitHub/YouTube content analysis
- Agent orchestration for intelligent recommendations
- Conversational AI interface with tool-calling capabilities

## Success Metrics

### Sprint 1 Completion Criteria
- [ ] All 6 required pages functional and responsive
- [ ] Two distinct user roles with different capabilities
- [ ] GitHub API integration working
- [ ] Database relationships implemented
- [ ] Local authentication system complete
- [ ] Professional styling and navigation

### Overall Project Goals
- Complete assignment requirements in Sprint 1
- Build scalable architecture for future enhancements
- Implement modern web development practices
- Create professional, portfolio-worthy application