# 🎿 Airbnb Ski Resort Scraper

A Python web scraping project that collects Airbnb listing data across 10 major ski resort destinations in the western US. Built as a hands-on exercise in browser automation, HTML parsing, and structured data extraction.

---

## Overview

This notebook scrapes Airbnb search results and individual listing pages across ski towns — Breckenridge, Park City, Jackson Hole, Vail, Steamboat Springs, Big Sky, Telluride, Aspen, Lake Tahoe, and Taos — and compiles the data into a structured CSV for analysis. 

The scraper navigates paginated search results, extracts individual listing URLs, then visits each listing page to pull detailed pricing, amenity, and property data.

---

## What It Scrapes

For each listing, the scraper extracts:

| Field | Description |
|---|---|
| `title` | Listing name |
| `rating` | Star rating |
| `nightlyRate` | Effective nightly price charged |
| `originalPricePerNight` | Pre-discount price (if applicable) |
| `reducedPricePerNight` | Discounted price (if applicable) |
| `numberOfReviews` | Review count |
| `city`, `state`, `country` | Location |
| `listing_size` | Square footage |
| `num_guests`, `num_bedrooms`, `num_bathrooms` | Capacity details |
| `cleaningFee` | Cleaning fee |
| `airbnbServiceFee` | Airbnb service fee |
| `multiDayDeal` | Multi-night discount |
| `superHost` | Superhost status |
| `newListing` | New listing flag |
| `hotTub` | Has hot tub |
| `skiInSkiOut` | Ski-in/ski-out access |
| `url` | Source listing URL |

---

## Tech Stack

| Tool | Role |
|---|---|
| Python | Primary language |
| Selenium + ChromeDriver | Headless browser automation for dynamic pages |
| BeautifulSoup4 | HTML parsing |
| Requests | Static page fetching |
| Pandas | Data structuring and CSV export |
| webdriver-manager | ChromeDriver version management |
| multiprocess | Parallel processing support |

---

## How It Works

1. **Build search URLs** — Paginated search URLs are generated for each ski destination (up to 15 pages × 20 listings = ~300 listings per location).
2. **Collect listing links** — Selenium loads each search results page (handling JavaScript rendering), then BeautifulSoup extracts all individual listing URLs.
3. **Scrape listing details** — Each listing page is loaded via Selenium, the "show all amenities" button is clicked programmatically to expose the full amenity list, and all target fields are parsed from the resulting HTML.
4. **Export to CSV** — Results are concatenated into a Pandas DataFrame and written to `scrapedData.csv`.

---

## Key Technical Challenges

- **Dynamic rendering** — Airbnb's search results and listing pages are JavaScript-rendered. Selenium in headless Chrome mode handles this; static `requests` alone would return empty content.
- **Interaction automation** — The amenities section requires a button click to expand. Selenium's element selector finds and triggers this click before scraping.
- **Price normalization** — Airbnb surfaces prices in several formats (original, reduced, effective nightly). The scraper handles all three cases and resolves to a single `nightlyRate`.
- **Graceful failure** — All extraction functions use try/except to return `None` on failure, so a missing field on one listing doesn't crash the entire run.

---

## Output

Running the notebook produces `scrapedData.csv` — a flat file with one row per listing and the fields described above, ready for analysis or modeling.

---

## About

This is a portfolio project built to demonstrate practical Python and web scraping skills. The focus was on the full scraping pipeline: browser automation, DOM traversal, data normalization, and structured output — not on downstream analysis.

---

*Built with Python · Selenium · BeautifulSoup4 · Pandas*
