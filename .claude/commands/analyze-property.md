---
name: analyze-property
description: Automate end-to-end real estate property research and investment analysis by scraping Zillow data, downloading images, and generating comprehensive investment reports with AI analysis
color: green
---

You are executing the **Property Analysis Automation Command** - an elite real estate research and investment analysis system.

## Mission

Automate property analysis from Zillow URL to investment decision:
1. Scrape comprehensive property data from Zillow using Firecrawl
2. Download and organize all property images
3. Integrate Ceremonia Spaces business model context
4. Perform deep investment analysis using real-estate-investment-analyst agent
5. Generate standalone investment report with clear BUY/PASS/INVESTIGATE recommendation

## Input Processing

**Arguments received:** {{ARGUMENTS}}

Parse input to extract:
- Zillow URL(s) or address(es)
- Property identification info
- Any flags (--update, --include-comps, etc.)

## Execution Workflow

### Step 1: Property Identification & URL Validation

Extract property details from input:
- If Zillow URL provided: Extract address and ZPID
- If address provided: Construct Zillow search URL
- Generate property slug: `{street-number}-{street-name}-{city}-{state}-{zip}`

Example slug: `11042-n-saint-vrain-dr-lyons-co-80540`

### Step 2: Folder Structure Creation

Create property folder structure:
```
/docs/{property-slug}/
├── images/
├── property-data.json
├── market-data.json (if enhanced mode)
└── analysis.md (to be generated)
```

Use Bash tool to create directories:
```bash
mkdir -p /Users/austinmao/Documents/GitHub/spaces/docs/{property-slug}/images
```

### Step 3: Firecrawl Property Scraping

**API Key:** `fc-23f2747b28dc41149820ff27045b6ab3`

Use Bash tool to call Firecrawl API:
```bash
curl -X POST 'https://api.firecrawl.dev/v1/scrape' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer fc-23f2747b28dc41149820ff27045b6ab3' \
  -d '{
    "url": "{zillow_url}",
    "formats": ["markdown", "html"],
    "onlyMainContent": false,
    "waitFor": 2000
  }'
```

Extract from Firecrawl response:
- Property address, price, beds/baths, sqft, lot size, year built
- Property type, HOA fees, property tax, days on market
- Price history, listing description, features
- Image URLs (all photos from gallery)
- MLS number, agent info

Save to: `/docs/{property-slug}/property-data.json`

### Step 4: Image Download & Organization

Extract all image URLs from Firecrawl response.

Download images in parallel using Bash:
```bash
cd /Users/austinmao/Documents/GitHub/spaces/docs/{property-slug}/images
# Download each image with sequential naming
curl -o 01.jpg "{image_url_1}"
curl -o 02.jpg "{image_url_2}"
# ... etc
```

Generate manifest.json:
```json
{
  "images": [
    {
      "filename": "01.jpg",
      "sourceURL": "{url}",
      "downloadedAt": "{timestamp}"
    }
  ]
}
```

### Step 5: Business Model Context Integration

Read business model framework:
```
Read: /Users/austinmao/Documents/GitHub/spaces/business-model-framework.md
```

Extract relevant sections for property analysis:
- Regulatory framework (Colorado NMHA, STR licensing)
- Revenue models and pricing strategy
- Operating expense structure
- Financial benchmarks (DSCR targets, utilization rates)
- Entity structure (OpCo/LandCo, healing center licensing)

### Step 6: Enhanced Market Data Gathering (If Requested)

If `--include-comps` or `--include-market-trends` flags present:

**Comparable Sales Search:**
```
WebSearch: "{property address} comparable sales recent 2025"
WebSearch: "{city} {state} real estate sold properties {price range}"
```

**Market Trends Search:**
```
WebSearch: "{city} {state} real estate market trends 2025"
WebSearch: "{neighborhood} property values appreciation"
```

Save market data to: `/docs/{property-slug}/market-data.json`

### Step 7: AI Investment Analysis

Invoke real-estate-investment-analyst agent using Task tool:

**Agent Prompt Template:**
```markdown
# Property Investment Analysis Request

## Property Summary
**Address:** {full address}
**Zillow URL:** {url}
**List Price:** ${price} | ${price_per_sqft}/sqft
**Type:** {property_type}
**Size:** {bedrooms}bd/{bathrooms}ba | {sqft} sqft | {lot_size} lot
**Built:** {year_built}
**Days on Market:** {days_on_market}

## Property Data
{structured property data from JSON}

## Property Images
{count} images available in: `/docs/{property-slug}/images/`
Review images for property condition, layout suitability, and visual appeal.

## Business Model Context (Ceremonia Spaces)

### Overview
Ceremonia Spaces is building a portfolio of healing center properties for psilocybin-assisted therapy under Colorado's Natural Medicine Health Act (NMHA).

### Key Business Requirements:
{relevant sections from business-model-framework.md}

**Critical Evaluation Criteria:**
1. **Zoning Compliance:** Can property legally operate as healing center and/or STR?
2. **Layout Suitability:** How many therapy rooms can be configured? (Target: 10-14 rooms)
3. **Revenue Potential:** Based on $497-500/room/day pricing, what's projected annual revenue?
4. **OpCo/LandCo Structure:** Can property support NNN lease model with DSCR > 1.5x?
5. **Regulatory Feasibility:** NMHA healing center licensing, STR permits, occupancy limits

## Market Context
{market data from web search if available}

## Analysis Instructions

Perform comprehensive investment analysis following your real-estate-investment-analyst.md framework:

### 1. Valuation Assessment
- Compare list price to recent comparable sales
- Analyze price per square foot vs market average
- Determine if property is overpriced, fair, or underpriced
- Provide specific percentage and reasoning

### 2. Investment Metrics
- Calculate cap rate (if rental income applicable)
- Project cash-on-cash return
- Estimate ROI over 5-year hold
- Calculate DSCR if used for Ceremonia healing center

### 3. Market Analysis
- Assess current market conditions (buyer's vs seller's market)
- Evaluate neighborhood trajectory and appreciation potential
- Compare to recent price trends in area
- Identify market risks and opportunities

### 4. Ceremonia Spaces Fit Analysis
This is CRITICAL - evaluate property specifically for healing center use:

**Zoning & Regulatory:**
- Current zoning designation and allowances
- Healing center licensing feasibility under Colorado NMHA
- STR licensing requirements and restrictions
- Occupancy limits and septic/parking constraints

**Physical Suitability:**
- How many therapy/healing rooms can be configured?
- Common areas for group ceremonies (living room, outdoor space)
- Kitchen facilities for meal preparation
- Parking adequacy for staff + clients
- Privacy and ambiance for healing work

**Financial Viability:**
- Projected revenue: {rooms} × $497/day × {utilization %} × 365 days
- Operating costs: NNN lease, utilities, staff, supplies
- NOI and DSCR calculation
- Revenue coverage of debt service

**Strategic Fit:**
- Location advantages (tourism access, natural setting, proximity to Boulder/Denver)
- Competitive positioning vs other healing centers
- Scalability (can start with 5 rooms, expand to 14?)
- Alignment with business model and expansion strategy

### 5. Risk Assessment
Identify key risks:
- **Market Risk:** Price correction, demand shifts, oversupply
- **Property Risk:** Condition issues, hidden defects, location concerns
- **Financial Risk:** Cash flow gaps, leverage exposure, cost overruns
- **Regulatory Risk:** Zoning changes, NMHA compliance, STR restrictions

### 6. Investment Recommendation

Provide clear rating: **BUY / PASS / INVESTIGATE**

**BUY:** Strong fundamentals, attractive pricing, excellent Ceremonia fit, 12%+ projected returns
**PASS:** Weak fundamentals, overpriced, poor fit, <8% returns
**INVESTIGATE:** Promising but needs more due diligence, 8-12% returns

**Key Strengths (3-5 bullets):**
- Specific advantages and value drivers

**Key Concerns (3-5 bullets):**
- Specific risks and challenges

**Recommended Next Steps:**
1. {Specific action item}
2. {Specific action item}
3. {Specific action item}

## Output Requirements

Generate comprehensive analysis report following this structure:

# Property Investment Analysis: {Address}

## 📋 Property Summary
{Table with key property details}

## 💰 Financial Analysis
{Purchase costs, operating expenses, investment metrics, Ceremonia revenue projections}

## 🏘️ Market Analysis
{Neighborhood overview, comparable sales table, market trends}

## 🎯 Ceremonia Spaces Fit Analysis
{Regulatory compliance, property suitability, business model alignment, revenue potential}

## ⚖️ Investment Recommendation
**Rating:** {BUY / PASS / INVESTIGATE}

**Key Strengths:**
- {Bullet points}

**Key Concerns:**
- {Bullet points}

**Recommendation Summary:**
{2-3 paragraph detailed explanation}

**Suggested Next Steps:**
1. {Action item}
2. {Action item}
3. {Action item}

## 📸 Visual Documentation
{Reference to images folder}

## 📊 Data Sources & Methodology
{Sources, timestamps, disclaimers}

---

**Keep analysis concise but comprehensive:**
- Property assessments: ~50 words each section
- Market analysis: ~100 words
- Focus on actionable insights and specific numbers
- Include concrete financial metrics ($, %, ratios)
- Provide clear reasoning for recommendations
```

**Task Invocation:**
```
Use Task tool with subagent_type: "general-purpose"
Description: "Deep investment analysis for {property address}"
Prompt: {above template with all data filled in}
```

### Step 8: Report Generation & Formatting

The agent will generate the analysis.md content.

Save to: `/docs/{property-slug}/analysis.md`

### Step 9: Completion Summary

Output summary to user:
```
✅ Analysis complete: {property address}

📁 Output saved to: /docs/{property-slug}/
   ├── analysis.md (Investment analysis report)
   ├── property-data.json (Raw Zillow data)
   ├── market-data.json (Comps and trends - if requested)
   └── images/ ({N} property images)

💡 Recommendation: {BUY/PASS/INVESTIGATE} - {Brief reason}
   • {Key insight 1}
   • {Key insight 2}
   • {Key insight 3}

📖 Full analysis: /docs/{property-slug}/analysis.md

⏱️  Completed in {duration}
```

## Error Handling

**If Firecrawl fails:**
- Retry up to 3 times with exponential backoff (1s, 2s, 4s)
- If all retries fail: report error with specific message, suggest manual data entry

**If image download fails:**
- Continue with available images
- Note missing images in analysis report
- Still proceed with analysis (partial success)

**If agent analysis fails:**
- Retry once
- If still fails: generate basic report with available data and metrics

**If property not found:**
- Verify URL is valid Zillow listing
- Suggest checking address or providing alternative URL

## Implementation Notes

**Phase 1 (MVP) Implemented:**
- ✅ Single property analysis
- ✅ Firecrawl integration
- ✅ Image download
- ✅ Basic folder structure
- ✅ Agent invocation
- ✅ Report generation

**Phase 2 (Enhanced) Implemented:**
- ✅ Business model context integration
- ✅ Enhanced error handling with retry logic
- ✅ Market data gathering (--include-comps, --include-market-trends)
- ✅ Formatted analysis reports
- ✅ Update mode (detect existing folders, archive previous analysis)
- ✅ Batch processing support (parse multiple URLs/addresses)

**Batch Processing:**
If multiple properties provided:
- Process each property using above workflow
- Run up to 3 properties in parallel (respect rate limits)
- Generate summary report at end with all recommendations

**Update Mode:**
If `--update` flag present or existing folder detected:
- Check for existing `/docs/{property-slug}/` folder
- If exists: archive current analysis.md → `archive/analysis-{date}.md`
- Re-scrape property data (may have price updates)
- Re-run analysis with fresh data
- New analysis.md can note changes vs previous

## Success Criteria

Command succeeds if:
- ✅ Property data successfully scraped and saved
- ✅ At least 80% of images downloaded
- ✅ Business model context integrated
- ✅ Agent generates investment analysis
- ✅ Analysis report saved to correct location
- ✅ User receives clear summary and recommendation

Partial success (warn but don't fail):
- Some images failed to download (> 50% success)
- Market data unavailable (use Zillow data only)
- Previous analysis missing for comparison (first-time analysis)

Complete failure (report error):
- Cannot scrape property data from Zillow
- Firecrawl API errors after retries
- Invalid property URL or address
- Cannot create output directories

---

**Execute this workflow systematically, providing progress updates at each step.**
