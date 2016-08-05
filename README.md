# Laravel Faulty - RESTful Exceptions

A Laravel/Lumen package that lets you handle the API problems with ease. All the thrown exceptions HTTP or non-HTTP are properly rendered as JSON.

Faulty provides a straightforward implementation of [IETF Problem Specification](https://tools.ietf.org/html/draft-nottingham-http-problem-07) and turns your exceptions to be returned in the below format with the content type of `application/problem+json`
 
```json
{
   "status": 403,
   "type": "http://example.com/problems/out-of-credit",
   "title": "You do not have enough credit.",
   "detail": "Your current balance is 30, but that costs 50.",
   "instance": "http://example.net/account/12345/logs?id=233"
}
```
Where 
- `type` is the absolute URI that identifies the type of problem
- `title` is the summary of problem
- `status` is the status code
- `detail` is human readable explanation specific to problem
- `instance` is the absolute URI that identifies the specific occurrence of the problem

> Faulty automatically turns your thrown exceptions to the specified response.  

## Installation

Run the below command
```
composer require kamranahmedse/laravel-faulty
```

Make your exception handler i.e. `App\Exceptions\Handler` that can be found at `app\Exceptions\Handler.php` extend from the Faulty's handler i.e.
 
 ```
 \KamranAhmed\Faulty\Handler
 ```
 
 And that's it. You are all set to use Faulty.
 
 ## Usage
 
 For HTTP exceptions to be rendered properly with the proper status codes, you should use the exception classes provided by faulty i.e. the ones available in `Faulty\Exceptions` namespace or use the relevant ones provided by the Symfony's HTTP component i.e. the ones available under `Symfony\Component\HttpKernel\Exception`
 
 #### Throwing Exceptions
 All the exception classes have the below signature
 ```php
\KamranAhmed\Faulty\Exceptions\[ProblemType]Exception($detail, $title = '', $instance = '', $type = '')
 ```
Here are some of the provided exception classes
 
```php
// Include the exception classes from the given namespace


throw new BadRequestException('Invalid request data');
throw new ConflictException('Same request is already pending');
throw new ForbiddenException('You are not allowed to perform this action');
throw new InternalErrorException('Exports directory isn\'t writable');
throw new NoContentException('Deletion request successfuly accepted');
throw new NotFoundException('Item not found');
throw new NotModifiedException('..');
throw new PaymentRequiredException('..');
throw new PreconditionFailedException('..');
throw new ProcessingException('..');
throw new RequestTimeoutException('..');
throw new RequestTooLongException('..');
throw new UnauthorizedException('..');
throw new UnprocessableEntityException('..');
```

Also, if you would like to return any response for which the exception class isn't available, you can use the `HttpException` class i.e.
 
```php
throw new \KamranAhmed\Faulty\Exceptions\HttpException($title = '', $status = 500, $detail = '', $instance = '', $type = '');
```
For example

```php
throw new \KamranAhmed\Faulty\Exceptions\HttpException('Unsupported Media Type', 415);
```

#### Syntactic Sugar
Also, for any of the exception classes above, you can use the below syntax as well.
 
```php
$typeUrl = route('api.problem', ['type' => 'forbidden']);
$occurence = route('account.error', ['account_id' => 'A837332A', 'log_id' => 34]);

(new ForbiddenException("Your account doesn't have the balance of 50 USD"))
    ->setTitle('Balance too low)
    ->setType($problemRoute)
    ->setInstance($occurence)
    ->toss();
```


## Contributing
Feel free to fork, enhance, create PR and lock issues.

## License
MIT © [Kamran Ahmed](http://kamranahmed.info)
