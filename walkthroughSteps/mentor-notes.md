# pg code along walkthrough notes

This workshop is intended to be delivered as a code-along. This file contains the mentor notes, and the `step-*` folders in the same directory as this file contain the correct code after the corresponding step. There is no `step-1` folder as step 1 is simply inspecting the files. The students should code in the root directory.

## Getting Started
```sh
git clone https://github.com/ali-7/pg-code-along.git
```

## Step 1 – Navigating the initial files
1. Open `server/controllers/index.js`.
    - Here we see the `/static` endpoint reads and serves data from file called `static.js`.

2. Open `server/controllers/static.js`
    - We see that it contains a data array with two superhero objects.

3. Run `npm run dev` in command line and navigate to `http://localhost:3000/static` in the browser.
    - Here we see our hardcoded, static, data from `static.js`.
    - Storing/loading dynamic data in files is bad, because file I/O is inefficient and should just be used for server load/config data.

## Step 2 – Setting up the database
1. Create a folder inside the root project: `database`.
    - In the folder `database`, we're going to setup the structure/schema of our database, and hardcoded data.

2. Create a new file: `database/build.sql`.
    1. Put a `BEGIN;` at the start of the file and a `COMMIT;` at the end of the file.
        - Clarify that the init queries should be written between these lines (so BEGIN and COMMIT wrap around them).
        - The code inside it is a transaction, so the multiple queries are run as one query/chunk. If an error occurs inside of here, it will rollback the previous commands, preventing messing up the database. Can think of it as SQL error handling

    2. Write `DROP TABLE IF EXISTS superheroes CASCADE;`.
        - This line drops our database each time this file is run.
        > ONLY RUN IT ONCE. This file should never be used in production other than for initialisation. You only want to use this to reset your test database (and can add mock data for it).

         To update your schema, you can create separate update scripts.
        - Cascade will delete tables with relations (that have a REFERENCE defined towards) to `superheroes` too.

    3. Write the schema:
        ```sql
        CREATE TABLE superheroes (
          id SERIAL PRIMARY KEY,
          name VARCHAR(100) NOT NULL,
          superPower TEXT NOT NULL,
          weight INTEGER DEFAULT 100
        );
        ```

        - All tables should have an integer `id` that is set as a `PRIMARY KEY` - this is used relate databased together (integer PRIMARY KEY helps with performance)
        - `PRIMARY KEY` also adds `UNIQUE` and `NOT NULL` (primary keys have to be unique).
        - `VARCHAR(LENGTH)`, `INTEGER`, `TEXT` (unlimited length, but larger data usage), etc are SQL data types.
        - `NOT NULL` tells the PostgreSQL to give an error if this is not set.
        - `DEFAULT 100` changes `NULL` values to be `100`.

    4. Initialise some mock/hardcoded data:
        ```sql
        INSERT INTO superheroes (name, superPower, weight) VALUES
          ('Wolverine', 'Regeneration', 300),
          ('Captain Marvel', 'Shoots concussive energy bursts from her hands', 165),
          ('Iron Man', 'None', 425);
        ```

        - Rows separated with commas and each bracket, `(comma-separated values inside here)`, has a row inside it with values

## Step 3 – Creating the database

We will be using an autocomplete client `pgcli`, [reference here](https://github.com/macintoshhelper/learn-sql/blob/master/postgresql/setup.md#bonus---installing-an-autocomplete-client)

We will be using these commands to create our database and user for it.

> Note that password is a string inside '' (NOT double quotes "")

```
CREATE DATABASE db_name;
CREATE USER user_name WITH SUPERUSER PASSWORD 'password';
ALTER DATABASE db_name OWNER TO user_name;
```

1. In your command line, run `pgcli`.

2. Create the database by typing `CREATE DATABASE film;` into your Postgres CLI client.

3. Create a user specifically for the database with a password by typing `CREATE USER [the new username] WITH SUPERUSER PASSWORD '[the password of the database]'`;

4. Change ownership of the database to the new user by typing `ALTER DATABASE db_name OWNER TO user_name;`.

5. Add a config.env file and add the database's url in this format: `DB_URL = postgres://[username]:[password]@localhost:5432/[database]`
    - Don't use semi-colons or apostrophes for strings in `config.env`, or use alternative JSON notation

6. Try connecting to the database by typing `pgcli postgres://[username]:[password]@localhost:5432/[database]`.

If you experience permission problems, try running `pgcli film` then `GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO [the new username];`

