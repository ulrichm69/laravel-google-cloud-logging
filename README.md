# laravel-google-cloud-storage

A Google Cloud Storage filesystem for Laravel.

[![Latest Stable Version](https://poser.pugx.org/superchairon/laravel-google-cloud-logging/v/stable)](https://packagist.org/packages/superchairon/laravel-google-cloud-logging)
[![License](https://poser.pugx.org/superchairon/laravel-google-cloud-logging/license)](https://packagist.org/packages/superchairon/laravel-google-cloud-logging)
[![Total Downloads](https://poser.pugx.org/superchairon/laravel-google-cloud-logging/downloads)](https://packagist.org/packages/superchairon/laravel-google-cloud-logging)

This package is a driver for logging and error reporting for Google Cloud Platform Stackdriver.

## Installation

```bash
composer require debtia/laravel-google-cloud-logging
```

Add a new driver in your `logging.php` config

```php
        'stackdriver' => [
            'driver' => 'custom',
            'via' => \Debtia\LaravelGoogleCloudLogging\StackdriverDriver::class,
            'logName' => 'laravel.log',
            'labels' => [
                'application' => env('APP_NAME'),
                'environment' => env('APP_ENV'),
                'logchannelname' => 'stackdriver',
            ],
            'level' => 'debug',
        ]
```

In the Log Explorer, you will se this payload
```json
{
  insertId: "1fwd45fghmtpq",
  jsonPayload: {
    message: "Small fish is a healthy snack for sharks"
  },
  resource: {
    type: "global",
    labels: {
      project_id: "house-chimney-893111"
    }
  },
  timestamp: "2020-10-23T07:26:27.685740Z",
  severity: "DEBUG",
  labels: {
    application: "MaintainHouse",
    environment: "suburb",
    logchannelname: "stackdriver"
  },
  logName: "projects/house-chimney-893111/logs/laravel.log",
  receiveTimestamp: "2020-10-23T07:26:28.005469561Z"
}
```

### Authentication

The Google Client uses a few methods to determine how it should authenticate with the Google API.

If the `GOOGLE_CLOUD_PROJECT` and `GOOGLE_APPLICATION_CREDENTIALS` env vars are set, it will use that.
   ```php
   putenv('GOOGLE_CLOUD_PROJECT=project-id');
   putenv('GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json');
   ```

While running on **Google Cloud Platform** environments such as **Google Compute Engine**, **Google App Engine** and **Google Kubernetes Engine**, no extra work is needed. The Project ID and Credentials and are discovered automatically. Code should be written as if already authenticated.

For more information visit the [Authentication documentation for the Google Cloud Client Library for PHP](https://github.com/googleapis/google-cloud-php/blob/master/AUTHENTICATION.md) 

### Create a bucket [GOOGLE_CLOUD_PROJECT]
```bash
gsutil mb gs://[GOOGLE_CLOUD_PROJECT]
```

In Storage browser, click on the three dots to the right of [GOOGLE_CLOUD_PROJECT] and select Edit Bucket Permissions - Add member
Add cloud-logs@google.com as an owner to the bucket.
