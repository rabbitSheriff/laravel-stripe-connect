## Installation

Install via composer

```
composer require rap2hpoutre/laravel-stripe-connect
```

Add your stripe credentials in `.env`:

```
STRIPE_KEY=pk_test_XxxXXxXXX
STRIPE_SECRET=sk_test_XxxXXxXXX
```

## Usage

You can make a single payment from a user to another user
 or save a customer card for later use. Just remember to
 import the base class via:
 
```php
use Rap2hpoutre\LaravelStripeConnect\StripeConnect;
```

### Example #1: direct charge

The customer gives his credentials via Stripe Checkout and is charged.
It's a one shot process. `$customer` and `$vendor` must be `User` instances.

```
StripeConnect::transaction($token)
    ->amount(1000, 'usd')
    ->from($customer)
    ->to($vendor)
    ->create(); 
```

### Example #2: save a customer then charge later

Sometimes, you may want to register a card then charge later.
First, create the customer.

```php
StripeConnect::createCustomer($token, $customer);
```

Then, (later) charge the customer without token.

``` 
StripeConnect::transaction()
    ->amount(1000, 'usd')
    ->from($customer)
    ->to($vendor)
    ->create(); 
```

### Exemple #3: create a vendor account

You may want to create the vendor account before charging anybody.
Just call `createAccount` with a `User` instance.

``` 
StripeConnect::createAccount($vendor);
```

### Exemple #4: Charge with application fee

``` 
StripeConnect::transaction()
    ->amount(1000, 'usd')
    ->fee(50)
    ->from($customer)
    ->to($vendor)
    ->create(); 
```
