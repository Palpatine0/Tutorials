# Project Logics - Vue

## <span style="color: #990000">Tools</span>
# 2 Template

##### Mobile responsive
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

##### Bootstrap import
**International version**

- V4
```html
<!--bootstrap import-->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.1/dist/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
<!--/bootstrap import-->
```
**Chinese version**

- V4
```html
<!--bootstrap import-->
<link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.6.0/css/bootstrap.min.css">
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.6.0/js/bootstrap.bundle.min.js"></script>
<!--/bootstrap import-->
```
- V5
```html
<!-- Bootstrap import V5 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.1/css/bootstrap.css" rel="stylesheet">
<link href="https://cdn.bootcdn.net/ajax/libs/bootstrap-icons/1.11.0/font/bootstrap-icons.css" rel="stylesheet">
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.1/js/bootstrap.bundle.min.js"></script>
<!-- /Bootstrap import V5 -->
```
 

##### render raw html content 
```html
<p v-html="analysis.explain"></p>
```

##### append calculated data in tag element
```html
<div :style="{ width: `${(dataset.comment_count / dataset.positive_feedback_count) * 100}%` }">
```

##### condition check to decide whether hidden for a class
```html
<div class="panel-body" :class="{ 'hidden': !isPanelVisible(resultIndex)&&!(result.sort_key==dataset.list_exam_user.length) }">
```


 

