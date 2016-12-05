# getCollectionProducts

Получаем массив продуктов из коллекции. 

## Аргументы

- `handle` handle коллекции
- `count_product` колличество товаров в коллекции
- `filter` параметры для запроса, например для фильтрации по характеристикам
```js
// примеры вызовов
getCollectionProducts('all', 'full', { characteristics: [ 18775928, 18775929 ] }).done(function (products) {
  console.log(products);
});
getCollectionProducts('all', 'full', 'characteristics%5B%5D=18775928').done(function (products) {
  console.log(products);
});
getCollectionProducts('all', 15).done(function (products) {
  console.log(products);
});
getCollectionProducts('all', 'full').done(function (products) {
  console.log(products);
});
```

```js
/**
 * Получаем массив продуктов из коллекции.
 * handle - пермалинк коллекции
 * count_product - число || 'full';
 * filter - сериализованная форма, либо объект;
 */
function getCollectionProducts(handle, count_product, filter) {
  return $.when(_getCollectionProducts (handle, count_product, filter));

  function _getCollectionProducts(handle, count_product, _filter) {
    var dfd = jQuery.Deferred();
    var sizeIsFull = (typeof _size == 'string') && count_product === 'full';
    var size = (sizeIsFull) ? false : count_product;
    var _pagePag = Math.ceil(size / 100);
    var _size = '&page_size=100';
    if (!sizeIsFull && count_product < 100) {
      _size = '&page_size=' + count_product;
    }
    var _products = [];
    var filter = _filter || '';
    if (typeof filter == 'object') {
      filter = $.param(filter);
    }

    if (!sizeIsFull) {
      var _index = 1;
      for (var i = 0; i < _pagePag; i++) {
        var _i = i + 1;
        var _page = '&page=' + _i;
        var _param = filter + _page + _size;
        $.when( _getCollectionList(handle, _param) )
        .done(function( _collection ) {
          $.merge(_products, _collection.products);
          if (_index == _pagePag) {
            dfd.resolve( _products );
          }
          _index++;
        })
        .fail(function( error ) {
          dfd.reject( error );
        })
      }
    }

    if (sizeIsFull) {
      var _pageOne = '&page=1';
      var _param = filter + _pageOne + _size;
      $.when( _getCollectionList(handle, _param) )
      .done(function( _collection ) {
        $.merge(_products, _collection.products);
function getCollectionProducts(handle, count_product, filter) {
  return $.when(_getCollectionProducts (handle, count_product, filter));

  function _getCollectionProducts(handle, count_product, _filter) {
    var dfd = jQuery.Deferred();
    var sizeIsFull = (typeof count_product == 'string') && count_product === 'full';
    var size = (sizeIsFull) ? false : count_product;
    var _pagePag = Math.ceil(size / 100);
    var _size = '&page_size=100';
    if (!sizeIsFull && count_product < 100) {
      _size = '&page_size=' + count_product;
    }
    var _products = [];
    var filter = _filter || '';
    if (typeof filter == 'object') {
      filter = $.param(filter);
    }

    console.log(filter, sizeIsFull);

    if (!sizeIsFull) {
      var _index = 1;
      for (var i = 0; i < _pagePag; i++) {
        var _i = i + 1;
        var _page = '&page=' + _i;
        var _param = filter + _page + _size;
        $.when( _getCollectionList(handle, _param) )
        .done(function( _collection ) {
          $.merge(_products, _collection.products);
          if (_index == _pagePag) {
            dfd.resolve( _products );
          }
          _index++;
        })
        .fail(function( error ) {
          dfd.reject( error );
        })
      }
    }

    if (sizeIsFull) {
      var _pageOne = '&page=1';
      var _param = filter + _pageOne + _size;
      $.when( _getCollectionList(handle, _param) )
      .done(function( _collection ) {
        $.merge(_products, _collection.products);
        var isMOre = (_collection.count / 100) > 1
        if (isMOre) {
          var _index = 2;
          var _pagePaginate = Math.ceil(_collection.count / 100);
          for (var i = 1; i < _pagePaginate; i++) {
            var _i = i + 1;
            var _page = '&page=' + _i;
            var _param = filter + _page + _size;
            $.when( _getCollectionList(handle, _param) )
            .done(function( _collection ) {
              $.merge(_products, _collection.products);
              if (_index == _pagePaginate) {
                dfd.resolve( _products );
              }
              _index++;
            })
            .fail(function( error ) {
              dfd.reject( error );
            })
          }
        }else{
          dfd.resolve( _products );
        }
      })
      .fail(function( error ) {
        dfd.reject( error );
      })
    }
    return dfd.promise();
  }

  function _getCollectionList ( handle, param ){
    var dfd = jQuery.Deferred();

    $.ajax({
      url: '/collection/' +handle+ '.json',
      type: 'POST',
      dataType: 'json',
      data: param
    })
    .done(function(collection) {
      dfd.resolve( collection );
    })
    .fail(function(error) {
      dfd.reject( error.responseText );
    })

    return dfd.promise();
  }
}
```
