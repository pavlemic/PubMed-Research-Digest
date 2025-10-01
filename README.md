# PubMed Research Digest 📚

An automated literature search and email digest system that fetches recent papers from PubMed, ranks them by relevance to your research interests, and sends you a personalized summary via email.

## Features

- 🔍 **Automated PubMed Search**: Queries PubMed for multiple research topics
- 🧠 **Semantic Ranking**: Uses sentence transformers to rank papers by relevance
- 📝 **AI Summarization**: Generates concise summaries of abstracts using BART
- 📧 **Email Delivery**: Sends formatted digest directly to your inbox
- ⚙️ **Customizable**: Easy to modify research interests and settings

## How It Works

1. Fetches papers from PubMed for each research interest
2. Uses semantic embeddings to rank papers by relevance
3. Summarizes abstracts using a transformer-based model
4. Formats results into an email digest
5. Sends the digest to your specified email address

## Prerequisites

- Python 3.8+
- Google Cloud Project with Gmail API enabled
- OAuth 2.0 credentials (`credentials.json`)

## Installation

### Local Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/pubmed-digest.git
cd pubmed-digest

# Create virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install biopython sentence-transformers transformers torch
pip install google-auth-oauthlib google-auth-httplib2 google-api-python-client
```

### Google Colab

Simply upload the notebook and uncomment the pip install cells at the top.

## Setup

### 1. Configure Email and Interests

Edit the configuration section in `pubmed_digest.py`:

```python
# Replace with your email
ENTREZ_EMAIL = "your.email@example.com"
RECIPIENT_EMAIL = "your.email@example.com"

# Customize your research interests
INTERESTS = [
    "cancer genomics",
    "brain organoids",
    # Add your topics here
]
```

### 2. Set Up Gmail API

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable the Gmail API:
   - Navigate to "APIs & Services" → "Library"
   - Search for "Gmail API" and enable it
4. Create OAuth 2.0 credentials:
   - Go to "APIs & Services" → "Credentials"
   - Click "Create Credentials" → "OAuth client ID"
   - Application type: "Desktop app"
   - Name it (e.g., "PubMed Digest")
   - Download the JSON file and save as `credentials.json`
5. Add authorized redirect URI:
   - Edit your OAuth client
   - Under "Authorized redirect URIs", add: `urn:ietf:wg:oauth:2.0:oob`
   - Save changes

### 3. Place Credentials

Put `credentials.json` in the same directory as the script:

```
pubmed-digest/
├── pubmed_digest.py
├── credentials.json
└── README.md
```

## Usage

### Run the Script

```bash
python pubmed_digest.py
```

### First-Time Authentication

1. The script will display an authorization URL
2. Open the URL in your browser
3. Log in with your Google account
4. Grant permissions
5. Copy the authorization code
6. Paste it back into the terminal
7. Authentication token will be saved in `token.pkl` for future use

### Expected Output

```
Loading embedding model...
Loading summarization model...

🔍 Fetching and processing papers...
  [1/13] Processing: cancer genomics
  [2/13] Processing: brain organoids
  ...

📧 Authenticating with Gmail...
📤 Sending email...
✅ Email sent successfully!
```

## Configuration Options

Edit these variables in the script:

```python
MAX_PAPERS_PER_TOPIC = 15      # Papers to fetch per topic
TOP_PAPERS_PER_TOPIC = 5       # Top papers to include in digest
SUMMARY_MAX_LENGTH = 100       # Maximum summary length
```

## Customization

### Add/Remove Research Interests

```python
INTERESTS = [
    "your topic 1",
    "your topic 2",
    # Add more topics
]
```

### Change Summarization Model

Replace `facebook/bart-large-cnn` with another model:
- `facebook/bart-large-xsum` (more concise)
- `google/pegasus-xsum` (alternative)

### Use GPU for Faster Processing

```python
summarizer = pipeline("summarization", model="facebook/bart-large-cnn", device=0)  # 0 for GPU
```

## Scheduling (Optional)

### Linux/Mac (cron)

```bash
# Edit crontab
crontab -e

# Run daily at 8 AM
0 8 * * * /path/to/venv/bin/python /path/to/pubmed_digest.py
```

### Windows (Task Scheduler)

Create a scheduled task to run the script at your preferred time.

## Troubleshooting

### "Error: could not locate runnable browser"
- Use the manual authorization code flow (already implemented)
- Make sure `redirect_uri='urn:ietf:wg:oauth:2.0:oob'` is in your OAuth client settings

### "Invalid credentials"
- Delete `token.pkl` and re-authenticate
- Verify your `credentials.json` is for a Desktop app

### "No papers found"
- Check your PubMed query terms
- Verify `ENTREZ_EMAIL` is set correctly
- Check internet connection

### Summarization is slow
- Use GPU by setting `device=0` in the summarizer
- Reduce `MAX_PAPERS_PER_TOPIC` or `TOP_PAPERS_PER_TOPIC`

## Dependencies

- `biopython`: PubMed API access
- `sentence-transformers`: Semantic similarity
- `transformers`: Text summarization
- `torch`: Deep learning backend
- `google-api-python-client`: Gmail API
- `google-auth-oauthlib`: OAuth authentication

## Security Notes

⚠️ **Important**:
- Never commit `credentials.json` or `token.pkl` to version control
- Add them to `.gitignore`
- Keep your OAuth credentials secure

## License

MIT License - feel free to use and modify!

## Contributing

Pull requests welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a pull request

## Acknowledgments

- PubMed/NCBI for the E-utilities API
- Hugging Face for transformer models
- Google for Gmail API

## Contact

For questions or issues, please open a GitHub issue.

---

**Note**: This tool is for personal research purposes. Please comply with PubMed's usage guidelines and rate limits.
