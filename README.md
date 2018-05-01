## 1. INSTALL MongoDB

Hey hey from learnyoumongo. First, let's get MongoDB installed.
You can download MongoDB from https://www.mongodb.org/downloads.

We will also need to add it to your $PATH.

### HINTS

To verify that `mongod` is installed, you can try running `mongod --version`.

If you are on Windows, you will need to use `mongod.exe` instead.

It should print out something similar to:

    db version v2.6.8
    2015-05-06T09:44:39.362-0500 git version: nogitversion

## 2. CONNECT

Start mongod on port 27017 with data as the dbpath

-------------------------------------------------------------------------------

### HINTS

You may have to create the data directory.

    mkdir data

To start mongo on port 27017, run `mongod --port 27017 --dbpath=./data`.

Then, in another terminal, run `npm install mongodb`.

Then, run `learnyoumongo verify`.

If this lesson is passed, be sure to leave mongod running as it will
be used for the remainder of the exercise.
