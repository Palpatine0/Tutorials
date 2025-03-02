# Project Logics - PHP

# <span style="color: #990000">Tools</span>

# 1 APIs

## Time

### String to timestamp

**PHP**

```php
strtotime("2024-03-05");
```

#### Timestamp to string

```php
{$REPLACE|date='Y-m-d H:i:s',###}
```

## Data Transmission

**HTML**
```html
<input type="text" class="huge bLeftRequire input " name="uid" id='uid' value="{$uid}">
```

# 2 Variables

## $_SERVER

**`$_SERVER['HTTP_HOST']`**

Get current host name

```php
$url = "http://{$_SERVER['HTTP_HOST']}" ;
```

---

# 3 Functional Code Combos

## Ajax

**html**

```html
<input type="text" class="input input-small" name="REPLACE" size="10" placeholder="" value="{$Think.request.REPLACE}" onchange="get_REPLACE(this.value)"/>
<div class="REPLACE_ajax" style="display: none">
    <span class="REPLACE_ajax_span center_h" style="margin-top: 8px;"></span>
</div>
```

```javascript
function get_REPLACE(uid) {
    var REPLACE_ajax = document.querySelector('.REPLACE_ajax');
    if (REPLACE_ajax) {
        REPLACE_ajax.style.display = 'block';
        var nameTestSpan = REPLACE_ajax.querySelector('.REPLACE_ajax_span');
        if (nameTestSpan) {
            $.get("__GROUP__/MODEL_NAME/get_REPLACE/",
                {uid: uid},
                function (res) {
                    if (res.XXX != null) {
                        nameTestSpan.innerHTML = res.XXX;
                    } else {
                        nameTestSpan.innerHTML = '查找目标不存在';
                    }
                }
            );
        }
    }
}
```

**php**
```php
function get_REPLACE() {
    header('Content-Type: application/json');
    $uid = I('get.uid');
    ...
    echo json_encode($res);
}
```

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

**php**

```php
if (isset($_REQUEST['[R]']) && $_REQUEST['[R]'] != "") {
    $map["[R]"] = array('like', '%' . $_REQUEST['[R]'] . '%');
}
```

