# Project Logics-PHP

# API

## Time

### string to timestamp

**PHP**

```php
strtotime("2024-03-05");
```
### timestamp to string 
```php
{$REPLACE|date='Y-m-d H:i:s',###}
```

# _Templates_

## Search Bar

### text

**html**

```html

<div class="form-group">
    <div class="label">
        <label for="[R]">[R]</label>
    </div>
    <div class="field">
        <input type="text" class="input input-small" id="[R]" name="[R]" size="10" placeholder="[R]" value="{$Think.request.[R]}"/>
    </div>
</div>
```

```php
if (isset($_REQUEST['[R]']) && $_REQUEST['[R]'] != "") {
    $map["[R]"] = array('like', '%' . $_REQUEST['[R]'] . '%');
}
```

