# ngaitipos

## Overview

**ngaitipos** is a lightweight, offline‑capable Point‑of‑Sale (POS) web application. It runs entirely in the browser using IndexedDB (via Dexie.js) to store products, customers, orders, and configuration data. The app is built with the **Owl** component framework and includes a service worker for Progressive Web App (PWA) support, allowing it to be installed on desktop or mobile devices and operate without an internet connection.

## Key Features

- **Offline‑first**: All data is persisted locally in IndexedDB; a service worker caches the UI assets.
- **Multi‑tab cart management**: Users can open multiple order tabs simultaneously.
- **Dynamic product catalog** with categories, pricing, and stock tracking.
- **Pricelist engine** supporting percentage‑based discounts per product or category.
- **Customer management** with optional pricelist assignment.
- **Payment handling** supporting cash, bank transfer, e‑wallet, and credit card.
- **Database migration system** (up to version 6) handling schema evolution automatically.
- **Demo data seeding** for quick testing.
- **Responsive UI** that adapts to desktop, tablet, and mobile viewports.
- **Print‑ready receipts** generated via `html2canvas`.

## Tech Stack

- **HTML5 / CSS3** – UI layout and styling.
- **JavaScript (ES6+)** – Core application logic.
- **Owl.js** – Lightweight component framework used for reactive UI.
- **Dexie.js** – Promise‑based wrapper for IndexedDB.
- **html2canvas** – Capture receipts as images for printing.
- **Service Worker** – Implements caching for offline PWA functionality.
- **Font Awesome** – Icon set.

## Project Structure

```
ngaitipos/
├─ index.html                # Entry point, loads Owl, Dexie, and app scripts
├─ manifest.json             # PWA manifest
├─ service-worker.js         # Caches assets for offline use
├─ src/
│  ├─ icons/                # App icons for various sizes
│  ├─ lib/                  # Third‑party libraries (owl.js, dexie.js, html2canvas, Font Awesome)
│  └─ services/
│     └─ db_service.js      # IndexedDB schema, migrations, import/export, seeding
└─ README.md                 # This documentation
```

## Installation & Running

The application is static and does not require a build step. To run it locally:

```bash
# Clone the repository
git clone https://github.com/yourusername/ngaitipos.git
cd ngaitipos

# Serve the directory with any static HTTP server (e.g., Python, http‑server)
# Python 3 example
python -m http.server 8080
# Then open http://localhost:8080 in a browser
```

> **Note:** Accessing the app via `file://` URLs may restrict IndexedDB usage in some browsers. Use a local server as shown above.

## Usage

1. **Open the app** in a modern browser (Chrome, Edge, Firefox, Safari).
2. On first load you will be prompted to **load demo data** – choose *Yes* for a quick start.
3. The **navbar** provides quick actions (settings, sync, etc.).
4. Use the **category filter** to browse products, click a product to add it to the current order.
5. Manage multiple orders via the **order tabs** at the top of the screen.
6. Adjust quantities, prices, or discounts using the **numpad**.
7. When ready, proceed to the **payment screen** to record payments and print a receipt.

All changes are saved automatically to IndexedDB. You can export the database via the *Export* button (implementation located in `db_service.js`) and import a previously exported JSON file.

## Database Details

The app uses Dexie.js to define a versioned IndexedDB schema. Schemas evolve through versions 1 → 6, adding tables such as `pricelist_items`, `pos_orders`, `customers`, and extending primary keys to auto‑increment for consistency.

Key tables:
- `products` – product information, stock, pricing.
- `categories` – hierarchical categorisation.
- `pricelists` & `pricelist_items` – discount rules.
- `customers` – optional customer records linked to a pricelist.
- `orders`, `orderlines`, `payments` – transactional data synchronized with a backend (not included).
- `pos_orders` – local cart representation enabling multi‑tab usage.
- `config` – key/value store for POS settings (currency, rounding, etc.).
- `sync_queue` – queue for pending sync operations.

The `DBMigration` class provides:
- `exportDB()` – serialises the entire database to JSON.
- `importDB()` – imports JSON data, optionally clearing existing tables.
- `clearDB()` – wipes all tables and resets the initialization flag.
- `getStats()` – returns record counts per table.

## Development Notes

- **Adding new tables**: Increment the Dexie version number and call `db.version(x).stores({ … })` with the new schema. Existing data will be migrated automatically.
- **Seeding data**: Modify `MOCK_DATA` in `db_service.js` to change the default demo dataset.
- **Service worker**: Update `CACHE_NAME` and `ASSETS_TO_CACHE` when adding new static assets.
- **Styling**: All CSS lives inside `index.html` inside a `<style>` tag. Adjust as needed for branding.

## Contributing

Contributions are welcome. Follow these steps:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/foo`).
3. Make your changes, ensuring the UI remains responsive and the IndexedDB schema stays consistent.
4. Run the app locally to verify functionality.
5. Commit with clear messages and push.
6. Open a Pull Request describing your changes.

Please adhere to the existing coding style (ES6 modules, consistent indentation, descriptive variable names) and update the README if you add new features.

## License

This project is licensed under the **MIT License** – see the `LICENSE` file for details.


## Description

ngaitipos is a ... (provide a brief overview of the project's purpose and functionality).

## Installation

```bash
# Clone the repository
git clone https://github.com/okkype/ngaitipos.git
cd ngaitipos

# Install dependencies
npm install  # or appropriate package manager
```

## Usage

```bash
# Run the application
npm start  # or the command used to start the project
```

Provide usage examples here, demonstrating the main features or commands.

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Commit your changes with clear messages.
4. Open a pull request describing your changes.

Please ensure code follows the existing style guidelines and includes appropriate tests.
