self.addEventListener('install', event => {
  self.skipWaiting();
});

self.addEventListener('activate', event => {
  event.waitUntil(clients.claim());
});

self.addEventListener('fetch', event => {
  event.respondWith(
    caches.open('letter-builder-cache').then(cache => {
      return cache.match(event.request).then(response => {
        return response || fetch(event.request).then(networkResp => {
          cache.put(event.request, networkResp.clone());
          return networkResp;
        });
      });
    })
  );
});
