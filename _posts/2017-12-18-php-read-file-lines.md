---
tags: php i/o
description: read a file, then iterate line by line
---
Iterate over the set of lines from a file:

```php
$lines = file('/tmp/test.txt');
foreach ($lines as $num => $line) {
  echo sprintf("%d: %s", $num, $line);
}

```
