[//]: # (AUTO-GENERATED BY "PHP README Helper": base file -> docs/base.md)

[![Build Status](https://github.com/voku/anti-xss/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/voku/anti-xss/actions)
[![codecov.io](http://codecov.io/github/voku/anti-xss/coverage.svg?branch=master)](http://codecov.io/github/voku/anti-xss?branch=master)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/8e3c9da417124971b8d8e0c1046c24c7)](https://www.codacy.com/app/voku/anti-xss)
[![Latest Stable Version](https://poser.pugx.org/voku/anti-xss/v/stable)](https://packagist.org/packages/voku/anti-xss) 
[![Total Downloads](https://poser.pugx.org/voku/anti-xss/downloads)](https://packagist.org/packages/voku/anti-xss)
[![License](https://poser.pugx.org/voku/anti-xss/license)](https://packagist.org/packages/voku/anti-xss)
[![Donate to this project using Paypal](https://img.shields.io/badge/paypal-donate-yellow.svg)](https://www.paypal.me/moelleken)
[![Donate to this project using Patreon](https://img.shields.io/badge/patreon-donate-yellow.svg)](https://www.patreon.com/voku)

# :secret: AntiXSS

"Cross-site scripting (XSS) is a type of computer security vulnerability typically found in Web applications. XSS enables 
attackers to inject client-side script into Web pages viewed by other users. A cross-site scripting vulnerability may be 
used by attackers to bypass access controls such as the same origin policy. Cross-site scripting carried out on websites 
accounted for roughly 84% of all security vulnerabilities documented by Symantec as of 2007." - http://en.wikipedia.org/wiki/Cross-site_scripting

### DEMO:
[http://anti-xss-demo.suckup.de/](http://anti-xss-demo.suckup.de/)

### NOTES:
1) Use [filter_input()](http://php.net/manual/de/function.filter-input.php) - don't use GLOBAL-Array (e.g. $_SESSION, $_GET, $_POST, $_SERVER) directly

2) Use [html-sanitizer](https://github.com/tgalopin/html-sanitizer) or [HTML Purifier](http://htmlpurifier.org/) if you need a more configurable solution

3) Add "Content Security Policy's" -> [Introduction to Content Security Policy](http://www.html5rocks.com/en/tutorials/security/content-security-policy/)

4) DO NOT WRITE YOUR OWN REGEX TO PARSE HTML!

5) READ THIS TEXT -> [XSS (Cross Site Scripting) Prevention Cheat Sheet](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md)

6) TEST THIS TOOL -> [Zed Attack Proxy (ZAP)](https://github.com/zaproxy/zaproxy)

### Install via "composer require"
```shell
composer require voku/anti-xss
```

### Usage:

```php

use voku\helper\AntiXSS;

require_once __DIR__ . '/vendor/autoload.php'; // example path

$antiXss = new AntiXSS();
```

Example 1: (HTML Character)

```php
$harm_string = "Hello, i try to <script>alert('Hack');</script> your site";
$harmless_string = $antiXss->xss_clean($harm_string);

// Hello, i try to alert&#40;'Hack'&#41;; your site
```

Example 2: (Hexadecimal HTML Character)

```php
$harm_string = "<IMG SRC=&#x6A&#x61&#x76&#x61&#x73&#x63&#x72&#x69&#x70&#x74&#x3A&#x61&#x6C&#x65&#x72&#x74&#x28&#x27&#x58&#x53&#x53&#x27&#x29>";
$harmless_string = $antiXss->xss_clean($harm_string);
    
// <IMG >
```
    
Example 3: (Unicode Hex Character)

```php
$harm_string = "<a href='&#x2000;javascript:alert(1)'>CLICK</a>";
$harmless_string = $antiXss->xss_clean($harm_string);
    
// <a >CLICK</a>
```

Example 4: (Unicode Character)

```php
$harm_string = "<a href=\"\u0001java\u0003script:alert(1)\">CLICK<a>";
$harmless_string = $antiXss->xss_clean($harm_string);
    
// <a >CLICK</a>
```

Example 5.1: (non Inline CSS)

```php
$harm_string = '<li style="list-style-image: url(javascript:alert(0))">';
$harmless_string = $antiXss->xss_clean($harm_string);

// <li >
```

Example 5.2: (with Inline CSS)

```php
$harm_string = '<li style="list-style-image: url(javascript:alert(0))">';
$antiXss->removeEvilAttributes(array('style')); // allow style-attributes
$harmless_string = $antiXss->xss_clean($harm_string);

// <li style="list-style-image: url(alert&#40;0&#41;)">
```

Example 6: (check if an string contains a XSS attack)

```php
$harm_string = "\x3cscript src=http://www.example.com/malicious-code.js\x3e\x3c/script\x3e";
$harmless_string = $antiXss->xss_clean($harm_string);

// 

$antiXss->isXssFound(); 

// true
```

Example 7: (allow e.g. iframes)

```php
$harm_string = "<iframe width="560" onclick="alert('xss')" height="315" src="https://www.youtube.com/embed/foobar?rel=0&controls=0&showinfo=0" frameborder="0" allowfullscreen></iframe>";

$antiXss->removeEvilHtmlTags(array('iframe'));

$harmless_string = $antiXss->xss_clean($harm_string);

// <iframe width="560"  height="315" src="https://www.youtube.com/embed/foobar?rel=0&controls=0&showinfo=0" frameborder="0" allowfullscreen></iframe>
```


### Unit Test:

1) [Composer](https://getcomposer.org) is a prerequisite for running the tests.

```
composer install
```

2) The tests can be executed by running this command from the root directory:

```bash
./vendor/bin/phpunit
```

## AntiXss methods

<p id="voku-php-readme-class-methods"></p><table><tr><td><a href="#adddonotclosehtmltagsstring-strings-this">addDoNotCloseHtmlTags</a>
</td><td><a href="#addevilattributesstring-strings-this">addEvilAttributes</a>
</td><td><a href="#addevilhtmltagsstring-strings-this">addEvilHtmlTags</a>
</td><td><a href="#addneverallowedcallstringsstring-strings-this">addNeverAllowedCallStrings</a>
</td></tr><tr><td><a href="#addneverallowedjscallbackregexstring-strings-this">addNeverAllowedJsCallbackRegex</a>
</td><td><a href="#addneverallowedoneventsafterwardsstring-strings-this">addNeverAllowedOnEventsAfterwards</a>
</td><td><a href="#addneverallowedregexstring-strings-this">addNeverAllowedRegex</a>
</td><td><a href="#addneverallowedstrafterwardsstring-strings-this">addNeverAllowedStrAfterwards</a>
</td></tr><tr><td><a href="#isxssfound-boolnull">isXssFound</a>
</td><td><a href="#removedonotclosehtmltagsstring-strings-this">removeDoNotCloseHtmlTags</a>
</td><td><a href="#removeevilattributesstring-strings-this">removeEvilAttributes</a>
</td><td><a href="#removeevilhtmltagsstring-strings-this">removeEvilHtmlTags</a>
</td></tr><tr><td><a href="#removeneverallowedcallstringsstring-strings-this">removeNeverAllowedCallStrings</a>
</td><td><a href="#removeneverallowedjscallbackregexstring-strings-this">removeNeverAllowedJsCallbackRegex</a>
</td><td><a href="#removeneverallowedoneventsafterwardsstring-strings-this">removeNeverAllowedOnEventsAfterwards</a>
</td><td><a href="#removeneverallowedregexstring-strings-this">removeNeverAllowedRegex</a>
</td></tr><tr><td><a href="#removeneverallowedstrafterwardsstring-strings-this">removeNeverAllowedStrAfterwards</a>
</td><td><a href="#setreplacementstring-string-this">setReplacement</a>
</td><td><a href="#setstripe4bytecharsbool-bool-this">setStripe4byteChars</a>
</td><td><a href="#xss_cleanstringstring-str-stringstring">xss_clean</a>
</td></tr></table>

## addDoNotCloseHtmlTags(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_do_not_close_html_tags"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addEvilAttributes(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_evil_attributes"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addEvilHtmlTags(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_evil_html_tags"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addNeverAllowedCallStrings(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_never_allowed_call_strings"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addNeverAllowedJsCallbackRegex(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_never_allowed_js_callback_regex"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addNeverAllowedOnEventsAfterwards(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_never_allowed_on_events_afterwards"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addNeverAllowedRegex(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_never_allowed_regex"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## addNeverAllowedStrAfterwards(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Add some strings to the "_never_allowed_str_afterwards"-array.

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## isXssFound(): bool|null
<a href="#voku-php-readme-class-methods">↑</a>
Check if the "AntiXSS->xss_clean()"-method found an XSS attack in the last run.

**Parameters:**
__nothing__

**Return:**
- `bool|null <p>Will return null if the "xss_clean()" wasn't running at all.</p>`

--------

## removeDoNotCloseHtmlTags(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_do_not_close_html_tags"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeEvilAttributes(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_evil_attributes"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeEvilHtmlTags(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_evil_html_tags"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeNeverAllowedCallStrings(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_never_allowed_call_strings"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeNeverAllowedJsCallbackRegex(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_never_allowed_js_callback_regex"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeNeverAllowedOnEventsAfterwards(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_never_allowed_on_events_afterwards"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeNeverAllowedRegex(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_never_allowed_regex"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## removeNeverAllowedStrAfterwards(string[] $strings): $this
<a href="#voku-php-readme-class-methods">↑</a>
Remove some strings from the "_never_allowed_str_afterwards"-array.

<p>
<br />
WARNING: Use this method only if you have a really good reason.
</p>

**Parameters:**
- `string[] $strings`

**Return:**
- `$this`

--------

## setReplacement(string $string): $this
<a href="#voku-php-readme-class-methods">↑</a>
Set the replacement-string for not allowed strings.

**Parameters:**
- `string $string`

**Return:**
- `$this`

--------

## setStripe4byteChars(bool $bool): $this
<a href="#voku-php-readme-class-methods">↑</a>
Set the option to stripe 4-Byte chars.

<p>
<br />
INFO: use it if your DB (MySQL) can't use "utf8mb4" -> preventing stored XSS-attacks
</p>

**Parameters:**
- `bool $bool`

**Return:**
- `$this`

--------

## xss_clean(string|string[] $str): string|string[]
<a href="#voku-php-readme-class-methods">↑</a>
XSS Clean

<p>
<br />
Sanitizes data so that "Cross Site Scripting" hacks can be
prevented. This method does a fair amount of work but
it is extremely thorough, designed to prevent even the
most obscure XSS attempts. But keep in mind that nothing
is ever 100% foolproof...
</p>

<p>
<br />
<strong>Note:</strong> Should only be used to deal with data upon submission.
  It's not something that should be used for general
  runtime processing.
</p>

**Parameters:**
- `TXssCleanInput $str <p>input data e.g. string or array of strings</p>`

**Return:**
- `string|string[]`

--------


### Support

For support and donations please visit [Github](https://github.com/voku/anti-xss/) | [Issues](https://github.com/voku/anti-xss/issues) | [PayPal](https://paypal.me/moelleken) | [Patreon](https://www.patreon.com/voku).

For status updates and release announcements please visit [Releases](https://github.com/voku/anti-xss/releases) | [Twitter](https://twitter.com/suckup_de) | [Patreon](https://www.patreon.com/voku/posts).

For professional support please contact [me](https://about.me/voku).

### Thanks

- Thanks to [GitHub](https://github.com) (Microsoft) for hosting the code and a good infrastructure including Issues-Managment, etc.
- Thanks to [IntelliJ](https://www.jetbrains.com) as they make the best IDEs for PHP and they gave me an open source license for PhpStorm!
- Thanks to [Travis CI](https://travis-ci.com/) for being the most awesome, easiest continous integration tool out there!
- Thanks to [StyleCI](https://styleci.io/) for the simple but powerfull code style check.
- Thanks to [PHPStan](https://github.com/phpstan/phpstan) && [Psalm](https://github.com/vimeo/psalm) for relly great Static analysis tools and for discover bugs in the code!

### License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fvoku%2Fanti-xss.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fvoku%2Fanti-xss?ref=badge_large)
