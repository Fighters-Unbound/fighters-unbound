# Fighters Unbound

A Web3 fitness training platform that gamifies NFT fighter training through real-world activities. Train your NFT fighters by running (Strava), practicing yoga, or meditating. Fighter attributes are stored off-chain and dynamically generate OpenSea-compatible NFT metadata in real-time.

## 🎯 Overview

Fighters Unbound connects real-world fitness activities with NFT gaming. Users own NFT fighters that can be trained through:

- **🏃 Strava Running**: Sync your running activities to boost fighter endurance
- **🧘 Yoga Sessions**: Complete yoga courses to improve agility and mental strength
- **🧘 Meditation Sessions**: Practice meditation to enhance mental strength and leadership

The platform features an **AI-Powered Personal Training Coach** that provides personalized recommendations based on your goals, training history, and fighter needs.

## ✨ Key Features

### Training System
- **Real-World Activity Integration**: Connect Strava to sync running activities automatically
- **Interactive Training Courses**: Complete yoga and meditation sessions with quizzes
- **Dynamic Stat Tracking**: Fighter attributes update in real-time based on training
- **Training Cycles**: Monthly cycles with progressive stat caps for balanced progression
- **Daily Limits**: Fair training limits to ensure balanced gameplay

### AI Coach System
- **Personalized Recommendations**: AI analyzes your goals, history, and fighter stats
- **Two Coach Personalities**: Choose between "Butch" (direct, motivational) or "Dead" (calm, reflective)
- **Goal-Based Logic**: 7 different training goals with intelligent recommendation algorithms
- **Runner Level Detection**: Automatically classifies your fitness level and adjusts recommendations
- **Share Cards**: Generate beautiful shareable images with your coach's recommendations

### NFT Integration
- **Dynamic Metadata**: Real-time NFT metadata generation for OpenSea and Mintify
- **Off-Chain Stats**: All fighter stats stored off-chain in SQLite for fast updates
- **OpenSea Compatible**: Full compatibility with major NFT marketplaces
- **5,200+ Fighters**: Support for large-scale NFT collections

## 🏗️ Architecture

The platform uses a **microservices architecture** with three main components:

```
┌─────────────────────────────────────────────────────────────┐
│              Frontend (React/Next.js)                        │
│  - User Interface                                            │
│  - Wallet Connectivity (RainbowKit/wagmi)                    │
│  - Share Card Generation                                     │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTP/REST API
┌──────────────────────▼──────────────────────────────────────┐
│              Backend API (TypeScript/Express)               │
│  - Training Data Management                                 │
│  - Strava Integration                                       │
│  - Fighter Stat Tracking                                    │
│  - NFT Metadata Generation                                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│          AI Trainer Agent (Python/FastAPI)                  │
│  - LangGraph Workflow Orchestration                         │
│  - Recommendation Generation                                │
│  - Training Analysis                                        │
│  - Natural Language Generation (Ollama)                      │
└────┬──────────────┬──────────────┬──────────────────────────┘
     │              │              │
┌─────▼──────┐ ┌────▼──────┐ ┌────▼──────────┐
│  SQLite    │ │ ChromaDB   │ │   Ollama LLM  │
│  (Structured│ │ (Vector)   │ │  (Generation)│
│   Data)    │ │  - Actions  │ │              │
│            │ │  - Analysis │ │              │
│            │ │  - Knowledge│ │              │
└────────────┘ └─────────────┘ └──────────────┘
```

## 📦 Project Structure

```
unbound/
├── squad-unbound-games-frontend/     # React/Next.js frontend
├── squad-unbound-games-backend/      # TypeScript/Express backend
├── squad-unbound-games-trainer-agent/ # Python/FastAPI AI agent
└── README.md                          # This file
```

## 🚀 Getting Started

### Prerequisites

- **Node.js** v16 or higher
- **Python** 3.8 or higher
- **npm** or **yarn**
- **Strava API credentials** (optional, for running integration)
- **Ollama** (for AI coach - see trainer agent setup)

### 1. Frontend Setup

```bash
cd squad-unbound-games-frontend

# Install dependencies
npm install

# Configure environment variables
# Create .env.local file:
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id  # Optional but recommended
NEXT_PUBLIC_SHAPE_CHAIN_ID=360
NEXT_PUBLIC_SHAPE_RPC_URL=https://mainnet.shape.network

# Run development server
npm run dev
```

The frontend will be available at `http://localhost:3000`

### 2. Backend Setup

```bash
cd squad-unbound-games-backend

# Install dependencies
npm install

# Configure environment variables
# Create .env file:
PORT=3001
NODE_ENV=development
STRAVA_CLIENT_ID=your_client_id
STRAVA_CLIENT_SECRET=your_client_secret
DATABASE_PATH=./src/database/mydb-backup-2025-12-03_09-45-50.sqlite

# Run development server
npm run dev
```

The backend API will be available at `http://localhost:3001`

**Note**: The database is pre-populated with 5,200 fighters. Do NOT run `npm run init-db` as it will overwrite existing data.

### 3. AI Trainer Agent Setup

```bash
cd squad-unbound-games-trainer-agent

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On Linux/Mac:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Install and run Ollama (for AI generation)
# Visit https://ollama.ai to download Ollama
# Then pull the model:
ollama pull gemma:7b

# Configure environment variables
# Create .env file:
BACKEND_DB_PATH=../squad-unbound-games-backend/src/database/mydb-backup-2025-12-03_09-45-50.sqlite
VECTOR_DB_DIR=./data/vector_db
OLLAMA_MODEL=gemma:7b
OLLAMA_BASE_URL=http://localhost:11434
PORT=8000

# Initialize vector databases
python init_db.py

# Run the server
python main.py
```

The trainer agent will be available at `http://localhost:8000`

## 🎮 How to Use

### 1. Connect Your Wallet

1. Open the frontend application
2. Click "Connect Wallet" in the top right
3. Select your preferred wallet (MetaMask, WalletConnect, etc.)
4. Sign the authentication message (no gas fee required)

### 2. Set Up Your Profile

1. After connecting your wallet, you'll be prompted to set up your profile
2. Choose your coach personality:
   - **Butch**: Direct, motivational, action-oriented
   - **Dead**: Calm, reflective, supportive
3. Select 1-3 training goals (e.g., "improve endurance", "reduce stress")
4. Choose your preferred activities
5. Set your practice level

### 3. Link Your Fighters

1. If you own Fighters Unbound NFTs, they will be automatically detected
2. Your fighters will appear in the carousel
3. Each fighter shows current stats and training status

### 4. Connect Strava (Optional)

1. Go to your profile settings
2. Click "Connect Strava"
3. Authorize the application
4. Your running activities will automatically sync

### 5. Get Training Recommendations

1. Click "Get Recommendation" to receive personalized training advice
2. The AI coach analyzes:
   - Your goals
   - Training history
   - Fighter needs
   - Activity balance
3. Accept the recommendation to start a 12-hour training window
4. Complete the training to earn fight points and stat bonuses

### 6. Complete Training

#### Strava Running
1. Go for a run and record it in Strava
2. Return to the app and sync your activity
3. Fighter endurance increases based on distance
4. Daily limit: 20 syncs per athlete
5. Cycle limit: 20 km per fighter per month

#### Yoga Training
1. Select a yoga course
2. Watch the lesson
3. Answer the quiz questions
4. Earn agility and mental strength bonuses
5. Daily limit: 1 completion per fighter

#### Meditation Training
1. Select a meditation course
2. Complete the session
3. Answer the quiz questions
4. Earn mental strength and leadership bonuses
5. Daily limit: 1 completion per fighter

### 7. View Your Progress

- Check fighter stats in the carousel
- View training history
- Track your streak
- See fight points earned
- Monitor level progression

## 🔧 Configuration

### Frontend Configuration

**WalletConnect Project ID** (Recommended):
1. Visit https://cloud.reown.com
2. Create a project
3. Copy your Project ID
4. Add to `.env.local`: `NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_id`

**Shape Network** (Auto-configured):
- Automatically switches to Shape Network when wallet connects
- No additional configuration needed

### Backend Configuration

**Strava API**:
1. Create an app at https://www.strava.com/settings/api
2. Set redirect URI: `http://localhost:3001/strava/callback`
3. Add credentials to `.env`

**Training Cycles**:
Edit `src/config/config.js` to modify training cycles and stat caps.

### Trainer Agent Configuration

**Ollama Setup**:
1. Download from https://ollama.ai
2. Run: `ollama pull gemma:7b`
3. Ensure Ollama is running on `http://localhost:11434`

**Vector Databases**:
- Automatically created on first run
- Stored in `./data/vector_db/`
- Contains: Action Recording, Training Analysis, Knowledge Base

## 📊 Training System Details

### Stat Bonuses

| Activity | Bonuses | Limits |
|----------|---------|--------|
| **Strava Running** | Endurance: +0.2/km (if < 90) or +0.1/km (if ≥ 90)<br>Level: +1 per cycle (max) | 20 syncs/day<br>20 km/cycle per fighter |
| **Yoga** | Agility: +0.1<br>Mental Strength: +0.1 | 1 completion/day per fighter |
| **Meditation** | Mental Strength: +0.1<br>Leadership: +0.1 | 1 completion/day per fighter |

### Training Cycles

Monthly cycles with progressive stat caps:

| Cycle | Max Level | Max Endurance |
|-------|-----------|--------------|
| 1-6   | 1-6       | 95-100       |
| 7-12  | 7-12      | 100 (capped) |

### Recommendation System

The AI coach uses 7 data sources to generate recommendations:

1. **User Profile & Goals**: Personality, goals, preferences, practice level
2. **Training History**: Session counts, last activity, success rate
3. **Strava Data**: Running distance, frequency, fitness level
4. **Fighter Training Data**: Owned fighters, undertrained attributes
5. **Activity Patterns**: Consistency, streaks, preferred times
6. **Action History**: Recent app interactions
7. **Training Analysis**: Overall progress and insights

### Goal-Based Recommendations

| Goal | Primary Method | Secondary Method |
|------|---------------|------------------|
| Improve Body Composition | Running/Walking (Strava) | Yoga |
| Build Strength | Yoga | Running (Strava) |
| Improve Endurance | Running (Strava) | Yoga |
| Move More | Balanced (less-used activity) | Recent activity |
| Reduce Stress | Meditation | Yoga |
| Build Routine | Most recent activity | Meditation |
| Explore Movement | Meditation | Yoga or light movement |

## 🔌 API Endpoints

### Backend API (Port 3001)

**Health Check**
- `GET /health` - Server status

**Fighter Metadata**
- `GET /fighter-metadata/:tokenId` - Get NFT metadata for OpenSea/Mintify

**Strava Integration**
- `POST /strava/connect` - Connect Strava account
- `POST /strava/sync` - Sync Strava activities
- `GET /strava/status/:walletAddress` - Get connection status

**Training**
- `GET /training/media/:mediaType` - Get available courses
- `GET /training/questions/:mediaId` - Get quiz questions
- `POST /training/submit/yoga` - Submit yoga training
- `POST /training/submit/meditation` - Submit meditation training
- `GET /training/status/:walletAddress` - Get training status

**Fighter Management**
- `GET /fighter/:tokenId/stats` - Get fighter stats
- `GET /fighter/:tokenId/history` - Get stat change history
- `POST /fighter/link` - Link fighter to wallet
- `GET /fighter/wallet/:walletAddress` - Get fighters by wallet

### Trainer Agent API (Port 8000)

**Recommendations**
- `POST /recommendations/generate` - Generate new recommendation
- `POST /recommendations/accept` - Accept recommendation
- `POST /recommendations/decline` - Decline recommendation
- `POST /recommendations/check-completion` - Check if training matches recommendation
- `GET /recommendations/active/:wallet_address` - Get active recommendation

**Profile Setup**
- `POST /profile-setup` - Create or update user profile

**Agent Actions**
- `POST /agent/record-action` - Record user action
- `GET /agent/status/:wallet_address` - Get agent status
- `GET /agent/actions/:wallet_address` - Get user actions

## 🗄️ Database Schema

### SQLite Database (Backend)

Main tables:
- **tokens**: User wallet addresses, Strava tokens, linked fighters
- **FighterStat**: Current fighter stats (baseValue + deltaValue)
- **FighterStatHistory**: Complete history of stat changes
- **StravaStat**: Per-cycle mileage tracking
- **StravaSyncHistory**: Daily sync attempt tracking
- **mediainfo**: Yoga and meditation course metadata
- **media_questions**: Questions and answers for courses
- **user_submissions_log**: Training submission history
- **user_retry_cooldown**: Cooldown tracking for failed attempts
- **user_profile_setup**: User profiles, goals, coach personality
- **training_recommendations**: AI-generated recommendations

### ChromaDB Vector Databases (Trainer Agent)

Three collections:
- **action_recording**: User actions with semantic search
- **training_analysis**: Training pattern analysis
- **knowledge_base**: System rules, personality guidelines, evaluation logic

## 🛠️ Development

### Adding New Training Courses

1. Insert course into `mediainfo` table:
```sql
INSERT INTO mediainfo (media_id, media_type, description)
VALUES ('yoga-course-2', 'yoga', 'Advanced Yoga Session');
```

2. Insert questions into `media_questions` table:
```sql
INSERT INTO media_questions (media_id, questions_json, correct_indexes)
VALUES ('yoga-course-2', '[...]', '[1, 2]');
```

### Modifying Training Cycles

Edit `squad-unbound-games-backend/src/config/config.js`:

```javascript
{
  CYCLE_ID: 13,
  START_TIMESTAMP: 1746057600,
  END_TIMESTAMP: 1748736000,
  MAX_LEVEL: 13,
  MAX_ENDURANCE: 100
}
```

### Testing

**Backend**:
```bash
cd squad-unbound-games-backend
npm run typecheck  # TypeScript type checking
npm run db-stats   # View database statistics
npm run verify     # Verify setup
```

**Frontend**:
```bash
cd squad-unbound-games-frontend
npm run lint       # Run linter
npm run build      # Build for production
```

## 📝 Important Notes

1. **Base Values**: Never modify `baseValue` in `FighterStat` - they represent original minted stats
2. **Delta Values**: Only `deltaValue` should be modified through training
3. **UTC Timezone**: All daily limits reset at midnight UTC
4. **Leftover Mileage**: Carries over within the same cycle, discarded when moving to new cycle
5. **Multi-Fighter Support**: Users can own multiple fighters; each has independent limits
6. **Off-Chain Stats**: All fighter stats are stored off-chain for fast updates and low gas costs

## 🐛 Troubleshooting

### Database Issues

If you encounter database errors:
```bash
# Verify database exists
ls squad-unbound-games-backend/src/database/

# Check database stats
cd squad-unbound-games-backend
npm run db-stats
```

### Strava Connection Issues

- Verify `STRAVA_CLIENT_ID` and `STRAVA_CLIENT_SECRET` are correct
- Ensure redirect URI matches Strava app settings: `http://localhost:3001/strava/callback`
- Check that required scopes are enabled: `read`, `activity:read_all`

### Ollama Issues

- Ensure Ollama is running: `ollama serve`
- Verify model is installed: `ollama list`
- Check connection: `curl http://localhost:11434/api/tags`

### Frontend Wallet Issues

- Clear browser cache and localStorage
- Ensure wallet extension is installed and unlocked
- Check network configuration (Shape Network)

## 📚 Additional Documentation

- [AI System Summary](./AI_SYSTEM_SUMMARY.md) - Complete AI coach system overview
- [How to Check Recommendations](./HOW_TO_CHECK_RECOMMENDATIONS.md) - Database queries and API usage
- [Backend README](./squad-unbound-games-backend/README.md) - Detailed backend documentation
- [Frontend README](./squad-unbound-games-frontend/README.md) - Frontend setup and configuration
- [Trainer Agent README](./squad-unbound-games-trainer-agent/README.md) - AI agent architecture

## 🎯 Tech Stack

### Frontend
- **Framework**: Next.js 14 (React)
- **Language**: TypeScript
- **Wallet**: RainbowKit, wagmi, viem
- **Styling**: Tailwind CSS

### Backend
- **Framework**: Express.js
- **Language**: TypeScript 5.x
- **Database**: SQLite3
- **Blockchain**: Ethers.js

### Trainer Agent
- **Framework**: FastAPI
- **Language**: Python 3.8+
- **AI**: LangGraph, LangChain
- **Vector DB**: ChromaDB
- **LLM**: Ollama (Gemma 7B)

## 📄 License

ISC

## 🤝 Contributing

This is a private project. For issues or questions, please contact the development team.

---

**Note**: This platform stores all fighter data off-chain. The on-chain NFT contract's `tokenURI` should point to `https://api.unbound.games/metadata/{tokenId}` for proper marketplace integration.

