# Principles of creating libraries

1. Do not pollute the global scope.
2. Make sure you don't block the webpage trying to load your library, hence make the injection of scripts async.
3. Dont use dependencies(jQuery, Lodash). Bundle them into your JS files and make sure to use noConflict. Webpack/Browserify can help your dependencies from the original site with scoping.
4. Using ```Access-Control-Allow-Origin: *``` (enabling CORS) to enable your library to communicate with a server wherever it is loaded.
5. Identifying users using first-party cookies - These cookies are placed in the same domain where your JS code runs.
6. Identifying users using third-party cookies - Cookies placed on a different domain.
7. Identifying users using localStorage - But the same origin policy applies to localStorage, so visiting the same site using HTTP and HTTPS will result in different localStorage contents.
8. Distributing
    - Always uglify/minify your JS/CSS resources. Using CDNs(paid).
    - Caching:
        - If you set ```max-age``` to a big number, pushing out new version of your library will take time to propagate to clients.
        - If you set ```max-age``` to a small number, your client will download your library frequently.
        - Split your library to 2 files - loader.js & application.js. Your loader will have the ```version number``` and set its ```max-age``` to 1 hour and let it download frequently (less than a kb file). The loader will download application.js if the version changes. Set ```max-age``` of application.js to infinity.
