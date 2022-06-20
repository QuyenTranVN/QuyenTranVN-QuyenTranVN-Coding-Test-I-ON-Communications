# nuxt

## How to build

```bash
# Install dependent libraries
$ yarn install

# Get a full fake REST API 

$ json-server --watch db.json --port 3000

# Run the app with hot reload
$ yarn dev


```


## Directory explanation

### components/pages

Contains the components used in `pages`

### components/general

Contains components to share

Modal uses `Modal.vue` under components / general


### pages

This directory contains your application views and routes. Nuxt will read all the `*.vue` files inside this directory and setup Vue Router automatically.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/get-started/routing).

### plugins

The plugins directory contains JavaScript plugins that you want to run before instantiating the root Vue.js Application. This is the place to add Vue plugins and to inject functions or constants. Every time you need to use `Vue.use()`, you should create a file in `plugins/` and add its path to plugins in `nuxt.config.js`.

Currently, only the axios client (wrapper) used for API communication is stored.

## Others

### Design related

- We use vuetify for our UI framework

- scss is basically written in the component
  icon uses "MDI SVG", and those that do not use the one provided by Resco.

- The image is / assets / img /

- icon is / assets / icon /

### Utility

#### `yarn format`

- `~/components`
- `~/layouts`
- `~/pages`
- `~/plugins`

Formats the `.ts`,` .vue` files in the above directory
# QuyenTranVN-QuyenTranVN-Coding-Test-I-ON-Communications
# QuyenTranVN-QuyenTranVN-Coding-Test-I-ON-Communications
