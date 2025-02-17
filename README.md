# iview-proxy

A proxy web app that serves ABC iView content outside of the iView webplayer, avoiding intrusive data harvesting.

## Purpose

This project has two main goals:

- Avoid as much account authentication as possible to watch ABC iView content,

- Avoid providing as much data as possible to iView to stop data harvesting.

Despite being aptly warned, the ABC has taken a forceful approach to collecting ABC iView data. This app seeks to remedy this by obtaining iView medias' `m3u8` playlists, and isolating them to a locked-down media player, completely free of any analytics tracking.

## How does it work?

### Search

The search function on iView is riddled with analytics tracking:

```json
{
	"requests": [
		{
			"indexName": "ABC_production_iview_web",
			"params": "highlightPreTag=%3Cais-highlight-0000000000%3E&highlightPostTag=%3C%2Fais-highlight-0000000000%3E&getRankingInfo=true&clickAnalytics=true&analytics=true&userToken=anonymous-60451354-4525-401b-9a54-3e18fa480cd5&facetFilters=playable%3A%20-false&query=s&facets=%5B%5D&tagFilters="
		},
		{
			"indexName": "ABC_production_iview_web",
			"params": "highlightPreTag=%3Cais-highlight-0000000000%3E&highlightPostTag=%3C%2Fais-highlight-0000000000%3E&getRankingInfo=true&clickAnalytics=true&analytics=true&userToken=anonymous-60451354-4525-401b-9a54-3e18fa480cd5&facetFilters=playable%3A%20-false&query=s&hitsPerPage=6&filters=docType%3A%20category%20OR%20docType%3A%20channel&ruleContexts=iview_categories&facets=%5B%5D&tagFilters="
		},
		{
			"indexName": "ABC_production_iview_web",
			"params": "highlightPreTag=%3Cais-highlight-0000000000%3E&highlightPostTag=%3C%2Fais-highlight-0000000000%3E&getRankingInfo=true&clickAnalytics=true&analytics=true&userToken=anonymous-60451354-4525-401b-9a54-3e18fa480cd5&facetFilters=playable%3A%20-false&query=s&hitsPerPage=8&filters=docType%3A%20Program&ruleContexts=iview_programs&page=0&facets=%5B%5D&tagFilters="
		},
		{
			"indexName": "ABC_production_iview_web",
			"params": "highlightPreTag=%3Cais-highlight-0000000000%3E&highlightPostTag=%3C%2Fais-highlight-0000000000%3E&getRankingInfo=true&clickAnalytics=true&analytics=true&userToken=anonymous-60451354-4525-401b-9a54-3e18fa480cd5&facetFilters=playable%3A%20-false&query=s&hitsPerPage=8&filters=docType%3A%20VideoEpisode%20AND%20NOT%20subType%3A%20feature%20AND%20NOT%20subType%3A%20livestream&ruleContexts=iview_episodes&page=0&facets=%5B%5D&tagFilters="
		}
	]
}
```

So `iview-proxy` disables it all, and locks down iView's search to the bare-minimum, keeping your data safe.

### Watching

When you watch a video on the normal iView site, all sorts of data is sent back to iView and third parties, which is concerning to those who care about their privacy. See [here](https://github.com/AusOpenTech/ABCData/).

`iview-proxy` calls only show/series metadata APIs to get the video info, and acts as if it is a web scraper of sorts. Once it has the video source for a show, it plays it in a non-custom `video.js` player, completely free of any analytics tracking.

## Setup

The actual app is contained in the `src` directory.

1. Install required dependencies

    ```sh
    npm install
    ```

2. Start the server

    ```sh
    npm start
    ```

    or 

    ```sh
    node .
    ```

3. Navigate to `localhost:1984` in a browser to access the app.