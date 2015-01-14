Statistical Object
==================

Allows to easily store: count, min, max, last, sum, sum-of-squares.
This is inspired by CouchDB's [`_stats`](https://wiki.apache.org/couchdb/Built-In_Reduce_Functions#A_stats) built-in reduce function.
Results are stored as [big-rational](https://github.com/peterolson/BigRational.js) numbers.

    bigRat = require 'big-rational'

Each statistical object stored offers the following fields:
- `count`, the number of items submitted
- `min`, the numerically smallest item submitted
- `max`, the numerically largest item submitted
- `last`, the most recent item submitted
- `sum`, the numerical sum of all items submitted
- `sumsqr`, the sum of squares of each item submitted

A new item is submitted either:
- by using `add(number)`;
- by using `add()`, in which case the number is assumed to be 0 (and only `count` is meaningful).

    class CaringBandData
      constructor: ->
        @count = new bigRat 0
        @min = null
        @max = null
        @last = null
        @sum = new bigRat 0
        @sumsqr = new bigRat 0

      add: (number) ->
        number ?= 0
        number = new bigRat number
        @last = number
        @count = @count.add 1
        if not @min? or @min.greater number
          @min = number
        if not @max? or number.greater @max
          @max = number

        @sum = @sum.add number
        @sumsqr = @sumsqr.add number.times number
        this

      toJSON: (decimals) ->
        text = []
        for own k,v of this when v?
          text.push """
            "#{k}": #{v.toDecimal decimals}
          """
        "{#{text.join ', '}}"

Storage Object
==============

The module exports a storage object which allows you to:
- `add(key,item)`: add a new numerical item to a key's statistical object; returns the new statistical object;
- `add(key)`: count a new entry; the value is assumed to be zero; returns the new statistical object;
- `get(key)`: retrieve the statistical object defined above;
- `delete(key)`: delete the statisitcal object for the key.

Keys might be any JSON-compatible types.

    class CaringBand

      constructor: ->
        @data = {}

      add: (key,number) ->
        if typeof key isnt 'string'
          key = JSON.stringify key

        @data[key] ?= new CaringBandData()
        @data[key].add number

      get: (key) ->
        if typeof key isnt 'string'
          key = JSON.stringify key
        @data[key]

      delete: (key) ->
        if typeof key isnt 'string'
          key = JSON.stringify key
        delete @data[key]

    module.exports = CaringBand
    module.exports.data = CaringBandData
