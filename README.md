## TS Bindings Examples Using Workspaces

# Setup

I've already add Soroban contract ID's to the .soroban folder. You can overwrite these using your own contract ID's generated from the docs

To get started, in the root folder:

Simply run `npm i`
Followed by `npm run dev`

`npm i` will will install npm deps and symlink the `bindings` dir so you can call it as a normal npm module from `web-app`. The `postinstall` cmd will build the TS bindings into the empty `bindings` folder.
