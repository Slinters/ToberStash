# Changelog

ToberStash versions follow the development of one product. Nothing was tagged before 0.9.0, so the work from 15 September 2026 to 18 October 2026 is collected here.

## 0.9.2 — 2026-10-20

- Several people can share one database. Tick it under System → Database and set your name on this PC. That name is written on the program log and the stock log. Opening a component, purchase, supplier, or list locks it until the window closes. A project build is locked only with Lock to me, and released with Release. System → Locks lists locks that were left behind. Work offline in that mode opens a read-only copy and deletes it when you go back online. The server is not replaced. Single-user Work offline still copies both ways.

## 0.9.1 — 2026-10-19

- Work offline copies a server database onto this PC and copies it back when you choose Work online. The mode is shown at the bottom of the sidebar. The button stays off for SQLite, and for a server marked as running on this computer. A copy with about 150 MB of photos and documents asks first. Around 1 GB it is refused.
- Excel and CSV import keeps the Browse button beside the file path.
- Advanced part lookup still lists the fields after the program has been protected with .NET Reactor.
- Mouser, DigiKey, and Nexar search requests keep their field names after that protection.
- Distributor lookup waits 30 seconds for a response.

## 0.9.0 — 2026-10-18

First numbered version, prepared for use. The next public build can be tagged 1.0.0.

### Inventory

- Component list with search, stock, low-stock marking, and storage locations.
- Choose which columns the inventory table shows.
- Excel import and export.
- Photos, datasheets, and other files stored on the component.
- MPN lookup through Mouser, DigiKey, TME, and Nexar. API keys stay on the PC, encrypted for the Windows account.

### Projects

- BOM builds with part matching, shortages, and a guided build.
- A build can start before serial numbers exist. The project cannot be finished until they are entered.
- Scrap of parts on the BOM, and of parts that were not on the BOM.
- Test protocol defined under Settings → Lists: ordered steps, an instruction on each step, and a comment when the test is run.
- Build-report PDF, with a progress indicator while the file is written.
- Export sheet for origin, HS, TARIC, HTSUS, and ECCN.

### Purchases

- Purchase orders, receiving, and supplier comparison.
- Status filters.
- The project list and the purchase list can be sorted by last saved, or chronologically with the newest created row first.

### System

- SQLite by default. MySQL, MariaDB, or SQL Server when a server is configured.
- JSON backup while the app is open.
- Move the component list to or from InvenTree and Part-DB.
- Themes, fonts, and twelve languages: English, Swedish, German, French, Spanish, Polish, Danish, Norwegian Bokmål, Finnish, Hindi, Simplified Chinese, and Japanese.
- Display currencies include Chinese yuan, Japanese yen, and Indian rupee. DigiKey prices can be fetched in those currencies.
- About page with a short disclaimer and the third-party copyright and license notices.
