# Laravel HTTP to Curl

[![Latest Version on Packagist](https://img.shields.io/packagist/v/jigarakatidus/laravel-http-to-curl.svg?style=flat-square)](https://packagist.org/packages/jigar-dhulla/laravel-http-to-curl)
[![GitHub Tests Action Status](https://img.shields.io/github/actions/workflow/status/jigar-dhulla/laravel-http-to-curl/php.yml?branch=main&label=tests&style=flat-square)](https://github.com/jigar-dhulla/laravel-http-to-curl/actions/workflows/php.yml)
[![Total Downloads](https://img.shields.io/packagist/dt/jigarakatidus/laravel-http-to-curl.svg?style=flat-square)](https://packagist.org/packages/jigarakatidus/laravel-http-to-curl)


This package has two use-cases:
1. Simple dump and die with curl command for local development for troubleshooting the http request.
2. Enable logging curl command for each request in the logs.

## Installation

You can pull in the package via composer:

``` bash
composer require jigarakatidus/laravel-http-to-curl
```

The package will automatically register itself

## Usage

### Basic GET Request

```php
Http::ddWithCurl()
    ->get('https://example.com/api/resource');
```

Outputs

```bash
curl -H 'User-Agent: GuzzleHttp/7' -X 'GET' 'https://example.com/api/resource'
```

### GET Request with Query Parameters

```php
Http::ddWithCurl()
    ->get('https://example.com/api/resource', [
        'param1' => 'value1',
        'param2' => 'value2',
    ]);
```

Outputs

```bash
curl -H 'User-Agent: GuzzleHttp/7' -X 'GET' 'https://example.com/api/resource?param1=value1&param2=value2'
```

### POST Request with JSON Payload

```php
Http::ddWithCurl()
    ->acceptJson()
    ->post('https://example.com/api/resource', [
        'key1' => 'value1',
        'key2' => 'value2',
    ]);
```

Outputs

```bash
curl -H 'User-Agent: GuzzleHttp/7' -H 'Accept: application/json' -H 'Content-Type: application/json' -X 'POST' 'https://example.com/api/resource' -d '{"key1":"value1","key2":"value2"}'
```

## Automatic Request Logging

To enable the automatic logging of curl command for each request, configure these options directly through environment variables:

- `HTTP_TO_CURL_LOGGING`: Enable/disable logging (defaults to false)
- `HTTP_TO_CURL_LOG_LEVEL`: Set log level (defaults to "debug")
- `HTTP_TO_CURL_LOG_CHANNEL`: Select log channel (defaults to "stack")

Optionally, you can publish the configuration file using the following command:
```bash
php artisan vendor:publish --tag=http-to-curl-config
```
This will create a `config/http-to-curl.php` file where you can customize the logging behavior.

### Logging Example

When you enable logging by setting `HTTP_TO_CURL_LOGGING=true` in your environment, all HTTP requests will be automatically logged. For example, if your application makes this request:

```php
Http::post('https://api.example.com/users', [
    'name' => 'John Doe',
    'email' => 'john@example.com'
]);
```

This cURL command will be automatically logged to your configured log channel:

```
[2025-05-13 23:53:46] local.DEBUG: curl -H 'User-Agent: GuzzleHttp/7' -H 'Content-Type: application/json' -X 'POST' 'https://api.example.com/users' -d '{"name":"John Doe","email":"john@example.com"}'
```

This is useful for debugging API calls in both development and production environments.


## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Pull Requests are welcome.

## Security

If you've found a bug regarding security please mail [jigar.tidus@gmail.com](mailto:jigar.tidus@gmail.com) instead of using the issue tracker.

## Credits

- [Jigar Dhulla](https://github.com/jigarakatidus)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
