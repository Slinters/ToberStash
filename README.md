# ToberStash

Local inventory and build manager for electronic components. It keeps the parts list, project BOMs, purchase orders, and the paperwork around a build on one Windows PC.

Copyright R.C. Tober AB. All rights reserved.

## What it does

- Inventory with stock, locations, photos, datasheets, and MPN lookup.
- Projects that turn a BOM into a build, a test protocol, and a PDF report.
- Purchase orders, receiving, and a comparison of supplier prices.
- SQLite on the machine, or MySQL, MariaDB, or SQL Server.
- English, Swedish, German, French, Spanish, Polish, Danish, Norwegian Bokmål, Finnish, Hindi, Simplified Chinese, and Japanese.
- Display currencies: SEK, NOK, DKK, EUR, CHF, GBP, PLN, USD, CNY, JPY, and INR. A stored price keeps its original currency. The lists convert to the currency chosen under **Settings → Economy**.

The current version is **0.9.0**. See [CHANGELOG.md](CHANGELOG.md).

## When the inventory outgrows this app

The component list can be moved to [Part-DB](https://github.com/Part-DB/Part-DB-server) or [InvenTree](https://inventree.readthedocs.io/). Under **System → Exchange**, export to either system. Part-DB files use semicolons. InvenTree files use commas. Prices are written as stored.

Projects, purchase orders, tests, and order documents stay in ToberStash. Import from the same menu brings a component list back.

## Install

ToberStash is a folder of files, not a setup program. It runs on 64-bit Windows.

1. Install the [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0) (x64). The SDK is only needed if you build from source.
2. Download the zip and extract the whole folder. Keep every file next to `ToberStash.exe`. The PDF reports need the native libraries that sit beside it.
3. Start `ToberStash.exe`.

Windows may say the app is unrecognized. Choose **More info**, then **Run anyway**.

The first start creates an empty inventory and a local database in `%LocalAppData%\ToberStash`. Nothing is written to the folder you extracted.

Then, in the app:

- **Settings → Language** picks the interface language. The choice is saved for the next start.
- **Settings → Economy** sets the display currency: SEK, NOK, DKK, EUR, CHF, GBP, PLN, USD, CNY, JPY, or INR. Each stored price keeps the currency it was entered in.
- **Settings → API** is optional. Part lookup works without keys for the public sources. Mouser, DigiKey, TME, and Nexar need your own keys. Those keys stay on this PC, encrypted for the Windows account.
- **System → Database** can move the inventory to MySQL, MariaDB, or SQL Server. Export a JSON backup first, switch, then import it.

To remove ToberStash, delete the extracted folder and `%LocalAppData%\ToberStash`.

## Requirements

- Windows 64-bit
- [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0) to run the published build
- [.NET 10 SDK](https://dotnet.microsoft.com/download) to build from source

## Build

From the repository root:

```powershell
dotnet build ToberStash/ToberStash.csproj
```

## Publish

The folder profile writes a 64-bit Release build and leaves native libraries next to the executable. QuestPDF needs those libraries beside the app.

```powershell
dotnet publish ToberStash/ToberStash.csproj -c Release -p:PublishProfile=FolderProfile
```

Output: `ToberStash/bin/Release/net10.0-windows/publish/win-x64/`

A 32-bit profile is available as `FolderProfile-x86`.

## Data

The first launch uses a local SQLite file. Under **System → Database** the connection can be switched to MySQL, MariaDB, or SQL Server. Export a JSON backup before switching, then import it into the new database.

API keys for part lookup are not stored in the inventory database. They stay on that PC, encrypted for the Windows account.

## Third-party software

Copyright lines and license terms are printed at the bottom of this file. The same text is shown in the app under **System → About**.

## Version

The version shown in **System → About** comes from `ToberStash/ToberStash.csproj`.

## Support

If ToberStash is useful, you can [buy the author a coffee](https://buymeacoffee.com/rctober).

[![Buy Me a Coffee](docs/buymeacoffee.png)](https://buymeacoffee.com/rctober)

## Third-party notices

```
ToberStash includes the third-party software listed below. The license texts are in English.

QuestPDF 2025.12.3
PDF documents.
Copyright Marcin Ziąbek and QuestPDF contributors.
QuestPDF Community MIT License. That grant applies when annual gross revenue is under 1,000,000 USD.
https://www.questpdf.com/

Six Labors ImageSharp 3.1.12
Image handling.
Copyright © Six Labors.
Six Labors Split License. For a for-profit organisation with annual gross revenue under 1,000,000 USD, this direct dependency is licensed under the Apache License 2.0.
https://github.com/SixLabors/ImageSharp

ClosedXML 0.105.0
Excel files.
MIT License. Copyright ClosedXML contributors.
https://github.com/ClosedXML/ClosedXML

DocumentFormat.OpenXml
Excel file format, used by ClosedXML.
MIT License. © Microsoft Corporation.

ExcelNumberFormat 1.1.0
Used by ClosedXML.
MIT License.

Dapper 2.1.66
Database access.
Apache License 2.0. Copyright 2019 Stack Exchange, Inc.
https://github.com/DapperLib/Dapper

Microsoft.Data.Sqlite 10.0.0
SQLite access.
MIT License. © Microsoft Corporation.

Microsoft.Data.SqlClient 6.1.1
SQL Server access.
MIT License. © Microsoft Corporation.

MySqlConnector 2.4.0
MySQL and MariaDB access.
MIT License. Copyright 2016–2024 Bradley Grainger.
https://mysqlconnector.net/

SQLitePCLRaw.bundle_e_sqlite3 3.0.0
SQLite native library.
Apache License 2.0. Copyright 2014–2025 SourceGear, LLC.
Includes SQLite, which is in the public domain.
https://sqlite.org/copyright.html

BouncyCastle.Cryptography
Used by Microsoft.Data.SqlClient.
MIT License. Copyright © Legion of the Bouncy Castle Inc.

.NET runtime
Shipped with this application.
MIT License. © Microsoft Corporation.
https://github.com/dotnet/runtime

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Apache License 2.0

Components marked Apache License 2.0 are licensed under the Apache License, Version 2.0.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
```
