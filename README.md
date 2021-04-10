# Quick Start for API setup

- [Requirements](#requirements)
- [Making your data public](#making-your-data-public)
  - [1. Setup your Arduino](#1-setup-your-arduino)
    - [🛠 Initial Arduino Setup 🛠](#-initial-arduino-setup-)
    - [▶️ Each time thereafter ▶️](#️-each-time-thereafter-️)
  - [2. Start your server and API service (`arduino-api-server`)](#2-start-your-server-and-api-service-arduino-api-server)
    - [🛠 Initial `arduino-api-server` Setup 🛠](#-initial-arduino-api-server-setup-)
    - [▶️ Each time thereafter ▶️](#️-each-time-thereafter-️-1)
  - [3. Start your serial message to API request converter (`arduino-serial-fetch`)](#3-start-your-serial-message-to-api-request-converter-arduino-serial-fetch)
    - [🛠 Initial `arduino-serial-fetch` Setup 🛠](#-initial-arduino-serial-fetch-setup-)
    - [▶️ Each time thereafter ▶️](#️-each-time-thereafter-️-2)
  - [Template for sharing info about your API](#template-for-sharing-info-about-your-api)
- [Retrieving someone else's data](#retrieving-someone-elses-data)

## Requirements

- Access to terminal (Mac OS) or command line (Windows)
- Ability to view hidden files (Mac OS: `Command + Shift + .` in Finder)
- [Node.js](https://nodejs.org/en/download/)
- [Arduino](https://nodejs.org/en/download/) and [Processing](https://processing.org/download/) IDEs

## Making your data public

Three things are required for this to happen:

1. [Setup your arduino](#1-setup-your-arduino)
2. [Start your server and API service (`arduino-api-server`)](#2-start-your-server-and-api-service-arduino-api-server)
3. [Start your serial message to API request converter (`arduino-serial-fetch`)](#3-start-your-serial-message-to-api-request-converter-arduino-serial-fetch)

---

### 1. Setup your Arduino

**🤔 Why? 🤔** Your Arduino must have a program on it that corresponds with the parser in `arduino-serial-fetch`, which expects to receive serial messages with the data all being printing on a single line, such as:

```js
// for three pins
<number>\t<number>\t<number>\t\n
```

Check the [api-serial-fetch](https://github.com/soundasobject21/arduino-serial-fetch#arduino-code-and-serialprint-format) documentation for detailed information on how to write a program that does this, or for some boilerplate code.

#### 🛠 Initial Arduino Setup 🛠

1. Upload the correct code to your Arduino (see https://github.com/soundasobject21/arduino-serial-fetch#arduino-code-and-serialprint-format)
2. Make a note of the port name you use for your Arduino (i.e. `/dev/cu.SLAB_USBtoUART`)
3. Open the Arduino IDE Serial Monitor to make sure the program is working as expected
4. **Close the Arduino IDE Serial Monitor** to free up the serial port
5. Keep the arduino plugged in

#### ▶️ Each time thereafter ▶️

Once you have done the initial setup, and you haven't changed the code or circuit on your arduino since, all you need to do next time is:

1. Plug in the arduino

---

### 2. Start your server and API service (`arduino-api-server`)

**🤔 Why? 🤔** Before you can make your data publicly available, you need to first set up a server to store that data and provide an API service that will allow you and others to update and retrieve that data.

#### 🛠 Initial `arduino-api-server` Setup 🛠

1. Download the repo and unzip the file: [⬇️ arduino-api-server main.zip](https://github.com/soundasobject21/arduino-api-server/archive/refs/heads/main.zip)
   > For detailed instructions on this repository, please read the [`arduino-api-server` documentation](https://github.com/soundasobject21/arduino-api-server)
2. Open a new terminal/command line window and make sure you are in the directory you just unzipped. To do this, you can use the `cd` (change directory) command, followed by a space, and then drag and drop the folder into terminal. It should write out the full path for you. Example:

   ```bash
   cd /Users/me/My Stuff/arduino-api-server
   ```

3. Install dependencies

   ```bash
   npm install
   ```

4. Create your database

   ```bash
   # Mac OS
   cp db.json.dist db.json
   # Windows
   copy db.json.dist db.json
   ```

   > Or you can do this manually and duplicate `db.json.dist` as `db.json`

5. Create your local `.env` file (this file stores variables that node.js can use)

   ```bash
   # Mac OS
   cp .env.dist .env
   # Windows
   copy .env.dist .env
   ```

   > Or you can do this manually and duplicate `.env.dist` as `.env`

6. Edit the newly created `.env` file to specify your desired subdomain (i.e. `<your-subdomain>.loca.lt`). Note: files starting with `.` are hidden by default. You may need to turn on hidden files in your OS. For Mac users, the shortcut `Command + Shift + .` will toggle hidden files.

   ```bash
   # all lowercase, no spaces
   SUBDOMAIN=<your-subdomain>
   ```

   **Important:** your subdomain should be fairly unique (ensures sure it's available). It should only contain letters, numbers, and hyphens. **No spaces or capital letters.**

   Example:

   ```bash
   SUBDOMAIN=stephanie-sao2021
   ```

7. Start the server and make it publicly available:

   ```bash
   npm run start:tunnel
   ```

   You should see output that looks like:

   ```
   🔄 serving at http://localhost:3000
   🏗  creating tunnel
   ✅ your public API_HOST is https://<your-subdomain>.loca.lt
   ```

8. 🎉 Your server is ready to use and is publicly available at the URL that ends in `.loca.lt`! Your publicly available API_HOST is confirmed in the console. **The public API_HOST is what you should share with others that want to access your data.**

   ```
   https://<your-subdomain>.loca.lt
   ```

9. To stop the server, use ctrl-c (`^C`) in the terminal/command line window where it is running.

#### ▶️ Each time thereafter ▶️

Once you've done the initial setup for `arduino-api-server`, all you need to do to start the server again is:

1.  Open a new terminal/command line window and make sure you're in the `arduino-api-server` folder:

    ```bash
    cd /Users/me/My Stuff/arduino-api-server
    ```

2.  Start the server and make it publicly available:

    ```bash
    npm run start:tunnel
    ```

3.  To stop the server, use ctrl-c (`^C`) in the terminal/command line window where it is running.

---

### 3. Start your serial message to API request converter (`arduino-serial-fetch`)

**🤔 Why? 🤔** In the previous step, you set up a server that can store data, so now we need a way to convert our serial messages to API requests which will then update the data on the server.

#### 🛠 Initial `arduino-serial-fetch` Setup 🛠

1. Download the repo and unzip the file: [⬇️ arduino-serial-fetch main.zip](https://github.com/soundasobject21/arduino-serial-fetch)
   > For detailed information on this repository, please read the [`arduino-serial-fetch` documentation](https://github.com/soundasobject21/arduino-serial-fetch)
2. Open a new terminal/command line window and make sure you are in the directory you just unzipped. To do this, you can use the `cd` (change directory) command, followed by a space, and then drag and drop the folder into terminal. It should write out the full path for you. Example:

   ```bash
   cd /Users/me/My Stuff/arduino-serial-fetch
   ```

3. Install dependencies

   ```bash
   npm install
   ```

4. Make sure your Arduino is plugged in and you've completed the required [Arduino setup](#1-setup-your-arduino)
5. Create your local `.env` file (this file stores variables that node.js can use)

   ```bash
   # Mac OS
   cp .env.dist .env
   # Windows
   copy .env.dist .env
   ```

   > Or you can do this manually and duplicate `.env.dist` as `.env`

6. Edit the newly created `.env` file to match your arduino configuration. At minimum, all you will need to change is the `SERIALPORT`. This should match the port name you took note of in the [Arduino setup](#1-setup-your-arduino)

   ```bash
   # SERIALPORT=<your-arduino-serial-port>
   SERIALPORT=/dev/tty.SLAB_USBtoUART
   ```

   You may also want change the `INTERVAL`. This value (in milliseconds) is how rapidly the data will update.

   ```bash
   # Will update data every 200 ms
   INTERVAL=200
   ```

7. Update `main.js` if your circuit and Arduino program are configured for a different set of pins. See [the documentation on adding pins](https://github.com/soundasobject21/arduino-serial-fetch#adding-more-pins) for more information.
8. Run the `arduino-serial-fetch` app
   ```bash
   npm start
   ```
9. To stop the app, use ctrl-c (`^C`) in the terminal/command line window where it is running.

#### ▶️ Each time thereafter ▶️

Once you've done the initial setup for `arduino-serial-fetch`, all you need to do to start the app again is:

1. Make sure your Arduino is plugged in (see [Arduino setup](#️-each-time-thereafter-))

2. Open a new terminal/command line window and make sure you're in the `arduino-serial-fetch` folder:

   ```bash
   cd /Users/me/My Stuff/arduino-serial-fetch
   ```

3. Start the app:

   ```bash
   npm run start
   ```

4. To stop the app, use ctrl-c (`^C`) in the terminal/command line window where it is running.

### Template for sharing info about your API

I've created a template that you can use to document information about your API. This will be useful to share with anyone that wants to access your data:

[⬇️ API Spec Template](https://docs.google.com/spreadsheets/d/1aokPioja67O47MOZi_Haebkh421ErVFZ602GePMZQQU/edit?usp=sharing)

Documenting your API like this will help those that want to use your data, as it clearly states:

- Your API_HOST
- Which pins are being used
- What types of inputs are connected to those pins (knob, sensor, button?)
- What the min and max ranges are of the pins. This is important so users will know how to map/scale your data accurately.

## Retrieving someone else's data

> If all you want to do is retrieve someone else's data, then you can completely ignore all of the steps in the [Making your data public](#making-your-data-public) section. You just need to find a classmate that has an API ready and running when you are.

To retrieve someone else's data, you will need:

1. Their public `API_HOST` (should end in `.loca.lt`). This information can be found when the other party [starts their server](#️-each-time-thereafter--1)
2. A program that will send API requests to retrieve data. A Processing template is available in our processing repo: [https://github.com/soundasobject21/processing/tree/main/ReadingFromAPI/template](https://github.com/soundasobject21/processing/tree/main/ReadingFromAPI/template). (You will need to download all three files and store them in a folder named `template`). Then open `template.pde` in Processing.

   If you are using the provided Processing template Be sure to update the `api_host` variable with the `API_HOST` that is provided to you by the other party.

   All pin data is retrieved from the other party's server and stored in a `pins[]` array that can be used in the `draw()` loop.

3. To use the other party's data effectively, you will also want [more information about the data they are sending](#template-for-sharing-info-about-your-api).
