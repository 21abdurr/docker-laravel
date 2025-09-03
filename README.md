tambahkan ini pada vite.config.js

    server: {
        host: "0.0.0.0", // penting: biar bisa diakses dari host
        port: 5173,
        strictPort: true,
        origin: "http://localhost:5173", // kasih tahu laravel-vite-plugin URL dev server
        hmr: {
            host: "localhost",
            port: 5173,
        },
        cors: true,
    }
