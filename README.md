# 🌾 Project Samarth - Backend API

> Intelligent Q&A system that answers complex questions about India's agricultural economy and climate patterns by querying live data from **data.gov.in**

**Built for Build for Bharat Challenge** 🇮🇳

[![Live Demo](https://img.shields.io/badge/Live-Demo-success?style=for-the-badge)](https://project-samarth-frontend-eight.vercel.app/)
[![API Status](https://img.shields.io/badge/API-Live-brightgreen?style=for-the-badge)](https://project-samarth-api-cs4k.onrender.com/api/health)
[![Python](https://img.shields.io/badge/Python-3.9+-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0.0-black?style=for-the-badge&logo=flask)](https://flask.palletsprojects.com/)

---

## 🎯 What This Does

This backend API powers an intelligent Q&A system that:
- 📊 **Fetches live data** from India's Open Government Data Portal (data.gov.in)
- 🧠 **Parses natural language** queries to extract entities (states, crops, years)
- 🔗 **Combines multiple datasets** across ministries (Agriculture, IMD)
- 📝 **Generates cited answers** with full source attribution

---

## 🌐 Live Links

- **🎨 Frontend App:** https://project-samarth-frontend-eight.vercel.app/
- **🔗 Backend API:** https://project-samarth-api-cs4k.onrender.com
- **💻 Frontend Repo:** https://github.com/mrmallick07/project-samarth-frontend

---

## 🏗️ Architecture
```
┌─────────────────┐
│  User Query     │
│ (Natural Lang)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Query Parser   │  ← Extracts: States, Crops, Years, Query Type
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Data Fetcher   │  ← API calls to data.gov.in
│                 │    - Ministry of Agriculture
│                 │    - India Meteorological Dept
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Answer Generator│  ← Synthesizes response + citations
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  JSON Response  │  ← With sources, metadata
└─────────────────┘
```

---

## 🚀 Key Features

### 1️⃣ **Intelligent Query Parsing**
```python
# Extracts entities from natural language
States: 29 Indian states recognized
Crops: 30+ major crops
Years: Handles ranges, "last N years", specific years
```

### 2️⃣ **Live Data Integration**
- ✅ Real-time API calls to data.gov.in
- ✅ No static datasets - always current
- ✅ Handles multiple data formats (CSV, JSON, XML)

### 3️⃣ **Cross-Domain Analysis**
- 🌧️ Climate data (rainfall, temperature) from IMD
- 🌾 Agricultural data (crop production, area, yield) from Ministry of Agriculture
- 🔗 Correlates data across ministries

### 4️⃣ **Mandatory Source Citations**
- Every data point linked to source dataset
- Includes dataset URL, resource ID, ministry name
- Full traceability for policy decisions

---

## 📊 Sample Queries

### Query 1: Rainfall Comparison
```
Compare the average annual rainfall in Punjab and Haryana 
for the last 5 years. List the top 3 most produced crops 
in each state during the same period.
```

**Response includes:**
- Average rainfall (mm) per state
- Seasonal breakdown (monsoon, winter, pre-monsoon)
- Top 3 crops with production volumes
- Source: IMD + Ministry of Agriculture datasets

### Query 2: Production Extremes
```
Identify the district in Punjab with the highest production 
of Wheat in 2023 and compare with the lowest production 
district in Haryana.
```

**Response includes:**
- District names
- Production figures (tonnes)
- Percentage of state production
- Geographic and infrastructure factors

### Query 3: Trend Analysis
```
Analyze the production trend of Rice in West Bengal over 
the last decade and correlate with rainfall patterns.
```

**Response includes:**
- Year-over-year production trends
- Correlation with rainfall data
- Climate impact analysis

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| **Python 3.9+** | Core language |
| **Flask** | Web framework & REST API |
| **Pandas** | Data manipulation & analysis |
| **Requests** | HTTP client for data.gov.in API |
| **Flask-CORS** | Cross-origin resource sharing |
| **Gunicorn** | Production WSGI server |

---

## 📦 API Endpoints

### `POST /api/query`
Submit a natural language query

**Request:**
```json
{
  "query": "Compare rainfall in Punjab and Haryana"
}
```

**Response:**
```json
{
  "success": true,
  "query": "Compare rainfall...",
  "answer": "**Rainfall Comparison Analysis**\n\n...",
  "sources": [
    {
      "dataset": "Rainfall in India",
      "source": "IMD",
      "url": "https://data.gov.in/...",
      "resource_id": "e9aafad3-6a08-4f66-b59d-38c65e7ae44f"
    }
  ],
  "metadata": {...},
  "timestamp": "2025-11-02T..."
}
```

### `GET /api/health`
Health check endpoint

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-11-02T..."
}
```

### `GET /api/datasets`
List available datasets

---

## 🔧 Local Development Setup

### Prerequisites
- Python 3.9 or higher
- pip (Python package manager)
- data.gov.in API key ([Get one here](https://data.gov.in))

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/mrmallick07/project-samarth-backend.git
cd project-samarth-backend
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure environment variables**
```bash
# Create .env file
echo "DATAGOVIN_API_KEY=your_api_key_here" > .env
```

Get your API key:
- Visit https://data.gov.in
- Register/Login
- Go to "My Account" → Copy API Key

5. **Run the server**
```bash
python app.py
```

Server runs at: `http://localhost:5000`

### Test the API
```bash
# Health check
curl http://localhost:5000/api/health

# Sample query
curl -X POST http://localhost:5000/api/query \
  -H "Content-Type: application/json" \
  -d '{"query":"Compare rainfall in Punjab and Haryana"}'
```

---

## 🌍 Deployment

### Deploy to Render (Free)

1. **Fork this repository**
2. **Sign up at [Render.com](https://render.com) with GitHub**
3. **Create New Web Service**
   - Runtime: Python 3
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `gunicorn app:app`
4. **Add Environment Variable:**
   - Key: `DATAGOVIN_API_KEY`
   - Value: Your data.gov.in API key
5. **Deploy!**

**Note:** Free tier sleeps after 15 min inactivity. First request takes 30-60 sec to wake up.

---

## 📁 Project Structure
```
project-samarth-backend/
├── app.py                 # Main Flask application
├── requirements.txt       # Python dependencies
├── .env.example          # Environment variables template
├── .gitignore            # Git ignore rules
└── README.md             # This file
```

### Key Components in `app.py`:
```python
# 1. Query Parser
class QueryUnderstanding:
    - extract_states()      # Identifies Indian states
    - extract_crops()       # Identifies crop names
    - extract_years()       # Handles time periods
    - classify_query()      # Determines query type

# 2. Data Fetcher
class DataGovInAPI:
    - fetch_dataset()       # Calls data.gov.in APIs
    - Handles authentication, pagination, caching

# 3. Answer Generator
class IntelligentAnswerGenerator:
    - answer()              # Main entry point
    - Handlers for different query types
    - Synthesizes responses with citations
```

---

## 🎓 Design Decisions

### 1. **Why Live API Calls?**
- ✅ Always current data (no stale information)
- ✅ No storage requirements
- ✅ Handles dataset updates automatically
- ⚠️ Trade-off: Slower first request (free tier)

### 2. **Why Rule-Based Parsing?**
- ✅ Fast and predictable
- ✅ No ML model overhead
- ✅ Easy to extend with new entities
- ⚠️ Trade-off: Limited to predefined patterns

### 3. **Why Mandatory Citations?**
- ✅ Traceability for policy decisions
- ✅ Builds trust in AI-generated answers
- ✅ Complies with open data principles

### 4. **Why Separate Backend/Frontend?**
- ✅ Technology independence
- ✅ Easier scaling
- ✅ Can deploy separately
- ✅ Better security (API key hidden)

---

## 🔒 Security & Privacy

- ✅ **Data Sovereignty:** Can be deployed on-premise
- ✅ **No Personal Data:** Uses only public government datasets
- ✅ **API Key Protection:** Never exposed to frontend
- ✅ **CORS Configuration:** Restricts origins in production

---

## 🐛 Troubleshooting

### Issue: "Application failed to respond"
**Solution:** Check Render logs. Ensure `gunicorn` is in requirements.txt

### Issue: CORS errors
**Solution:** Update CORS origins in `app.py` to include your frontend URL

### Issue: Slow first request
**Solution:** This is normal on free tier. Backend sleeps after inactivity.

### Issue: "No records found"
**Solution:** data.gov.in API may be down or dataset structure changed. Check API directly.

---

## 🤝 Contributing

This is a hackathon prototype built for Build for Bharat. To extend:

1. Add more datasets (soil data, market prices, irrigation)
2. Improve query parser with ML/NLP models
3. Add data visualization (charts, graphs)
4. Implement caching with Redis
5. Add user authentication

---

## 📄 License

MIT License - Feel free to use this for learning!

---

## 👨‍💻 Author

**Noushad Mallick**
- GitHub: [@mrmallick07](https://github.com/mrmallick07)
- Built for: Build for Bharat Challenge 2024

---

## 🙏 Acknowledgments

- **data.gov.in** - Open Government Data Platform
- **Ministry of Agriculture & Farmers Welfare** - Crop production data
- **India Meteorological Department (IMD)** - Climate data
- **Build for Bharat** - For the opportunity to solve real problems

---

## 📊 Stats

![Lines of Code](https://img.shields.io/badge/Lines%20of%20Code-500+-blue)
![API Endpoints](https://img.shields.io/badge/API%20Endpoints-3-green)
![Data Sources](https://img.shields.io/badge/Data%20Sources-2%20Ministries-orange)

---

**⭐ If you found this helpful, please star this repository!**

[🌐 Try Live Demo](https://project-samarth-frontend-eight.vercel.app/) | [📧 Report Issues](https://github.com/mrmallick07/project-samarth-backend/issues)
