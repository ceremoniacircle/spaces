# /generate-property-deck

## Description
Generates a complete single-page investment deck HTML file for any Ceremonia Spaces property. Analyzes property data from markdown files, integrates images, and applies the Ceremonia design system to create a production-ready, responsive investment presentation.

## Usage
```bash
/generate-property-deck property_path="docs/11042-n-saint-vrain-dr-lyons-co-80540/" deck_type="str-hybrid"
```

## Parameters
- **property_path** (required): Path to property data folder containing analysis.md, business-model-framework.md, and optional images/
- **output_path** (optional): Destination folder, defaults to `{property-slug}-deck/`
- **deck_type** (optional): "str-hybrid" | "healing-center" | "convertible-note" | "loan" - determines financial model emphasis

## Expected Input Structure
```
docs/{property-folder}/
├── analysis.md              # Real estate investment analysis
├── business-model-framework.md  # Ceremonia business model framework
├── gemini-property-info.md      # Supplementary property details (optional)
└── images/                      # Property photos (optional)
    ├── 01.jpg
    ├── 02.jpg
    └── ...
```

## Output
- Complete, self-contained HTML file with embedded CSS and JavaScript
- Production-ready responsive design (mobile, tablet, desktop)
- All interactive elements functional (charts, calculators, toggles)
- SEO meta tags, Open Graph tags for social sharing
- Performance optimized (<2 second load time excluding images)

## Implementation Logic

### Step 1: Parse Property Data
Extract structured information from markdown files:
- **From analysis.md**: Property specs, financials, market data, risks, valuation
- **From business-model-framework.md**: Revenue models, operating structure, regulatory framework
- **From gemini-property-info.md**: Operating model specifics, execution timeline

Key data points to extract:
- Property: address, size, bedrooms, bathrooms, lot size, year built, features
- Pricing: list price, purchase price, comparable data
- Financials: revenue scenarios, DSCR, cash-on-cash returns, operating expenses
- Market: demand drivers, competition, location advantages
- Risks: Primary concerns and mitigation strategies

### Step 2: Apply Ceremonia Design System
Implement core brand elements:
- **Colors**: #65C5B2 (teal), #6283C2 (cerulean), #3D352E (text), #F5F3FF (bg)
- **Fonts**: Lora (body), Public Sans (headings) via Google Fonts CDN
- **Components**: Stat cards, tables, section alternation, hover effects
- **Responsive**: Mobile-first with Tailwind CSS breakpoints

### Step 3: Generate Content Sections
Map property data to standard investment deck sections (matching investment-deck/index.html structure):

1. **Investment Ask Section**:
   - Headline with investment opportunity type (e.g., "Looking for 2-3 Investors to Co-Sign on Mortgage")
   - Key financing stats in stat cards: Down Payment, Monthly Payment, Annual Debt Service, LTV
   - Financing structure with Sources (mortgage, down payment) and Uses (purchase, closing, CapEx)
   - Zillow listing link button
   - Investment terms summary (loan type, interest rate, amortization, DSCR)

2. **Hero Section**: Property headline, location, key property stats (sqft, beds/baths, lot size, year built)

3. **Property Overview**:
   - Location map
   - Property specs table
   - **Modal Photo Gallery** (ALL images from images/ folder) - thumbnails open full-size images in modal overlay
   - Unique selling points

4. **Financial Analysis**: Acquisition costs, OpCo income, revenue models, LandCo returns, key metrics

5. **Investment Thesis**: Strengths (green), Concerns (yellow), Recommendation summary (blue)

6. **Next Steps**: 90-day execution timeline with week-by-week tasks

7. **Market Context**: Healing center market stats + Gateway tourism market stats

8. **Business Context**: The Colorado Psychedelic Infrastructure Crisis

9. **The Ceremonia Solution**: Premier asset positioning, turnkey compliance, market share chart

10. **The Ceremonia Ecosystem**: 3-entity structure (Journeys, EDU, Spaces)

11. **Legal & Entity Structure**: LandCo, OpCo, Holdings explanation

12. **Founder & Vision**: Austin Mao bio with credibility markers and highlight cards

13. **Due Diligence**: Link to full analysis document (if available)

14. **Contact**: Contact info, investment invitation

### Step 4: Build Interactive Elements

**Modal Photo Gallery** (REQUIRED):
- Grid of thumbnail images (4-5 columns responsive)
- Click thumbnail to open full-size image in modal overlay
- Modal features: dark backdrop, close button (X), previous/next navigation arrows, image counter
- Keyboard navigation: ESC to close, arrow keys for prev/next
- Touch swipe support for mobile
- Implementation: Pure CSS/JS (no external dependencies) using W3Schools approach
- CSS: `.modal { display: none; position: fixed; z-index: 999; width: 100%; height: 100%; background: rgba(0,0,0,0.9); }`
- JS: Click handlers for thumbnails, navigation buttons, and close

**Financial Calculator** (if deck_type includes financial modeling):
- Dynamic input controls (purchase price, financing terms, utilization rates)
- Real-time calculation of DSCR, cash-on-cash returns, NOI
- Scenario toggle buttons (Low/Base/High)
- Tabbed views (Summary/Annual/Details)

**Chart.js Visualizations**:
- Revenue projections chart (bar + line combo)
- Market share pie/donut chart
- Multi-year trend line chart
- All charts responsive with proper labels and legends

**Collapsible Sections**:
- Detail expansions with smooth CSS transitions
- "Show/Hide" toggle text updates
- max-height animation approach

### Step 5: Quality Assurance Checklist
- [ ] All data accurately sourced from input files
- [ ] Investment Ask section prominently displayed at top with financing details
- [ ] Modal photo gallery implemented with ALL images from images/ folder
- [ ] Modal navigation working (prev/next arrows, keyboard ESC/arrows, close button)
- [ ] Zillow listing link in Investment Ask section
- [ ] Financing structure shows Sources and Uses tables
- [ ] Current mortgage rates and down payment from agent research
- [ ] All business context sections from investment-deck/index.html are included
- [ ] No placeholder text or broken links
- [ ] Responsive design tested (mobile, tablet, desktop)
- [ ] Interactive elements functional (modal, calculators, charts)
- [ ] Thumbnail images display in grid (not full gallery on page)
- [ ] Chart.js renders correctly (if applicable)
- [ ] Financial calculations accurate
- [ ] Meta tags populated
- [ ] Performance optimized

## Example: Saint Vrain Property

**Input Files:**
- `docs/11042-n-saint-vrain-dr-lyons-co-80540/analysis.md`
- `docs/11042-n-saint-vrain-dr-lyons-co-80540/business-model-framework.md`
- `docs/11042-n-saint-vrain-dr-lyons-co-80540/gemini-property-info.md`

**Key Data Extracted:**
- Property: 5,512 sqft, 4bd/4ba, 12.82 acres, Lyons CO
- Price: $1,399,000
- Model: STR + Healing Center hybrid
- Revenue: $537K-$906K/year (Low-High)
- Returns: 12-22% cash-on-cash
- Key Risk: Flood/wildfire, regulatory approval

**Generated Deck Sections:**
- Hero: "Saint Vrain Healing Retreat: Premium Gateway Investment"
- Property showcase with creek/mountain photos
- Dual revenue calculator (STR nights + therapy rooms)
- RMNP gateway market analysis (4.15M annual visitors)
- Boulder County STR compliance section
- Risk mitigation strategies table
- 90-day execution timeline
- Contact Austin CTA

## Technical Stack
- HTML5 semantic markup
- Tailwind CSS via CDN (no build process)
- Chart.js via CDN for data visualization
- Vanilla JavaScript ES6+
- Google Fonts (Lora + Public Sans)
- Responsive images with lazy loading

## Limitations & Constraints
- Single HTML file output (no separate CSS/JS files)
- Total file size target: <500KB excluding images
- No server-side processing required
- Works in modern browsers (Chrome, Firefox, Safari, Edge)
- Print-optimized styles included

## Version History
- v1.0 (October 2025): Initial command creation based on investment-deck and loan-deck reference templates

## Maintainer
Austin Mao / Ceremonia Spaces
austin@ceremoniacircle.org
