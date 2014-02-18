#CurlWrapper

The simplest OOP-style wrapper for the standard php curl functions.
The main purpose is to make code that uses curl calls testable. We do it by injecting the Curl object as a dependency instead of calling curl functions directly.


Not testabe code. Hard-coded dependencies.
```php
class MyApiClient {
    ...
    function call($url)
    {
        $ch = curl_init($url);
        curl_set_opt($ch, ...)
        return curl_exec($ch);
    }
}
```


Testable code. Curl object is injected, so can be easily mocked in PHPUnit.
```php
class MyApiClient {
    private $curl;
    function __construct(\F3\CurlWrapper\Curl $curl)
    {
        $this->curl = $curl;
    }
    ...
    function call($url)
    {
        $this->curl->init($url);
        $this->curl->setOpt(...)
        return $this->curl->exec($ch);
    }
}
```


##Basic usage examples

Functional to OOP style mapping

| Functional                        | OOP |
| ---                               | --- |
| `$h = curl_init($url);`           | `$curl = new Curl($url);` or `$curl->init($url)` |
| `curl_close($h);`                 | `unset($curl);` |
| `$e = curl_errno($h);`            | `$e = $curl->errno();` |
| `$e = curl_error($h);`            | `$e = $curl->error();` |
| `$i = curl_getinfo($h, $opt);`    | `$i = $curl->getInfo($opt);` |
| `curl_setopt($h, $opt, $val);`    | `$curl->setOpt($opt, $val);` |
| `curl_setopt_array($h, $array);`  | `$curl->setOptArray($array);` |
| `curl_version($age)`              | `Curl::version($age);` |
| `curl_strerror($errornum)`        | `Curl::strerror($errornum);` |
| `$h2 = curl_copy_handle($h);`     | `$curl2 = clone($curl);` |
| `$result = curl_exec($h);`        | `$result = $curl->exec();` |
| `$res = curl_pause($h, $mask);`   | `$res = $curl->pause($mask);` |
| `$res = curl_escape($h, $str);`   | `$res = $curl->escape($str);` |
| `$res = curl_unescape($h, $str);` | `$res = $curl->unescape($str);` |

| `curl_multi_init();`                           |   `$cm = new CurlMulti();` |
| `curl_multi_close($h);`                        |   `unset($cm);` |
| `$i = curl_multi_add_handle($mh, $ch);`        |   `$i = $cm->add($curl);` |
| `$i = curl_multi_remove_handle($mh, $ch);`     |   `$i = $cm->remove($curl);` |
| `$i = curl_multi_exec($mh, $running);`         |   `$i = $cm->exec($running);` |
| `$s = curl_multi_getcontent($ch);`             |   `$s = $cm->getContent($curl);` |
| `$a = curl_multi_info_read($mh, $msgs);`       |   `$a = $cm->infoRead($msgs);` |
| `$i = curl_multi_select($mh, $timeout);`       |   `$i = $cm->select($timeout);` |
| `$r = curl_multi_setopt($h, $opt, $val);`      |   `$r = $cm->setOpt($opt, $val);` |

| `curl_share_init();`                           |   `$cs = new CurlShare();` |
| `curl_share_close($h);`                        |   `unset($cs);` |
| `$r = curl_multi_setopt($h, $opt, $val);`      |   `$r = $cs->setOpt($opt, $val);` |
