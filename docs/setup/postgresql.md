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
# Postgresql Setup

Setting up a PostgreSQL server on your laptop is a straightforward process that can be accomplished in a few steps:

1. Download: Obtain the PostgreSQL installer for your operating system from the official website (https://www.postgresql.org/download/).

2. Install: Run the installer and follow the on-screen instructions. You can usually accept the default settings.

3. Configuration (optional): After installation, you might want to adjust some settings, like the default port, data directory, or password for the "postgres" user. These can be changed using the configuration files or tools provided by PostgreSQL.
> NOTE: You DO NOT need the stackbuilder tools.

4. Access: PostgreSQL comes with a command-line tool called psql that lets you interact with the database server. You can also use graphical tools like pgAdmin to manage the database.

5. Create a database: Once PostgreSQL is running, you can create a database using the createdb command or through pgAdmin.

6. Start using: You can now connect to your database using psql or pgAdmin and start creating tables, inserting data, and running queries.

Additional tips:

* If you're using macOS, you can install PostgreSQL using the Homebrew package manager (brew install postgresql).
* Consider using tools like Docker to easily create a PostgreSQL instance in a container.
* Refer to the official PostgreSQL documentation for detailed instructions and troubleshooting.


## Post Installation


### Setup Environment

Edit your profile (~/.zshrc or ~/.bashrc) and add the following lines:

1. Locate PostgreSQL Binaries: `/Library/PostgreSQL/16`
2. Add the path to your .zshrc or .bashrc file.

```shell
echo "
export PG_HOME=/Library/PostgreSQL/16
export PATH=$PG_HOME/bin:$PATH
" >> .zshrc
```

Once complete, run:
```shell
source ~/.zshrc
```

Test to make sure sql is on your path:
```shell
which psql
# should return something like:
# /Library/PostgreSQL/16/bin/psql
```

## Lastly, Install the Chinook Database
```shell
# /Library/PostgreSQL/16/bin/psql
psql -U postgres < schemas/chinook.sql
```