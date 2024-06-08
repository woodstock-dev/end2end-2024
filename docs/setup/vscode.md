<!--
Copyright 2024 Ryan McGuinness

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
# Visual Studio Code Setup

**Setting Up Visual Studio Code (VSCode) for PostgreSQL on macOS**

This guide assumes you've already installed PostgreSQL on your Mac.

**1. Install Visual Studio Code:**

* Download and install VSCode from the official website: [https://code.visualstudio.com/](https://code.visualstudio.com/)
* Follow the installation wizard.

**2. Install the PostgreSQL Extension:**

* Open VSCode.
* Click the Extensions icon in the sidebar (or press `Shift+Cmd+X`).
* Search for "PostgreSQL" in the search bar.
* Look for the extension by Chris Kolkman and click "Install".

**3. Set Up a Connection:**

* Click on the PostgreSQL icon in the sidebar (it looks like an elephant).
* Click the plus (+) icon to add a new connection.
* Enter the following information:
    * **Hostname:** Typically `localhost`
    * **Port:** The port PostgreSQL is running on (usually `5432`)
    * **Username:** Your PostgreSQL username
    * **Password:** Your PostgreSQL password
    * **Database:** The name of the database you want to connect to (or choose "Show All Databases")
* Click "Save Connection" when finished.

**4. Start Querying:**

* Open a new SQL file (`Cmd+N`).
* Click the "Select PostgreSQL Server" button in the bottom-right corner of the status bar and choose your connection.
* Write your SQL queries in the file.
* Highlight the query and right-click to choose "Run Query" (or use the keyboard shortcut).

**Tips:**

* Explore other PostgreSQL extensions in the marketplace for additional features.
* Customize the extension's settings to your liking.
* Refer to the extension's documentation for detailed instructions and troubleshooting.

Let me know if you'd like help with any specific aspect of using PostgreSQL in VSCode! 
