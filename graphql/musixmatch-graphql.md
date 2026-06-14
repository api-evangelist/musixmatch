# Musixmatch GraphQL Schema

This directory contains a conceptual GraphQL schema for the Musixmatch API. The schema is derived from the [Musixmatch REST API documentation](https://developer.musixmatch.com/documentation) and represents the data model for lyrics, tracks, artists, albums, and related music metadata.

## Overview

Musixmatch is a music data company and lyrics platform with over 80 million users, 8 million songs with lyrics, and support for translations across 115+ languages. The REST API provides access to tracks, artists, albums, lyrics, subtitles, snippets, and chart data.

The GraphQL schema in `musixmatch-schema.graphql` is a conceptual translation of the REST API endpoints into a unified typed graph, making relationships between entities explicit and queryable in a single request.

## Schema Structure

### Core Entities

| Type | Description |
|------|-------------|
| `Track` | A song with metadata, ratings, lyrics references, and genre associations |
| `Artist` | A musical artist with aliases, country, image, and rating data |
| `Album` | A music album with cover art, release info, genres, and ratings |
| `Lyrics` | Full lyrics body with copyright, language, explicitness, and score data |
| `Subtitle` | Time-synced lyrics (karaoke-style) in formats such as LRC or DFXP |
| `Snippet` | A short preview excerpt of a track's lyrics |

### Supporting Detail Types

Each core entity has a companion detail type (`TrackDetails`, `ArtistDetails`, `AlbumDetails`, `LyricsDetails`, `SubtitleDetails`, `SnippetDetails`) that pairs the entity with a `StatusCode` response envelope.

### Decomposed Attribute Types

The schema breaks nested REST response attributes into discrete types:

- `TrackRating`, `ArtistRating`, `AlbumRating` — numerical rating breakdowns
- `TrackLyrics`, `TrackSnippet`, `TrackSubtitle`, `TrackGenres` — track sub-resources
- `LyricsLanguage`, `LyricsExplicit`, `LyricsCopyright`, `LyricsScore` — lyrics metadata facets
- `ArtistAlias`, `ArtistCountry`, `ArtistImage` — artist attribute groups
- `AlbumGenres` — genre associations for albums
- `SubtitleFormat` (enum) — LRC, DFXP, STLEDU, MXMID
- `TrackId` — cross-platform identifier mapping (Spotify, ISRC, MusicBrainz)

### Catalog and Genre

| Type | Description |
|------|-------------|
| `Genre` | Music genre with name, vanity slug, and parent reference |
| `GenreDetails` | Wrapper used in genre lists on tracks and albums |
| `GenreParent` | Simplified parent genre for hierarchy traversal |
| `Catalog` | A curated track catalog with language and country scope |
| `CatalogDetails` | Catalog with status response |

### Search

| Type | Description |
|------|-------------|
| `Search` | Container for a search operation with query, type, results, and pagination |
| `SearchQuery` | Input parameters matching the REST q, q_track, q_artist, q_lyrics fields |
| `SearchType` (enum) | TRACK, ARTIST, ALBUM, LYRICS |
| `SearchResults` | Aggregated results container |
| `TrackResults` | Track list with total count |
| `ArtistResults` | Artist list with total count |
| `AlbumResults` | Album list with total count |

### Charts

| Type | Description |
|------|-------------|
| `ChartTrack` | A named track chart for a country with ranked track entries |
| `ChartTrackItem` | Chart position, delta, and entry date for a track |
| `ChartArtist` | A named artist chart for a country with ranked artist entries |
| `ChartArtistItem` | Chart position, delta, and entry date for an artist |

### Tasteomatics and Mood

| Type | Description |
|------|-------------|
| `MoodDefinition` | A mood with valence, arousal, and dominance scores |
| `TastematicsTag` | A semantic tag used in Musixmatch's Tasteomatics system with track associations |

### Translations

| Type | Description |
|------|-------------|
| `Translation` | A crowd-sourced lyrics translation with language and body |
| `TranslationDetails` | Translation with status response |
| `TranslationLanguage` (enum) | Supported translation language codes |

### Authentication

| Type | Description |
|------|-------------|
| `APIKey` | Developer API key with plan, rate limit, and endpoint scope |
| `Token` | Short-lived bearer access token |
| `OauthToken` | OAuth 2.0 token pair with refresh token and user context |

### Infrastructure

| Type | Description |
|------|-------------|
| `PageInfo` | Pagination metadata (page, pageSize, totals, hasNextPage) |
| `StatusCode` | Musixmatch API response envelope mirroring the REST `header.status_code` pattern |
| `StatusHeader` | Detailed status header with execution time, PID, and feedback |
| `ErrorResponse` | Structured error with status code, message, and hint |

## Query Operations

The `Query` root type covers all read operations from the REST API:

- `track`, `trackByIsrc`, `trackId` — track lookup by ID, ISRC, or cross-platform identifier
- `trackLyrics`, `trackSnippet`, `trackSubtitle`, `trackRichsync` — track sub-resource retrieval
- `artist`, `artistAlbums`, `artistRelated` — artist and discography lookup
- `album`, `albumTracks` — album and tracklist retrieval
- `lyrics`, `lyricsByTrack`, `lyricsTranslation` — direct lyrics and translation access
- `searchTracks`, `searchArtists`, `searchAlbums`, `search` — unified search
- `chartTracks`, `chartArtists` — country and named chart retrieval
- `genre`, `genres` — genre directory
- `mood`, `moods`, `tastematicsTag`, `tastematicsTags` — mood and tasteomatics access
- `catalog`, `catalogs` — catalog browsing

## Files

| File | Description |
|------|-------------|
| `musixmatch-schema.graphql` | Full GraphQL SDL schema with 60+ named types |
| `musixmatch-graphql.md` | This documentation file |

## Source

- REST API documentation: https://developer.musixmatch.com/documentation
- Developer portal: https://developer.musixmatch.com/
