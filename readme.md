# Laravel/React

This is the installation guide to integrade reactjs in laravel project

## Installation

### Setup Laravel

Create a new Laravel project (if you haven’t already):

```sh
composer create-project laravel/laravel my-project
```

Navigate to your Laravel project directory:

```sh
cd my-project
```

### Install Packages

Install Inertia.js server-side adapter

```sh
composer require inertiajs/inertia-laravel
```

Install Inertia.js client-side adapter and React

```sh
npm install @inertiajs/inertia @inertiajs/inertia-react @inertiajs/progress
```

Install Vite and necessary React dependencies

```sh
npm install react react-dom @vitejs/plugin-react
```

### Setup middleware

setup the Inertia middleware

```php
php artisan inertia:middleware
```

add this in your bootstrap/app.php

```php
use App\Http\Middleware\HandleInertiaRequests;

->withMiddleware(function (Middleware $middleware) {
    $middleware->web(append: [
        HandleInertiaRequests::class,
    ]);
})
```

### Setup Vite

Install Vite Laravel Plugin:

```sh
npm install --save-dev laravel-vite-plugin
```

Create or update vite.config.js

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import react from '@vitejs/plugin-react';

export default defineConfig({
    plugins: [
        laravel({
            input: 'resources/js/app.jsx',
            refresh: true,
        }),
        react(),
    ],
});

```

### Set Up Your React Application

Go to the `resources/js` change `app.js` to `app.jsx`

Paste this in `app.jsx`

```jsx
import { createInertiaApp } from "@inertiajs/react";
import { createRoot } from "react-dom/client";

createInertiaApp({
    resolve: (name) => {
        const pages = import.meta.glob("./Pages/**/*.jsx", { eager: true });
        return pages[`./Pages/${name}.jsx`];
    },
    setup({ el, App, props }) {
        createRoot(el).render(<App {...props} />);
    },
});
```

Create a sample Inertia page in `resources/js/Pages/Home.jsx`

```jsx
import React from 'react';

const Home = () => {
    return (
        <div>
            <h1>Welcome to Inertia.js with React in Laravel</h1>
        </div>
    );
};

export default Home;
```

Create a route to serve the Inertia page

Update `routes/web.php`

```php
use Inertia\Inertia;

Route::get('/', function () {
    return Inertia::render('Home');
});
```

Update your `welcome.blade.php` to `app.blade.php`

paste this all in `app.blade.php`

```html
<!DOCTYPE html>
<html lang="en">
<head>

    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laravel with Inertia.js and React</title>
    @viteReactRefresh
    @vite('resources/js/app.jsx')

</head>

<body>
    @inertia
</body>

</html>
```

### Run Your Application

Run the Laravel development server

```sh
php artisan serve
```

Run the Vite development server

```sh
npm run dev
```

Visit **http://localhost:8000** in your browser to see your Inertia.js application using React in Laravel.
