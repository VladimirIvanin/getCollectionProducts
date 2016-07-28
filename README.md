# getCollectionProducts

Получаем массив продуктов

```js
$.when(getCollectionProducts('all', {{collections.all.products.size}}, '')).done(function (products) {
  console.log(products);
})
.fail(function (error) {
  console.error(error);
});

function getCollectionProducts(name, count, params) {
  var dfd = jQuery.Deferred();

  var _pagePag = Math.ceil(count / 100);
  var _size = '&page_size=100';
  var _products = []
  
  var _index = 1;
  for (var i = 0; i < _pagePag; i++) {
    var _i = i + 1;
    var _page = '&page=' + _i;
    var _param = params + _page + _size;
    $.when( getCollectionList('all', _param) )
      .done(function( products ) {
        $.merge(_products, products);
        if (_index == _pagePag) {
          dfd.resolve( _products );
        }
        _index++;
      })
      .fail(function( error ) {
        dfd.reject( error );
      })
  }
  return dfd.promise();
}

function getCollectionList ( name, param ){
  var dfd = jQuery.Deferred();

  $.ajax({
    url: '/collection/' +name+ '.json',
    type: 'POST',
    dataType: 'json',
    data: param
  })
  .done(function(collection) {
    dfd.resolve( collection.products );
  })
  .fail(function(error) {
    dfd.reject( error.responseText );
  })

  return dfd.promise();
}
```
