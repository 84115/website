---
layout: post
title: "Creating Openable PWA Notifications"
date: 2023-09-13 12:00
comments: true
categories: pwa android ios
---

Service Worker
```js
// ...

self.addEventListener('notificationclick', function (event) {
    let data;

    if (event.notification) {
        data = event.notification.data;
    }

    event.notification.close();

    // Display the current notification if it is already open, and then put focus on it.
    event.waitUntil(clients.matchAll({
        type: 'window',
    }).then(function (clientList) {
        for (var i = 0; i < clientList.length; i++) {
            let client = clientList[i];

            if ('navigate' in client && 'url' in data && data.url != '') {
                return client.navigate(data.url);
            }
        }

        if (clients.openWindow && 'url' in data && data.url != '') {
            return clients.openWindow(data.url);
        }
    }));
});


// ...
```


app.js
```js
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/service-worker.js')
        .then(function(registration) {
            console.log('Registration successful, scope is:', registration.scope);
        })
        .catch(function(error) {
            console.log('Service worker registration failed, error:', error);
        });

    navigator.serviceWorker.addEventListener('message', (event) => {
        if (!event.data.action) {
            return
        }

        switch (event.data.action) {
            case 'redirect-from-notificationclick':
                window.location.href = event.data.url
            break
        }
    })
}
```

ProgressiveWebAppNotification.php
```php
<?php

namespace App\Notifications;

use Illuminate\Bus\Queueable;
use Illuminate\Notifications\Notification;
use Illuminate\Contracts\Queue\ShouldQueue;
use NotificationChannels\WebPush\WebPushMessage;
use NotificationChannels\WebPush\WebPushChannel;

class ProgressiveWebAppNotification extends Notification implements ShouldQueue
{
    use Queueable;

    /**
     * Create a new notification instance.
     *
     * @return void
     */
    public function __construct(
        public $message = "",
        public $body = "",
        public $image = "",
        public $url = ""
    ) {
        $this->message = $message;
        $this->body = $body;
        $this->image = $image;
        $this->url = $url;
    }

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return [
            WebPushChannel::class,
        ];
    }

    public function toWebPush($notifiable, $notification)
    {
        return (new WebPushMessage)
            ->title($this->message)
            ->image($this->image ?? null)
            ->vibrate([100, 30, 100, 30, 100, 30, 200, 30, 200, 30, 200, 30, 100, 30, 100, 30, 100])
            ->icon('/pwa/square.png')
            ->badge('/pwa/badge.png')
            ->body($this->body ?? "Default Channel")
            ->data([
                'url' => $this->url ?? '',
            ]);
    }
}
```