# guardian_aot
# Guardian-AoT: Multimodal Disaster Memory & Response System

A Qdrant-powered disaster response recommendation system using Algorithm of Thoughts (AoT) planning with evidence-based memory and confidence scoring.

## Features

- **Multimodal Memory**: Text + Image embeddings stored in Qdrant
- **AoT Planning**: 5-7 atom structured reasoning with JSON schema
- **Evidence-Based Retrieval**: Historical disaster data retrieval
- **Memory Write-back**: Stores planning atoms for future reference
- **Baseline Comparison**: Compares current vs historical responses
- **Confidence Scoring**: Reliability metrics for recommendations
- **Operational Briefing**: Cited, actionable recommendations

## Quick Start (Google Colab)

### Option 1: Qdrant Cloud (Recommended for Colab)

1. Sign up for free Qdrant Cloud: https://cloud.qdrant.io/
2. Create a cluster and get your API key and URL
3. Open `demo.ipynb` in Colab
4. Set your Qdrant credentials in the notebook
5. Run all cells

### Option 2: Local Docker (For local development)
```bash
# Start Qdrant with Docker
docker run -p 6333:6333 qdrant/qdrant

# Install dependencies
pip install -r requirements.txt

# Run demo
python -m src.cli --mode demo
```

## Installation
```bash
pip install -r requirements.txt
```

## Usage

### CLI Demo
```bash
# Run full demo with synthetic data
python -m src.cli --mode demo

# Query specific disaster
python -m src.cli --mode query --disaster "earthquake magnitude 7.2"

# Generate synthetic dataset only
python -m src.cli --mode generate
```

### Programmatic Usage
```python
from src.response_system import GuardianAoTSystem

# Initialize system
system = GuardianAoTSystem(
    qdrant_url="http://localhost:6333",  # or your Qdrant Cloud URL
    qdrant_api_key=None  # or your Qdrant Cloud API key
)

# Process disaster scenario
result = system.process_disaster_scenario(
    disaster_type="earthquake",
    magnitude=7.2,
    location="coastal city",
    population_affected=50000
)

print(result['operational_briefing'])
```

## Architecture

1. **Data Layer**: Synthetic disaster scenarios with text and image metadata
2. **Embedding Layer**: sentence-transformers (text) + CLIP-like synthetic (images)
3. **Storage Layer**: Qdrant vector database with dual collections
4. **Reasoning Layer**: AoT planner with 5-7 structured reasoning atoms
5. **Memory Layer**: Write-back system for learning from past reasoning
6. **Response Layer**: Evidence-based recommendations with citations

## AoT Schema

Each reasoning atom follows this JSON structure:
```json
{
  "atom_id": 1,
  "thought": "Analysis of current situation",
  "evidence": ["citation_1", "citation_2"],
  "confidence": 0.85,
  "action_required": true,
  "priority": "high"
}
```

## Output Format

The system generates:
- **Operational Briefing**: Executive summary with citations
- **AoT Reasoning Chain**: Full 5-7 atom reasoning process
- **Confidence Scores**: Overall and per-recommendation
- **Baseline Comparison**: Historical vs current approach
- **Memory Atoms**: Stored for future retrieval

## Performance

- Colab Runtime: ~3-5 minutes for full demo
- Embedding Generation: ~30 seconds for 50 scenarios
- Qdrant Indexing: ~10 seconds
- Query + AoT Planning: ~2-3 seconds per query

## License

MIT License
