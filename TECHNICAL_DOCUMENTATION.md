# Envisioning Climate Futures — Technical Documentation & Developer Guide

> **Website:** [https://envisioningclimatefutures.org/](https://envisioningclimatefutures.org/)  
> **Repository:** Envisioning Climate Futures  
> **Last Updated:** October 2026  
> **Author / Maintainer:** Envisioning Climate Futures Research Team  

---

## 1. Project Overview & Architecture

### 1.1 Purpose
**Envisioning Climate Futures** is an interdisciplinary research initiative led by the University of Toronto in collaboration with UPEI, Brock University, OCAD University, and Toronto Metropolitan University. The website translates complex climate science, hydrodynamic simulations, and flood/wildfire models into accessible interactive 3D visualizations, games, AR experiences, and case studies for the public, educators, and policymakers.

### 1.2 Architectural Philosophy: Zero-Build Single-File Architecture
The site is intentionally built on a **lightweight, zero-build frontend architecture**:
- **Core Technologies**: HTML5, Vanilla CSS3 (Custom Design System), JavaScript (ES6+).
- **Frontend Library**: React 18 (`react.production.min.js`, `react-dom.production.min.js`) loaded directly via CDN.
- **In-Browser JSX Transpiler**: Babel Standalone (`@babel/standalone`).
- **Typography**: Google Fonts (*DM Sans*).
- **Hosting & Deployment**: Vercel (Automatic git integration).
- **Analytics**: Vercel Web Analytics + OpenPanel.

#### Why This Architecture?
1. **Zero Barrier to Entry**: Collaborators (professors, research assistants, designers, student developers) do not need to install Node.js, npm, Webpack, Vite, or resolve complex dependency conflicts.
2. **Instant Local Preview**: Anyone can double-click `index.html` or launch a simple local server in 1 second.
3. **High Reliability**: No breaking dependency upgrades, zero bundle build failures, and minimal deployment overhead.

---

## 2. Getting Started (Local Development)

### 2.1 Prerequisites
Any standard modern web browser (Chrome, Safari, Firefox, Edge) and a code editor (VS Code, Cursor, etc.).

### 2.2 Running Locally
To avoid browser local file CORS restrictions (e.g., loading local video files or external CDN assets smoothly), running a lightweight HTTP server is recommended:

#### Option A: Python (Built-in on macOS/Linux/Windows)
```bash
# In the project root directory
python3 -m http.server 8080
```
Then open `http://localhost:8080` in your browser.

#### Option B: VS Code Live Server
1. Install the **Live Server** extension in VS Code.
2. Right-click `index.html` -> **"Open with Live Server"**.

#### Option C: Node.js (npx)
```bash
npx serve .
```

---

## 3. Directory & File Structure

```text
├── index.html                   # Core single-page application (All CSS, React Components & Routing)
├── README.md                    # Quick repository introduction
├── TECHNICAL_DOCUMENTATION.md   # Complete technical handover & developer guide (this file)
├── vercel.json                  # Vercel deployment configuration & security headers (HSTS)
├── assets/                      # Local images, posters, videos & PDF reports
│   ├── lakeside-2026.jpg
│   ├── lakeside-2050.jpg
│   ├── brock-virtual-water-futures.mp4
│   ├── north-rustico-hero.jpg
│   ├── fort-mcmurray-aerial.jpg
│   └── ...
├── images/                      # Additional site graphics and illustrations
├── logo/                        # Institution logos (UofT, UPEI, Brock, OCAD, TMU)
└── .gitignore                   # Git ignore configuration
```

---

## 4. Code Architecture in `index.html`

`index.html` contains three main sections:

```text
index.html
├── <head>                   # Meta tags, SEO, OpenGraph/Twitter Cards, CDN scripts, Analytics
├── <style>                  # CSS Design System Tokens, Navigation, Layout Grid, Responsive rules
└── <script type="text/babel">
    ├── Global Utility Components (<ImgCard>, <ExpandableImage>, <Block>, <NavRow>, <Burger>, etc.)
    ├── Navigation Bar (<Navigation />) & Footer (<Footer />)
    ├── General Pages (<LandingPage />, <ClimateChangePage />, <MethodologyPage />, etc.)
    ├── Case Study Case Pages (<ChrisUpei />, <UpeiSimulator />, <GerdDam />, <BrockWaterFutures />, <WatershedGame />, <FortMcMurray />, etc.)
    ├── Who We Are Pages (<TeamPage />, <PartnersPage />, <SponsorsPage />, <ContactPage />, <EventsPage />)
    └── Main Application Entry & Router (<App />)
```

---

## 5. Client-Side Routing & Navigation

The application uses **Hash-based Routing (`window.location.hash`)**:
- Changing URL hash (e.g. `https://domain.com/#team`) renders the corresponding React component.
- The `App` component listens to `hashchange` events:

```javascript
function App() {
    const [page, setPage] = useState('home');

    React.useEffect(() => {
        const handle = () => {
            const h = window.location.hash.replace('#', '') || 'home';
            setPage(h);
            if (!window._preventScrollTop) {
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }
            window._preventScrollTop = false;
            // Analytics tracking
            if (window.op) { try { window.op('trackScreenView'); } catch(e) {} }
            trackEvent('page_view_manual', { page: h });
        };
        window.addEventListener('hashchange', handle);
        handle();
        return () => window.removeEventListener('hashchange', handle);
    }, []);

    // Page rendering switch
    return (
        <div>
            <Navigation />
            {page === 'home' && <LandingPage />}
            {page === 'climate-change' && <ClimateChangePage />}
            {page === 'methodology' && <MethodologyPage />}
            {page === 'chris' && <ChrisUpei />}
            {/* ... other pages */}
            {page === 'team' && <TeamPage />}
        </div>
    );
}
```

---

## 6. How to Add & Extend Content (Step-by-Step Guides)

### 6.1 How to Add a New Project Case Study

Follow these 4 simple steps to add a new project (e.g., `Project 10`):

#### Step 1: Create the Component
In `index.html` inside `<script type="text/babel">`, create your new case study component using the standard layout:

```jsx
function NewProjectCaseStudy() {
    return (
        <>
            <div className="content-page">
                <div className="content-page-header" style={{ backgroundImage: "url('assets/your-hero-image.jpg')" }}>
                    <span className="page-project-badge">Project 10</span>
                    <h1>New Project Title</h1>
                    <div className="page-location">
                        <span>Institution Name (Location)</span>
                    </div>
                </div>
                <SectionTracker name="new-project" />
                <div className="c-group">
                    <Block label="The Problem">
                        <p>Description of the extreme weather problem and environmental hazard...</p>
                    </Block>
                    <Block label="The Solution">
                        <p>Description of the simulation, tool, or visualization methodology...</p>
                    </Block>
                    <Block label="Visualizations & Results">
                        <ImgCard 
                            src="assets/your-figure.jpg" 
                            alt="Simulation output" 
                            caption="Simulation output showing flood levels" 
                        />
                    </Block>
                </div>
                <NavRow 
                    prev={{ href: 'fort-mcmurray', label: 'Fort McMurray' }} 
                    next={{ href: 'team', label: 'Team Members' }} 
                />
            </div>
            <Footer />
        </>
    );
}
```

#### Step 2: Register in the Router (`App`)
In the `App()` component, add your route:
```jsx
{page === 'new-project' && <NewProjectCaseStudy />}
```

#### Step 3: Add to Desktop Mega Menu (`projects-menu`)
In the `<Navigation />` component under `className="nav-dropdown projects-menu"`:
```jsx
<div className="nav-project-card" onClick={() => navigate('new-project')} data-track="nav_new_project">
    <span className="nav-proj-badge">Project 10</span>
    <div className="nav-project-content">
        <div className="nav-project-title">New Project Title</div>
        <div className="nav-project-loc">Institution Name (Location)</div>
    </div>
</div>
```

#### Step 4: Add to Mobile Menu
In `<Navigation />` under mobile drawer's `openSection === 'ourwork'`:
```jsx
<a className="nav-dropdown-item" onClick={() => navigate('new-project')} data-track="nav_new_project">
    Project 10: New Project Title
</a>
```

---

### 6.2 How to Update or Add Team Members

All team members are managed in the `groups` array inside `TeamPage()` in `index.html`:

```javascript
const groups = [
    {
        category: "Research Steering Committee", 
        accent: "blue", // "blue" or "green"
        members: [
            { 
                name: "Full Name", 
                title: "Research Lab / Role", 
                dept: "Department Name (optional)", 
                institution: "University of Toronto", 
                photo: "https://res.cloudinary.com/.../photo.jpg", // or "assets/photo.jpg"
                photoPos: "top center" // optional CSS object-position (e.g., 'center', 'top center')
            },
            // ...
        ]
    },
    // ...
];
```

> **Tip:** If a member does not have a photo (`photo: null` or empty), the system automatically renders an initials avatar badge with the designated category gradient color.

---

### 6.3 How to Add Images & Videos

#### 1. Expandable Image (Click-to-Zoom Lightbox)
Use the `<ExpandableImage>` component for posters, infographics, and high-resolution diagrams:
```jsx
<ExpandableImage
    src="assets/my-poster.jpg"
    alt="Poster description"
    style={{ width: '100%', borderRadius: '8px' }}
/>
```

#### 2. Image Card with Caption
```jsx
<ImgCard 
    src="assets/storm-surge.jpg" 
    alt="Storm surge in PEI" 
    caption="Observed storm surge during Hurricane Fiona (2022)" 
/>
```

#### 3. Video Player (Local MP4)
```jsx
<div style={{ width: '100%', borderRadius: '12px', overflow: 'hidden', background: '#000' }}>
    <video
        src="assets/demo-video.mp4"
        controls
        playsInline
        preload="metadata"
        style={{ width: '100%', display: 'block' }}
        onPlay={() => trackEvent('case_video_play_clicked', { location: 'demo_video' })}
    />
</div>
```

#### 4. YouTube Embedded Video (Responsive 16:9)
```jsx
<div style={{ position: 'relative', paddingBottom: '56.25%', height: 0, overflow: 'hidden', borderRadius: '12px', background: '#000' }}>
    <iframe
        style={{ position: 'absolute', top: 0, left: 0, width: '100%', height: '100%', border: 0 }}
        src="https://www.youtube.com/embed/VIDEO_ID"
        title="Video title"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowFullScreen
    />
</div>
```

---

## 7. Design System & CSS Tokens

All global styles and CSS variables are located in `<style>` at the top of `index.html`.

### 7.1 Primary Color Tokens
| CSS Variable | Hex / Value | Usage |
| :--- | :--- | :--- |
| `--primary` (`--blue`) | `#163f59` | Primary deep ocean blue for headings, text & brand |
| `--primary-mid` (`--blue-mid`) | `#215a7f` | Mid-tone blue for card borders, active links |
| `--accent` | `#b54a16` | Energetic terra-cotta orange for highlights, CTAs, hover states |
| `--bg` | `#f9fbfd` | Off-white subtle clean background |
| `--ink` | `#111e26` | Dark charcoal for high-contrast body text |
| `--ink-mid` | `#3d505c` | Secondary body text and labels |
| `--rule` | `rgba(22, 63, 89, 0.08)` | Section divider lines |

### 7.2 Core Layout Components
- `.content-page`: Universal page wrapper with `max-width: 1240px` and responsive horizontal padding.
- `.content-page-header`: Full-width dark blue gradient hero banner with glowing bottom edge accent line.
- `.c-block`: Two-column split layout (`240px` label on the left + `1fr` content on the right).
- `.c-block.full`: Single-column full-width block.
- `.nav-dropdown`: Desktop floating dropdown menu with smooth slide transitions.
- `.nav-dropdown.projects-menu`: 3x3 Mega menu grid with project badges and institution labels.

---

## 8. Analytics & Telemetry

The site includes dual-layer privacy-friendly telemetry:

### 8.1 Providers
1. **OpenPanel Analytics** (`client_id: caf932a7-e314-4c2b-a65c-78dccf25b0c5`): Automatically tracks screen views, session durations, and custom user interactions.
2. **Vercel Web Analytics** (`/_vercel/insights/script.js`): Provides deployment-level performance metrics, visitor counts, and page load analytics.

### 8.2 Custom Event Tracking
Call `trackEvent(eventName, properties)` anywhere in React components:
```javascript
// Example: Tracking an external link or download
trackEvent('external_link_clicked', {
    url: 'https://itch.io/...',
    title: 'Watershed Game'
});
```

Common standard events:
- `page_view_manual`: Triggered on route/hash change.
- `nav_click`: Triggered when clicking header menu links.
- `case_video_play_clicked`: Triggered when users play simulation videos.
- `play_game_clicked`: Triggered when users click the itch.io playable link.
- `image_modal_opened`: Triggered when expanding poster images.

---

## 9. Deployment & Production Release

### 9.1 Vercel Git Integration
The live site is deployed via **Vercel**. Every push to the `main` branch automatically triggers an instant production deployment.

```bash
# Push your changes to trigger automatic deployment
git add .
git commit -m "feat: add new research case study"
git push origin main
```

### 9.2 Security & Headers (`vercel.json`)
The `vercel.json` file enforces strict HTTP security headers:
- `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`

---

## 10. Summary Checklist for Handover

When adding new content to the website:
- [ ] Added new images/media to `assets/` or Cloudinary CDN.
- [ ] Created or updated the page component in `index.html`.
- [ ] Registered the page route in `App()`.
- [ ] Added navigation links in both desktop and mobile menus.
- [ ] Checked `<NavRow>` previous and next buttons to maintain cohesive page chaining.
- [ ] Tested responsiveness on desktop (>=1200px), tablet (768px-1024px), and mobile (<=480px).
- [ ] Verified that image zoom and video playback work as expected.
- [ ] Pushed changes to `main` branch to trigger live Vercel deployment.
