# Envisioning Climate Futures

Welcome to the **Envisioning Climate Futures** project repository. This website is designed to build computer simulation models of extreme weather events and translate them into accessible experiences to help individuals and communities understand past events and visualize future impacts.

🌍 **Live Website:** [https://envisioningclimatefutures.org/](https://envisioningclimatefutures.org/)

---

## 🛠 Technology Stack

This project is built using a lightweight, no-build-step architecture. It leverages modern frontend tools without requiring a complex Node.js build environment:

- **Core**: HTML5, Vanilla CSS3
- **Framework**: React.js 18 (Imported via CDN)
- **Compiler**: Babel Standalone (Compiles JSX in the browser)
- **Hosting / Deployment**: [Vercel](https://vercel.com/)
- **Analytics**: Vercel Web Analytics & OpenPanel

> 📘 **Developer & Handover Guide:** For comprehensive technical documentation, code structure maps, and step-by-step instructions on how to add new research projects, team members, and media assets, please refer to [TECHNICAL_DOCUMENTATION.md](file:///Users/ariazhengchuxuan/project%20website%20test/TECHNICAL_DOCUMENTATION.md).

## 🚀 Getting Started (Local Development)

Because this project uses React via a CDN and Babel Standalone, there is no need for `npm install` or `npm run dev`.

### Running the site locally
1. Clone this repository.
2. Simply double-click on `index.html` to open it in your browser.
3. **Alternative (Recommended)**: For the best experience and to avoid any local CORS issues with external assets, serve it using a lightweight local server.
   - Using Python: `python -m http.server 8000`
   - Using Node: `npx serve .`

## 📁 Project Structure

- `index.html`: The core of the application. It contains all the CSS styling (`<style>`), the React components (e.g., `<LandingPage />`, `<MethodologyPage />`), and the application's client-side routing logic.
- `vercel.json`: Configuration file setting up HTTP Strict-Transport-Security (HSTS) headers and deployment settings for Vercel.
- `images/`: Local assets and photographs used across case study pages.
- `logo/`: Institutional and partner logos.

## 📊 Analytics Tracking

The website is actively monitored using two robust analytics solutions:
1. **OpenPanel Analytics**: Configured in `index.html` `<head>` measuring screen views, outgoing links, and custom user attributes.
2. **Vercel Web Analytics**: Native tracking implemented via the `/_vercel/insights/script.js` script tag to map seamlessly with the Vercel deployment dashboard.

## 🤝 Case Studies & Features

Current modules featured on the platform:
- Climate Hazard & Risk Information System, PEI (UPEI)
- Coastal-Inland Flood Simulator, PEI (UPEI)
- Flooding of the Don River, Toronto (University of Toronto)
- Flooding of Little Etobicoke Creek, Mississauga (University of Toronto)
- Flooding of North Rustico PEI in Hurricane Fiona, 2022 (University of Toronto)
- Fort McMurray AB Wildfire, 2016 (University of Toronto)
- Grand Ethiopian Renaissance Dam (University of Toronto)
- Virtual Water Futures, St. Catharines ON (Brock)
- Watershed: An Educational Flooding Game (University of Toronto)