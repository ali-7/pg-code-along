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
