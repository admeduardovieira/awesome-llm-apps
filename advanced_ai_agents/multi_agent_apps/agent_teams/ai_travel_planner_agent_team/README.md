# ✈️ TripCraft AI

**Your journey, perfectly crafted with intelligence.**

Travel planning is overwhelming—juggling dozens of tabs, comparing conflicting info, spending hours just to get started. TripCraft AI makes that disappear. It's a multi-agent AI system that turns simple inputs into complete travel itineraries. Describe your ideal trip, and it handles flights, hotels, activities, and budget automatically.

## 🎯 Goal

Make travel planning effortless and personal. No stress, no endless research—just a plan that feels crafted specifically for you.

---

## ⚙️ How It Works

1. **🎯 Input Your Vision** - Fill out a form with destination, dates, budget, travel style, and preferences
2. **🤖 AI Agents Collaborate** - Specialized agents handle flights, hotels, activities, and budgeting in parallel
3. **🗺️ Get Your Itinerary** - Receive a complete day-by-day plan with bookings, costs, and recommendations

### Key Features
- **Personalized Planning** - Tailored to your travel style and interests
- **Hidden Gems Discovery** - Beyond typical tourist spots using advanced search
- **Smart Optimization** - Balances cost, time, and experience
- **Complete Packages** - Everything from flights to dining recommendations

---

## 🛠️ Tech Stack

**Frontend:** Next.js, React, TypeScript
**Backend:** Python, FastAPI, PostgreSQL
**AI:** Agno (agent coordination), Gemini (LLM), Exa (search), Firecrawl (web scraping)
**APIs:** Google Flights, Kayak

---

## 📸 Visuals

![Image](https://github.com/user-attachments/assets/5fae2938-6d2c-4fc7-86be-d22bb84729a6)
![Image](https://github.com/user-attachments/assets/1bd6e98f-ae32-47be-90a0-23ee6f06c613)
![Image](https://github.com/user-attachments/assets/45db7d19-67ca-4c92-985f-79a7cb976b1c)
![Image](https://github.com/user-attachments/assets/7a06c3de-281d-4820-a517-ea81137289d7)
![Image](https://github.com/user-attachments/assets/523f0d02-8a72-4709-b3d4-5102f1d1b950)
![Image](https://github.com/user-attachments/assets/dbab944a-7678-4eae-9ead-05f15c3de407)

---

## 👥 About

**Built by**: Amit Wani [@mtwn105](https://github.com/mtwn105)

Full-stack developer and software engineer passionate about building intelligent systems that solve real-world problems. TripCraft AI represents the intersection of advanced AI capabilities and practical travel planning needs.

---

## 📽️ Demo Video Link

[https://youtu.be/eTll7EdQyY8](https://youtu.be/eTll7EdQyY8)

---

## 🤖 AI Agents

Six specialized agents work together to create comprehensive travel plans:

1. **🏛️ Destination Explorer** - Researches attractions, landmarks, and experiences
2. **🏨 Hotel Search Agent** - Finds accommodations based on location, budget, and amenities
3. **🍽️ Dining Agent** - Recommends restaurants and culinary experiences
4. **💰 Budget Agent** - Handles cost optimization and financial planning
5. **✈️ Flight Search Agent** - Plans air travel routes and comparisons
6. **📅 Itinerary Specialist** - Creates detailed day-by-day schedules with optimal timing

---

## 📥 Installation

### Backend Setup

This backend uses **uv** as the package manager (ultra-fast, recommended).

```bash
cd backend

# Install uv (if not already installed)
pip install uv

# Install dependencies
uv pip install pyproject.toml

# Or use Python virtual environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
# Windows: venv\Scripts\activate

# Install dependencies with pip
pip install -r <(python -c "import tomllib; d=tomllib.load(open('pyproject.toml','rb')); print(''.join(chr(10).join(d['project']['dependencies'])))")
```

### Environment Configuration

1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```

2. Edit `.env` and add your API keys:
   ```
   # Bright Data credentials (optional - for scraping)
   BRIGHT_DATA_API_TOKEN=your_bright_data_api_token
   BRIGHT_DATA_BROWSER_AUTH=your_bright_data_browser_auth

   # Database connection URL
   DATABASE_URL=your_database_url

   # OpenRouter API key (optional)
   OPENROUTER_API_KEY=your_openrouter_api_key

   # OpenAI API key
   OPENAI_API_KEY=your_openai_api_key

   # Gemini API key
   GOOGLE_API_KEY=your_google_api_key
   ```

### Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Run development server
npm run dev
# App runs on http://localhost:3000
```

### Running the Application

```bash
# Start backend (port 8000)
cd backend
uv run python main.py

# Start frontend (port 3000)
cd frontend
npm run dev
```

---

## 📚 Resources

- [Step-by-step tutorials on Unwind AI](https://www.theunwindai.com) · **[Quick start](#-run-one-now)** · **[Browse all templates](#-browse-all-templates)**

---

> 📬 New templates drop weekly. [Get them in your inbox on Unwind AI](https://www.theunwindai.com).

---

## 🔧 Environment Variables

See `.env.example` for all available configuration options.