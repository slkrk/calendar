# Aeon

[![Minimum PHP Version](https://img.shields.io/badge/php-%3E%3D%207.4-8892BF.svg)](https://php.net/)
[![License](https://poser.pugx.org/aeon-php/calendar/license)](//packagist.org/packages/aeon-php/calendar)
![Tests](https://github.com/aeon-php/calendar/workflows/Tests/badge.svg?branch=master)

Time Management Framework for PHP

> The word aeon /ˈiːɒn/, also spelled eon (in American English), originally meant "life", "vital force" or "being", 
> "generation" or "a period of time", though it tended to be translated as "age" in the sense of "ages", "forever", 
> "timeless" or "for eternity".

[Source: Wikipedia](https://en.wikipedia.org/wiki/Aeon) 

This library is fully immutable wrapper for date/time objects provided by PHP. 
The goal is to simplify working with date and time through object oriented API. 
It works with precision up to microseconds if needed. 

## Examples

#### Creating calendar

```php
use Aeon\Calendar\Gregorian\GregorianCalendar;

$calendar = GregorianCalendar::UTC();
```

#### Getting current time 

```php
use Aeon\Calendar\Gregorian\GregorianCalendar;

$dateTime = GregorianCalendar::UTC()->now();
```

#### Iterating over time 

```php
use Aeon\Calendar\Gregorian\GregorianCalendar;
use Aeon\Calendar\Gregorian\TimeInterval;
use Aeon\Calendar\TimeUnit;

$now = GregorianCalendar::UTC()->now();

$now->to($now->add(TimeUnit::days(7)))
    ->iterate(TimeUnit::day())
    ->each(function(TimeInterval $interval) {
        echo $interval->startDateTime()->day()->format('Y-m-d H:i:s.uO') . "\n";
    });
```

#### Measuring difference between two points in time

```php
use Aeon\Calendar\Gregorian\GregorianCalendar;

$start = GregorianCalendar::UTC()->now();
\usleep(250000);
$end = GregorianCalendar::UTC()->now();

echo $start->to($end)->distance()->inMilliseconds(); // int(250) +/-
```

#### Iterating over all days in year

```php
use Aeon\Calendar\Gregorian\GregorianCalendar;

$days = GregorianCalendar::UTC()
    ->currentYear()
    ->mapDays(fn(Day $day) => $day->dayOfYear())
```

#### Creating DateTime from string 

```php
use Aeon\Calendar\Gregorian\DateTime;

echo DateTime::fromString('2020-01-01 10:00:00')
    ->toISO8601(); // 2020-01-01T10:00:00+0000
```

#### Casting DateTime to different timezone string 

```php
use Aeon\Calendar\Gregorian\DateTime;
use Aeon\Calendar\Gregorian\TimeZone;

echo DateTime::fromString('2020-01-01 10:00:00')
    ->toTimeZone(TimeZone::australiaSydney())
    ->toISO8601(); // 2020-01-01T21:00:00+1100
```

## Extensions

### Holidays

[Github: Calendar Holidays](https://github.com/aeon-php/calendar-holidays) 

Check if specific day is a holiday.  

```php
$regionalHolidays = new GoogleCalendarRegionalHolidays(CountryCodes::PL);

echo $regionalHolidays->isHoliday(Day::fromString('2020-01-01'); // true 
```

### Process Sleep

[Github: Process](https://github.com/aeon-php/process)

Friendly sleep 

```php
SystemProcess::current()->sleep(TimeUnit::milliseconds(250));
```

### Doctrine Type

[Github: Doctrine Types](https://github.com/aeon-php/calendar-doctrine)

```yaml
# config/packages/doctrine.yaml

doctrine:
    dbal:
        types:
            aeon_date: Aeon\Calendar\Doctrine\Gregorian\DateType
            aeon_datetime: Aeon\Calendar\Doctrine\Gregorian\DateTimeType
            aeon_datetime_tz: Aeon\Calendar\Doctrine\Gregorian\DateTimeTzType
```
