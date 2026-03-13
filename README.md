# nuxeo-node-importer

Command line tool to import a local folder to a Nuxeo Platform instance.

## Prerequisites

- Node.js **v16** or higher
- npm (comes with Node.js)

---

## Node 18+ Compatibility Fix

If you encounter this error when running with Node 18+:

```
RequestInit: duplex option is required when sending a body.
```

This is caused by Node's native `fetch` API conflicting with the older `isomorphic-fetch` library. To fix it, patch `node_modules/isomorphic-fetch/fetch-npm-node.js`:

**Change this:**
```javascript
if (!global.fetch) {
    global.fetch = module.exports;
    // ...
}
```

**To this:**
```javascript
// Always override global.fetch (fixes Node 18+ native fetch duplex issue)
global.fetch = module.exports;
global.Response = realFetch.Response;
global.Headers = realFetch.Headers;
global.Request = realFetch.Request;
```

This forces the use of `node-fetch` instead of Node's native fetch.

> **Note:** This patch will be overwritten if you run `npm install`. Re-apply it after reinstalling dependencies.

---

## Installation

Since this module is not yet published on npm, you can install it locally:

```bash
# Clone the repository
$ git clone https://github.com/qualitified/nuxeo-node-importer.git
$ cd nuxeo-node-importer

# Install globally
$ npm install -g .
```
This will make the nuxeo-importer command available globally.


## Usage

    $ nuxeo-importer [options] local-path remote-path

Recursively import the files and directories of `local-path` to the `remote-path` on a Nuxeo Platform instance.

Default behavior:
- for each directory, it creates a document of type `Folder`.
- for each file, it uses the `FileManager.Import` operation to create the corresponding document.


Options are:

- `-b --baseURL`: the base URL of the Nuxeo Platform instance. Default to `http://localhost:8080/nuxeo`.
- `-u, --username`: the username to use to connect to the server. Default to `Administrator`.
- `-p, --password`: the password to use to connect to the server. Default to `Administrator`.
- `-c, --chainId`: operation chain to use when creating files. Default to `FileManager.Import`.
- `-f, --folderishType`: document type to use when creating folders. Default to `Folder`.
- `-m, --maxConcurrentRequests`: Maximum number of concurrent requests. Default to 5.
- `-t, --timeout`: Timeout in ms. Default to 30000.
- `-v, --verbose`: verbose output, print configuration and more logs Default to `false`.
