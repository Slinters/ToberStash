# ToberStash wiki

How to use ToberStash 0.9.2. Menu names below are the English ones. **Settings → Language** shows the same commands in the language you pick.

ToberStash keeps a component inventory, the projects that consume it, and the purchase orders that refill it. The first start creates an empty inventory in `%LocalAppData%\ToberStash`. Photos, datasheets, purchase documents, and the local database stay in that folder. Nothing is written into the folder you extracted.

## Contents

- [The window](#the-window)
- [To do](#to-do)
- [Components](#components)
- [Storage locations](#storage-locations)
- [Import and export of the inventory](#import-and-export-of-the-inventory)
- [MPN lookup](#mpn-lookup)
- [Price update](#price-update)
- [From a component to a purchase](#from-a-component-to-a-purchase)
- [Suppliers](#suppliers)
- [Purchases](#purchases)
- [Projects](#projects)
- [BOM import](#bom-import)
- [Matching](#matching)
- [Adding and editing lines](#adding-and-editing-lines)
- [Ready to build](#ready-to-build)
- [The build](#the-build)
- [Build photos](#build-photos)
- [Test protocol](#test-protocol)
- [Running the test](#running-the-test)
- [Export sheet](#export-sheet)
- [Build report](#build-report)
- [Scrap](#scrap)
- [Settings](#settings)
- [Database, backup, and moving the list](#database-backup-and-moving-the-list)

## The window

The sidebar is the map of the program.

| Sidebar | What it opens |
| --- | --- |
| To do | Work that is waiting |
| Inventory | The component list |
| Projects | Builds and their BOMs |
| Purchases | Purchase orders |
| Suppliers | Supplier cards |
| Import | The Excel and CSV import wizard |
| Settings | Language, money, lists, API keys |
| System | Database, backup, exchange, logs, About |

The footer shows the part count and which database is open. The backup badge above the logo shows the last automatic backup. Click it to open **System → Backup**.

**Inventory** is the list you live in. The boxes at the top count unique parts, total stock, stash value, and how many parts are low or empty. The search box matches MPN, name, type, value, footprint, location, and supplier. Ctrl+F focuses it. Filters narrow the list to one type, one location, low stock, unverified, verified, or hide discontinued parts.

**More** on the inventory toolbar holds **Fetch missing**, **Update prices**, **Import Excel**, **Export .xlsx**, and **Export PDF**.

## To do

**To do** is a short list of things that need a person. Double-click a row to open it.

- **Below threshold.** A part on the purchase watch, not discontinued, with nothing already incoming, and with stock at or below its reorder point.
- **Purchase overdue.** An order that is not closed, still has something to receive, and whose requested delivery date is before today.
- **Waiting for test.** A project whose build has been run and which is not yet completed.
- **Ready to build.** A project that still has lines, every line is matched, the build has not been run, and stock covers the number of builds on the project.

An empty list means nothing in those four groups is waiting.

## Components

**Add component** opens a card in four steps: identify, describe, stock, documents. A component needs an MPN or a name, a type, and a location before it is complete. A datasheet or the verified tick completes the document step.

The card is grouped into identity, stock and location, technical data, customs and compliance, and links and notes.

Identity and technical fields:

- MPN, name, manufacturer, type, and class. Type is the specific category, such as Resistor or Connector. Class is the broader group, such as Mechanical, Module, or Component. Both lists are edited under **Settings → Lists**.
- Value, footprint, and detailed specifications. Footprint is meant as TH or SMD plus a size. A lookup may fill a packaging note such as tape and reel, which is a sign the footprint still wants a real size.
- Schematic designator and internal reference. The internal reference is your own id. The designator is the schematic name, such as R12.
- Alternate MPNs. A later import or match can find the part through an alternate.

Stock:

- On-hand quantity. **Adjust stock** sets a new quantity and writes the difference, with an optional note, to the stock log.
- Reorder point. Zero means the part uses the global low-stock threshold in **Settings → Stock**. Any other number applies only to this part.
- **Included in purchase watch.** On by default. A discontinued part drops out of the purchase watch even if the box stays ticked.
- **Serial-tracked part.** Tick this when each physical unit has its own serial, for example a module. The serial itself is entered on the project, one row per module.

Compliance and documents:

- Country of origin, HS, TARIC, HTSUS, and ECCN.
- RoHS / REACH status, and separate URL or local PDF slots for RoHS, REACH, and one shared RED / EMC / RF declaration.
- Datasheet URL and a local datasheet PDF.
- **Verified (OK)** is your own check. It is independent of RoHS and REACH.
- **Discontinued / obsolete** hides the part from the purchase watch. The inventory filter can hide discontinued rows.
- **Flag** plus a comment. Flagged parts are left out of **Fetch missing** and **Update prices**.

Suppliers on the card hold a supplier name, that supplier's part number, a price, and a currency. The preferred supplier is the one a new purchase line follows. MOQ, when set, raises a purchase quantity up to that minimum.

**Duplicate** copies a component so you can change the MPN. **Delete** removes it after a confirm. **Stock history** lists every movement: import, manual add, adjustment, receipt, build, undo, scrap, and merge.

**Merge duplicates** finds the same MPN on more than one row. You pick the row to keep. Stock, empty fields, documents, and orders move onto it, and the other rows are deleted.

Click a supplier name in the list to open that supplier's website when one is stored. The PDF column opens a local file when you have saved one, and the online document otherwise.

**Columns** chooses which fields the inventory table shows, and in which order. OK and PDF stay first. **Default** restores the original set.

## Storage locations

A location is a name on the component, up to three levels deep: a row, a shelf, and a box. It is the place you walk to. Stock is still one quantity on the component.

Open **Settings → Lists → Locations**.

- **New level 1** adds a top location.
- **New below** adds a child under the selected location.
- Drag a location onto another, or pick where it sits and click **Move**. Parts stored there follow, including everything below it.
- A location cannot be moved into itself or into something under it.
- Three levels is the maximum. Remove children before removing a parent.
- A name cannot contain `›`.

The inventory filter **All locations** lists these names. The project pick list sorts by row, shelf, and box.

## Import and export of the inventory

### Excel and CSV import

**Import** in the sidebar, or **More → Import Excel**, opens the wizard.

1. Choose an `.xlsx` or `.csv` file. If `BOM-Inventarie.xlsx` is found, the wizard offers that file.
2. The preview shows mapped columns, headers it ignored, how many rows are ready, and rows that have no MPN, name, or reference.
3. **Overwrite stock quantity from file** is optional. Leave it off when the file is a catalog and the quantities in ToberStash are the ones you trust.
4. Import writes the rows.

A row updates an existing component when it matches, in this order:

1. The legacy BOM-ID, when the file has one.
2. MPN together with the internal reference, when both are present.
3. The MPN, including an alternate MPN already stored on a component.
4. The internal reference on its own.

A new row is inserted when none of those hit. Empty fields on an existing component are filled from the file. A price in the file replaces the stored price when the file price is greater than zero. Stock changes only when you asked for it. New types are added to the type list.

The wizard understands the column names from `BOM-Inventarie.xlsx` and the usual English names (`MPN`, `Manufacturer Part Number`, `Quantity`, `Location`, and so on). A header it does not know is ignored.

**Export .xlsx** writes the inventory back in that same column layout, including price currency, flag, and flag comment. **Export PDF** writes a readable list of the same inventory.

### JSON backup

**System → Database → Export backup** writes a JSON file of the whole database: components, locations, suppliers, projects, tests, scrap, purchases, and the files attached to them. **Import backup** replaces the data in the database you are connected to. Export a backup before you switch engines.

**System → Backup** runs the same kind of copy while ToberStash is open: at startup when one is due, on a daily or weekly timer, and at exit when a copy is overdue. You choose the folder and how many copies to keep. No Windows scheduled task is required. **Back up now** runs one immediately.

### Part-DB and InvenTree

**System → Exchange** moves the component list only.

- **To Part-DB** writes a semicolon-separated file.
- **To InvenTree** writes a comma-separated file.
- Prices are written as stored.
- **From Part-DB** and **From InvenTree** read a file of the same shape. Unknown columns are skipped. Stock quantities already in ToberStash are kept.

Projects, purchase orders, tests, and order documents stay in ToberStash. Bring a component list back from the same menu.

## MPN lookup

Lookup fills a component from a distributor search on its MPN.

Sources:

- **LCSC** works with no key.
- **Mouser** needs an API key.
- **DigiKey** needs a client id and a client secret.
- **TME** needs a token and a secret.
- **Nexar** needs a client id and a client secret.

Enter the keys under **Settings → API** and save. They stay on this PC, encrypted for the Windows account. They are not stored in the inventory database. On another computer, enter them again.

**Preferred source** is where a simple fetch starts. Every configured source still runs afterwards. A later source only fills fields the earlier one left empty.

On the component card, **Auto-fetch MPN** offers to fill empty fields. Existing text is left as it is. The offer can be turned off with **Offer auto-fetch when an MPN is typed**.

Fields a successful lookup may fill when they are empty: manufacturer, name, MPN, datasheet, RoHS and REACH, image, footprint, type, value, specifications, ECCN, HS, TARIC, HTSUS, country of origin, unit price and its currency, price breaks, MOQ, and the supplier's part number.

**Advanced lookup** picks one source and shows the values it returned next to what is already on the form. Tick **Use** on a value to write it. That path can replace a value that is already there.

**More → Fetch missing** walks the components in the current view. It skips flagged rows and rows with no MPN. It only fetches a part that still has an empty lookup field, or a part that has an image URL and no saved image yet. You confirm the count before it starts. One failed part does not stop the rest.

## Price update

**More → Update prices** looks up every non-flagged part with an MPN in the current view and bypasses the recent cache.

When the returned unit price differs from the stored price in your display currency, the part is listed with the old price and the new price. You choose which rows to save. The new amount is stored in the currency the source returned.

Price breaks, MOQ, and supplier part numbers can be written even when the unit price itself did not change. A part whose display price is already the same is left off the confirm list.

Prices from APIs are exclusive of VAT. **Settings → Economy → Show prices including VAT** only changes what you see on cards, the inventory, stash value, and projects.

## From a component to a purchase

On the component, **Add to purchase** asks how many to order and adds that quantity to the open purchase order for the part's preferred supplier. Parts keep collecting on that order until you mark it ordered. The next part for the same supplier joins the same open order. A different supplier gets its own open order.

If the part has an MOQ, the quantity is raised to at least that MOQ.

**More** is not required for a single part. Two inventory actions cover a whole set:

- **Shortages to purchase** is on the project. It adds each short, matched line to an open order, grouped by preferred supplier.
- **All below threshold to purchase** is on the inventory. It orders enough to reach the reorder point for every watched part that is low. A part that is already on an open, ordered, or partly received order is skipped. The quantity is at least one, and at least the MOQ.

The **Incoming** column shows how many are still to receive. A part with incoming quantity is left off the low-stock to-do row.

## Suppliers

**Suppliers** is the address book. A card stores name, website, email, phone, contact, address, city, postal code, and notes. **Card** on a row opens it.

The commercial link lives on the component: each supplier can have its own part number and its own price. **Compare prices** on a purchase order reads those saved prices. When a line has no supplier price, the comparison uses the component price for the preferred supplier.

Rename and add suppliers from the card, or by typing a supplier on a component. **Settings → Lists** does not hold the supplier address book. It holds types, classes, locations, and the test protocol.

## Purchases

A purchase order is **Open**, **Ordered**, or **Closed**.

- **Open.** Lines can be added, removed, and moved. Quantities can change. The order can be deleted.
- **Ordered.** The order is placed. Further parts for that supplier start a new order. Lines on an ordered purchase stay as they were.
- **Closed.** Every line has been fully received.

On the order you can set:

- The supplier for the whole order. Line prices stay as they are. Part numbers update where the component has one for the new supplier. If that supplier already has an open order, the lines move there and this order is removed.
- A requested delivery date. The to-do list uses it for overdue orders.
- A tracking link.
- An order note.
- Documents: PDF, Excel, CSV, Word, images, and mail (`.eml`, `.msg`), up to 20 MB each, with a comment per file.

**Receive line** adds the arrived quantity to stock and writes a receipt in the stock log. **Close order** is available when every line is fully received.

**Move line** sends one line to another open order. The same part is combined. You can also start a new order for a supplier.

**Compare prices** totals the order from the saved price at each supplier and marks the cheapest. **Apply prices** writes that supplier's prices onto the lines. Applying and switching supplier changes the order's supplier at the same time. Coverage shows how many lines have a price at that supplier.

**Add line** picks a part from the inventory. The project action **Shortages to purchase** and the inventory action **All below threshold to purchase** land here as well.

The purchase list sorts by last saved, or with the newest created order first, until you click a column. The choice is **Settings → Display → Project and purchase order**.

## Projects

A project is one build of a product: the BOM, the stock it will take, the photos, the test, and the paperwork.

**New project** starts an empty project named as an untitled build. The sheet has four tabs, and a guide above them that names the next step.

1. **BOM.** Lines, matching, shortages, serials.
2. **Build.** Pick list and execute.
3. **Test.** Photos and the protocol.
4. **Report.** The PDF and the export sheet.

The status describes where the project sits:

| Status | Meaning |
| --- | --- |
| Planned | The BOM is being prepared |
| Started | Assembly is underway |
| Waiting | Parts are missing |
| Done | The build has been run and the test is not finished |
| Completed | The test is approved and the cost is locked |
| Archived | Kept, and left off the to-do list |

**Build count** is how many of the product this project produces. It multiplies every line when stock is reserved and deducted. A serial-tracked row stays a single piece: one module, one serial.

The sheet shows line count, build cost, shortages, unmatched lines, and missing serials. **Stock covers N builds** tells you how many complete builds the current stock can support after other open projects have reserved their share.

A completed project is sealed. **Reopen build** lets you change the BOM again. The test has to be finished once more, and that date is written on the PDF. The locked cost is cleared until you complete it again.

A project can be deleted while it is still only a plan. Once a build has been started, undo the build first. A completed build is reopened, then undone, then deleted.

The project list uses the same sort as the purchase list.

## BOM import

On the project, **Import BOM** reads a CSV, TSV, TXT, or Excel file from EasyEDA, KiCad, or Altium.

For each row the importer looks for a designator, an MPN, a comment or value, a footprint, and a quantity. Supplier and LCSC part numbers are used as the MPN when the manufacturer part is empty. A value that looks like a part number, rather than a passive value such as `10k`, is promoted to the MPN.

Rows marked do-not-populate are skipped. That is a fitted flag of no, or a DNP / DNI / do-not-install mark. A quantity of zero or less becomes one. Header rows are skipped.

Each imported line is matched against the inventory immediately. See [Matching](#matching).

## Matching

A BOM line is matched in this order:

1. The MPN, compared with the component MPN and with alternate MPNs.
2. The comment, when it looks like an MPN.
3. A designator token, compared with the schematic designator and the internal reference on a component. `R12:A` can match a component stored as `R12`.

A matched line shows the inventory part, the stock on hand, and either **Matched** or **Shortage**. Shortage means the stock left after earlier lines in the same project is below what this line needs.

An unmatched line has no inventory part yet. The guide counts them and offers **Re-match**.

**Pick part** chooses the inventory component by hand. That choice is kept. **Re-match** runs the automatic rules again and leaves hand-picked lines alone. **Clear match** drops the link so the line can be matched again.

**Fill Designator from Schema** and **Fill comment from name** copy those fields onto the lines that are empty.

The build cannot run while any line is unmatched or short.

## Adding and editing lines

**Add line** picks a part that is already in the inventory, such as a battery, a display, or an enclosure, and appends it to this BOM. You set the quantity on the line.

**Remove line** deletes the selected line after a confirm.

Edit the designator, comment, quantity, and serial on the line in the grid. **Track serial** comes from the component. A serial-tracked part needs a serial on every module before the project can be finished. The build itself can be run before those serials exist.

## Ready to build

A project is ready to build when all of this is true:

- The BOM has lines.
- Every line is matched to an inventory part.
- No line is short of stock for the build count.
- The build has not been run yet.

The guide then reads that stock covers the build, and its button is **Run build**. The same project appears on **To do** as **Ready to build**.

If something is short, the guide offers **Buy shortages**, which creates or extends the purchase orders described in [From a component to a purchase](#from-a-component-to-a-purchase).

## The build

**Pick list** shows the matched parts sorted by row, shelf, and box. The quantity is what you take for this build count. **Deduct from stock** on that list is the same action as **Execute build**.

**Execute build** asks you to confirm, then:

- Deducts each line's demand from stock.
- Writes a stock-log row per part, with the project name, the designator, and the serial when there is one.
- Sets the project to **Done** when it was planned, started, or waiting.
- Records the time construction finished.

It refuses to run while any line is unmatched or short. You can undo it. **Undo build** puts the deducted stock back and clears the build so the BOM can be changed. A completed project is reopened before it can be undone.

## Build photos

After the build has been run, and after any missing serials are filled in, the guide asks for at least one build photo. **Build photos** adds images from the build. Files that are too large or are not images are skipped. A photo can be deleted.

Build photos and test photos are stored separately. The build report prints them in those two groups. At least one of each is required before the project can be completed.

## Test protocol

The protocol is the ordered list of checks every project is tested against. Edit it once, under **Settings → Lists → Test protocol**.

Each step has a name, an instruction, and a flag for whether it is required before the project can be finished. **Up** and **Down** set the order. A new database starts with these steps, all required:

1. Visual inspection. Damaged parts, missing components, solder bridges, placement.
2. Polarity and assembly. Polarized parts and the drawing.
3. Continuity / no shorts. No short between supply and ground.
4. Power-up. Current and voltages within limits.
5. Functional test. The function the product is built to perform.
6. Firmware programmed (if applicable). Leave it unticked when the product has nothing to program, or clear **Required** on that step if it should not block completion.
7. Quality approved. The unit meets the bar.

Changing the protocol later does not rewrite steps already stored in an existing database. Add, rename, and reorder them in the list.

## Running the test

**Testing** opens the protocol for this project. For each step you tick that it was done and you can write a comment. The comment belongs to this project. The instruction belongs to the protocol.

The form also asks for:

- Built by
- Tested and approved by
- A protocol note
- At least one test photo

**Mark completed** checks the gaps. Required steps must be ticked. Built by, approved by, the protocol note, one build photo, and one test photo must be present. Serial-tracked lines must have their serials. The button tells you what is still missing.

When the requirements are met, the project becomes **Completed**. The build cost is locked at the prices in use then, so a later price update does not rewrite the cost of a finished build. The guide moves to the report.

## Export sheet

**Export sheet** is available after the build has been run. It is a support sheet for customs and shipping, not a declaration.

Each row is a component in the built product, with origin, HS, TARIC, HTSUS, and ECCN. The quantity follows the build. The sheet states how many products were built and the day construction finished. The finished product's own commodity code is filled in by the sender.

The sheet counts rows that are missing origin, HS, or TARIC, and rows whose ECCN is something other than EAR99 or NLR. Those codes are whatever you stored on the component, or whatever a lookup filled into an empty field.

## Build report

**Build report** writes a PDF after the project is completed and the cost is locked. A progress window stays up while the file is written.

The report carries the project, when construction started and finished, the component count, the locked cost, the test protocol with the ticks and comments, the names of who built and who approved, the build photos, and the test photos.

Reopening the build and finishing the test again writes the new test date into the next PDF.

You can also export the project BOM itself as Excel or PDF from the project, separate from the build report and the export sheet.

## Scrap

**Register scrap** deducts a quantity from stock immediately and stores a reason, for example a failed solder joint or a defective part. The part can be one on this BOM, or any other part in the inventory. The quantity cannot exceed what is on hand.

The project lists the scrap registered on it. **System → Scrap log** lists scrap across projects, including how often it happens and the most common reason. A scrap movement also appears in **Stock history** and in **System → Stock log**.

## Settings

Save on the settings page writes the section you are in.

**Language.** Twelve languages: English, Swedish, German, French, Spanish, Polish, Danish, Norwegian Bokmål, Finnish, Hindi, Simplified Chinese, and Japanese. The choice is kept for the next start. A new language is a JSON file in the `Locales` folder beside the program. Restart to load it.

**Display.** Nine themes: Dark, Light, Ember, Teal, Circuit, Classified, Sage, Coral, and Forest. Separate typeface and size for menus and headings, and for data in the lists. **Project and purchase order** chooses last saved, or chronological with the newest created row first, until you sort by a column. **Reset theme and fonts** restores the defaults.

**Economy.** Display currency: SEK, NOK, DKK, EUR, CHF, GBP, PLN, USD, CNY, JPY, or INR. A stored price keeps the currency it was entered in. Lists, cards, stash value, and project cost convert to the display currency. Rates come from the European Central Bank through Frankfurter. Amounts already in the display currency still show when a rate has not been fetched yet. VAT is a display option. The rate is yours. Stored prices stay exclusive of VAT.

**Lists.** Types, classes, locations, and the test protocol.

**Logo.** A PNG, JPG, or WebP shown at the bottom of the sidebar. Empty uses the ToberStash mark.

**Stock.** The global low-stock threshold. A part at or below it is marked low, unless that part has its own reorder point.

**API.** The keys described under [MPN lookup](#mpn-lookup), and the preferred source.

## Database, backup, and moving the list

**System → Database** shows the connection in use.

- **SQLite** is the local file created on the first start.
- **MySQL / MariaDB** and **SQL Server** need a host, port, database name, and user. **Encrypt connection** turns on SSL. **Test connection** checks it. **Use this connection** switches the program over and creates the tables if they are missing.

Export a JSON backup on the old database first, switch, then import that backup if the new database is empty. **Save SQL setup script** writes a script you can run in a tool such as phpMyAdmin. The program does not need that file in order to start. It creates the tables itself.

If the configured server cannot be opened at startup, ToberStash says so and opens the ordinary local SQLite file for that session. That is not Work offline, and it does not copy the server. Your server data is left where it is. Fix the connection and use it again.

While a server session drops, the program tries to reconnect on its own. It does not switch to Work offline by itself.

**Work offline** is for a server database on another machine. The strip at the bottom of the sidebar shows the mode: Online, Offline, or On this computer. The button sits in that strip.

Tick **This database runs on my computer** when MySQL, MariaDB, or SQL Server runs on this PC. Work offline is then switched off, because a second copy is not useful. SQLite is already on this computer, so the button stays off for it too.

**Work offline** copies the whole server database into a separate SQLite file on this PC. You keep working on that copy. The server is left as it is. **Work online** copies the file back and replaces the server database. Changes made on the server while you were offline are replaced. If the copy fails, you stay in the mode you were already in, and you can work again only when the message says the copy has finished.

Photos, datasheets, and purchase files are what can make a copy too large to hold in memory. A component list on its own, even a large one, is fine. Around 150 MB of those files you get a warning that also shows how many components, projects, and purchase orders you have. Around 1 GB the copy is refused.

**Several people.** Tick **Several people use this database** under System → Database. Each PC sets its own name. The name stays on that PC and is written into the program log and the stock log.

Opening a component, a purchase, a supplier, or a list locks that row until the window closes. Another person sees who has it, and Edit stays off. A project build is separate: **Lock to me** stops anyone else from starting, undoing, or scrapping that build. **Release** gives it back. The project itself can still be opened. **System → Locks** lists locks that stayed behind, and can remove one.

**Work offline** with several people copies the server for reading only. Add, edit, and delete stay off. **Work online** deletes that copy and opens the server again. The server is not replaced. With a single user, Work offline still copies both ways, and going online replaces the server.

The scheduled backup follows whichever database is open.

**System → Program log** records edits, imports, purchases, merges, and price updates. **System → Stock log** records quantity movements. **System → About** shows the version, a short disclaimer, and the third-party notices.

Uninstall is deleting the extracted program folder and `%LocalAppData%\ToberStash`. Deleting the AppData folder removes the local database, the keys, the photos, and the backups that were stored there.
