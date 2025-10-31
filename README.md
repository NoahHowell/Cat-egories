# Cat-egories

A data analysis project that scrapes and analyzes YouTube cat channel content to discover content patterns, engagement metrics, and genre classifications. Created for CIS400: Principles of Social Media & Data Mining at Syracuse University.

## Project Overview

This project uses the YouTube Data API v3 to collect metadata from popular cat-themed YouTube channels, ***it will soon*** analyze the data to understand:
- Content themes and categories (rescue, funny, daily life, etc.)
- Engagement patterns across different content types
- Channel-specific strategies and posting patterns
- Hashtag and tag effectiveness

## Features

- **Automated Data Collection**: Scrapes video metadata from multiple cat channels
- **Clean Data Export**: Separate CSV files for each channel plus aggregate summary
- **Flexible Channel Input**: Supports channel IDs, @usernames, and custom URLs
- **Comprehensive Metadata**: Titles, descriptions, tags, hashtags, views, likes, comments, and more

## Setup

### Prerequisites

- Python 3.7+
- YouTube Data API v3 key

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/NoahHowell/Cat-egories.git
   cd Cat-egories
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install google-api-python-client pandas tqdm python-dotenv
   ```

4. **Get a YouTube API Key**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select an existing one
   - Enable the **YouTube Data API v3**
   - Create credentials (API Key)
   - Copy your API key

5. **Configure your API key**
   - Create a `.env` file
   - Replace `your_api_key_here` with your actual YouTube API key:
     ```
     YOUTUBE_API_KEY=AIzaSyD...your_actual_key...
     ```

6. **Add YouTube channels**
   - Edit `accounts.txt` to include the channels you want to scrape
   - Format: one channel per line (supports @username, channel ID, or custom URL)
   ```
   @mugumogu
   @CrunchycatLuna
   @chacha-rme
   ```

## Usage

### Running the Scraper

1. Open `scrape.ipynb` in Jupyter Notebook or VS Code
2. Run all cells in order
3. The scraper will:
   - Resolve channel identifiers to channel IDs
   - Fetch channel metadata
   - Scrape up to 50 videos per channel
   - Export data to CSV files in the `data/` directory

### Output Files

**Individual Channel CSVs** (`data/{ChannelName}.csv`):
- One file per channel containing video-level data only
- Columns: `video_id`, `title`, `description`, `published_at`, `tags`, `hashtags`, `duration`, `view_count`, `like_count`, `comment_count`

**Channel Summary CSV** (`data/channels_summary.csv`):
- Aggregate statistics for all scraped channels
- Includes: subscriber counts, total views, video counts, average engagement metrics

## Data Schema

### Video Data
| Column | Description | Example |
|--------|-------------|---------|
| `video_id` | YouTube video ID | `dQw4w9WgXcQ` |
| `title` | Video title | `Cute Cat Playing with Box` |
| `description` | Video description (cleaned) | `My cat loves this box!` |
| `published_at` | Upload timestamp | `2024-03-15T10:30:00Z` |
| `tags` | Creator-assigned tags (pipe-separated) | `cat\|funny\|pets\|animals` |
| `hashtags` | Hashtags from description (pipe-separated) | `#catsofyoutube\|#funny` |
| `duration` | Video length (ISO 8601) | `PT5M32S` |
| `view_count` | Number of views | `125000` |
| `like_count` | Number of likes | `3500` |
| `comment_count` | Number of comments | `450` |

### Channel Summary Data
| Column | Description |
|--------|-------------|
| `channel_id` | YouTube channel ID |
| `channel_title` | Channel name |
| `channel_description` | Channel about section |
| `subscriber_count` | Number of subscribers |
| `view_count` | Total all-time views |
| `video_count` | Total number of videos |
| `total_videos_scraped` | Number of videos collected |
| `avg_views` | Average views per video (from scraped videos) |
| `avg_likes` | Average likes per video |
| `avg_comments` | Average comments per video |

## Example Channels

The project currently includes these popular cat channels:
- **@mugumogu** - Maru
- **@CrunchycatLuna** - Crunchy Cat Luna
- **@chacha-rme** - ChaCha

## API Rate Limits

YouTube Data API v3 has a default quota of **10,000 units per day**. This scraper uses approximately:
- 1 unit per channel info request
- 1 unit per 50 videos in playlist
- 1 unit per 50 video details

For 3 channels with 50 videos each, you'll use roughly **10-15 units** per run.

## Security Notes

- Never commit your `.env` file with your API key
- The `.gitignore` file is configured to exclude `.env` automatically
- Keep your API key secure and don't share it publicly

