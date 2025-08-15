# Music Streaming API

An API for searching, streaming, and getting song recommendations.

## Features

*   **Song Search**: Search for songs using a query, powered by YouTube. Returns top search results with details like name, artist, URL, thumbnail, and duration.
*   **Audio Streaming**: Get direct audio stream URLs for YouTube videos.
*   **Song Recommendations**: Based on a given YouTube video URL, retrieve YouTube's "Up Next" list as recommendations.
*   **Internal Recommendation Engine (Initialized)**: The application loads `songs_dataset.csv` and pre-processes it to create a TF-IDF vectorizer and cosine similarity matrix for a content-based recommendation system. *Note: While this engine is initialized, the `/recommend` endpoint currently uses YouTube's "Up Next" feature.*

## Technologies Used

*   **FastAPI**: For building the web API.
*   **Pydantic**: For data validation and serialization.
*   **yt-dlp**: For interacting with YouTube to search, stream, and get recommendations.
*   **pandas**: For data manipulation (used in the internal recommendation engine initialization).
*   **scikit-learn**: For machine learning utilities like `TfidfVectorizer` and `cosine_similarity` (used in the internal recommendation engine initialization).

## Installation

1.  **Clone the repository** (assuming this is a typical project setup):
    ```bash
    git clone <repository_url>
    cd <repository_directory>
    ```
2.  **Install dependencies**:
    Create a `requirements.txt` file with the following content:
    ```
    fastapi
    uvicorn
    pandas
    yt-dlp
    scikit-learn
    pydantic
    ```
    Then install them:
    ```bash
    pip install -r requirements.txt
    ```
3.  **Prepare Data**:
    *   Ensure you have a `songs_dataset.csv` file in the root directory if you plan to extend or use the internal content-based recommendation engine.

## Usage

To run the API, use Uvicorn:```bash
uvicorn app:app --reload
```

The API documentation will be available at `http://127.0.0.1:8000/docs` (Swagger UI) and `http://127.0.0.1:8000/redoc` (ReDoc).

## API Endpoints

### 1. Search for Songs

*   **Endpoint**: `GET /search`
*   **Description**: Searches YouTube for songs based on a query.
*   **Query Parameters**:
    *   `query` (string, required): The search term for the song.
*   **Response**: A list of `SongSearchResult` objects.

    ```json
    [
      {
        "name": "Song Title",
        "artist_name": "Artist Name",
        "url": "https://www.youtube.com/watch?v=VIDEO_ID",
        "thumbnail": "https://i.ytimg.com/vi/VIDEO_ID/hqdefault.jpg",
        "duration": 240
      }
    ]
    ```

### 2. Get Audio Stream URL

*   **Endpoint**: `GET /stream`
*   **Description**: Retrieves a direct audio stream URL for a given YouTube video URL.
*   **Query Parameters**:
    *   `url` (string, required): The full YouTube video URL.
*   **Response**: A `StreamInfo` object.

    ```json
    {
      "stream_url": "https://rr2---sn-vgqs7nel.googlevideo.com/videoplayback?..."
    }
    ```

### 3. Get Song Recommendations

*   **Endpoint**: `POST /recommend`
*   **Description**: Fetches YouTube's "Up Next" list as recommendations based on a provided YouTube video URL.
*   **Request Body**: A `RecommendationRequest` object.

    ```json
    {
      "url": "https://www.youtube.com/watch?v=VIDEO_ID"
    }
    ```
*   **Response**: A list of `Song` objects.

    ```json
    [
      {
        "name": "Recommended Song Title",
        "artist_name": "Recommended Artist Name",
        "url": "https://www.youtube.com/watch?v=RECOMMENDED_VIDEO_ID",
        "thumbnail": "https://i.ytimg.com/vi/RECOMMENDED_VIDEO_ID/hqdefault.jpg",
        "duration": 200
      }
    ]
    ```
