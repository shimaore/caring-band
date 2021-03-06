    chai = require 'chai'
    chai.should()
    band = require '../README'

    describe 'Band', ->

      b = new band()
      b.add ['hello'], 3
      b.add ['hello'], 5
      b.add ['hello'], 0.9

      b.add 'hello', 4
      b.add 'hello', -1
      b.add 'hello', 7

      it 'should report additions', (done) ->
        b.on 'add', (data) ->
          data.should.have.property 'key', 'hello'
          data.should.have.property 'number'
          data.number.should.have.property 'numerator'
          data.number.should.have.property 'denominator'
          chai.expect( data.number.numerator.equals 1 ).to.be.true
          chai.expect( data.number.denominator.equals 5 ).to.be.true
          data.should.have.property 'value'
          data.value.should.have.property 'count'
          data.value.should.have.property 'sumsqr'
          done()

        b.add 'hello', 0.2

      it 'should count properly', ->
        c = b.get ['hello']
        c.should.have.property 'count'
        chai.expect( c.count.equals 3 ).to.be.true

      it 'should min properly', ->
        c = b.get ['hello']
        c.should.have.property 'min'
        chai.expect( c.min.equals 0.9 ).to.be.true

      it 'should max properly', ->
        c = b.get ['hello']
        c.should.have.property 'max'
        chai.expect( c.max.equals 5 ).to.be.true

      it 'should sum properly', ->
        c = b.get ['hello']
        c.should.have.property 'sum'
        chai.expect( c.sum.equals 8.9 ).to.be.true

      it 'should sumsqr properly', ->
        c = b.get ['hello']
        c.should.have.property 'sumsqr'
        chai.expect( c.sumsqr.equals 34.81 ).to.be.true

      it 'should last properly', ->
        c = b.get ['hello']
        c.should.have.property 'last'
        chai.expect( c.last.equals 0.9 ).to.be.true

      it 'should count properly', ->
        c = b.get 'hello'
        c.should.have.property 'count'
        chai.expect( c.count.equals 4 ).to.be.true

      it 'should min properly', ->
        c = b.get 'hello'
        c.should.have.property 'min'
        chai.expect( c.min.equals -1 ).to.be.true

      it 'should max properly', ->
        c = b.get 'hello'
        c.should.have.property 'max'
        chai.expect( c.max.equals 7 ).to.be.true

      it 'should sum properly', ->
        c = b.get 'hello'
        c.should.have.property 'sum'
        chai.expect( c.sum.equals 10.2 ).to.be.true

      it 'should sumsqr properly', ->
        c = b.get 'hello'
        c.should.have.property 'sumsqr'
        chai.expect( c.sumsqr.equals 66.04 ).to.be.true

      it 'should last properly', ->
        c = b.get 'hello'
        c.should.have.property 'last'
        chai.expect( c.last.equals 0.2 ).to.be.true

      it 'should JSON properly', ->

Semantic testing

        c = JSON.parse b.toJSON 7
        c.should.have.property 'hello'
        c.hello.should.have.property 'sum', 10.2
        c.should.have.property '["hello"]'
        c['["hello"]'].should.have.property 'sumsqr', 34.81

Litteral value testing

        (b.toJSON 7).should.eq '{"[\\"hello\\"]":{"count": 3, "min": 0.9, "max": 5, "last": 0.9, "sum": 8.9, "sumsqr": 34.81},"hello":{"count": 4, "min": -1, "max": 7, "last": 0.2, "sum": 10.2, "sumsqr": 66.04}}'

      it 'should report deletions', (done) ->
        b.on 'delete', (data) ->
          data.should.have.property 'key', 'hello'
          done()
        b.delete 'hello'
