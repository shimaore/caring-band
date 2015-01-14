    chai = require 'chai'
    chai.should()
    {data} = require '../README'

    describe 'Data', ->

      c = new data()

      c.add 3
      c.add 5
      c.add 0.9

      it 'should count properly', ->
        c.should.have.property 'count'
        chai.expect( c.count.equals 3 ).to.be.true

      it 'should min properly', ->
        c.should.have.property 'min'
        chai.expect( c.min.equals 0.9 ).to.be.true

      it 'should max properly', ->
        c.should.have.property 'max'
        chai.expect( c.max.equals 5 ).to.be.true

      it 'should sum properly', ->
        c.should.have.property 'sum'
        chai.expect( c.sum.equals 8.9 ).to.be.true

      it 'should sumsqr properly', ->
        c.should.have.property 'sumsqr'
        chai.expect( c.sumsqr.equals 34.81 ).to.be.true

      it 'should last properly', ->
        c.should.have.property 'last'
        chai.expect( c.last.equals 0.9 ).to.be.true

      it 'should JSON properly', ->
        j = JSON.parse c.toJSON()
        j.should.have.property 'count', 3
        j.should.have.property 'min', 0.9
        j.should.have.property 'max', 5
        j.should.have.property 'sum', 8.9
        j.should.have.property 'sumsqr', 34.81
        j.should.have.property 'last', 0.9

      it 'should JSON properly when empty', ->
        d = new data()
        j = JSON.parse d.toJSON()
        j.should.have.property 'count', 0
