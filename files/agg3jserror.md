```js
MongoDB shell version: 2.8.0-rc0
connecting to: power2
{
	"_firstBatch" : [
		{
			"_id" : "04",
			"count" : 480
		},
		{
			"_id" : "05",
			"count" : 467
		},
		{
			"_id" : "06",
			"count" : 403
		}
	],
	"_cursor" : {
		"next" : function next() { [native code] },
		"hasNext" : function hasNext() { [native code] },
		"objsLeftInBatch" : function objsLeftInBatch() { [native code] },
		"readOnly" : function readOnly() { [native code] }
	},
	"hasNext" : function () {
    return this._firstBatch.length || this._cursor.hasNext();
},
	"next" : function () {
    if (this._firstBatch.length) {
        // $err wouldn't be in _firstBatch since ok was true.
        return this._firstBatch.pop();
    }
    else {
        var ret = this._cursor.next();
        if ( ret.$err )
            throw Error( "error: " + tojson( ret ) );
        return ret;
    }
},
	"objsLeftInBatch" : function () {
    if (this._firstBatch.length) {
        return this._firstBatch.length;
    }
    else {
        return this._cursor.objsLeftInBatch();
    }
},
	"help" : function () {
    // This is the same as the "Cursor Methods" section of DBQuery.help().
    print("\nCursor methods");
    print("\t.toArray() - iterates through docs and returns an array of the results")
    print("\t.forEach( func )")
    print("\t.map( func )")
    print("\t.hasNext()")
    print("\t.next()")
    print("\t.objsLeftInBatch() - returns count of docs left in current batch (when exhausted, a new getMore will be issued)")
    print("\t.itcount() - iterates through documents and counts them")
    print("\t.pretty() - pretty print each document, possibly over multiple lines")
},
	"toArray" : function (){
    if ( this._arr )
        return this._arr;

    var a = [];
    while ( this.hasNext() )
        a.push( this.next() );
    this._arr = a;
    return a;
},
	"forEach" : function ( func ){
    while ( this.hasNext() )
        func( this.next() );
},
	"map" : function ( func ){
    var a = [];
    while ( this.hasNext() )
        a.push( func( this.next() ) );
    return a;
},
	"itcount" : function (){
    var num = 0;
    while ( this.hasNext() ){
        num++;
        this.next();
    }
    return num;
},
	"shellPrint" : function (){
    try {
        var start = new Date().getTime();
        var n = 0;
        while ( this.hasNext() && n < DBQuery.shellBatchSize ){
            var s = this._prettyShell ? tojson( this.next() ) : tojson( this.next() , "" , true );
            print( s );
            n++;
        }
        if (typeof _verboseShell !== 'undefined' && _verboseShell) {
            var time = new Date().getTime() - start;
            print("Fetched " + n + " record(s) in " + time + "ms");
        }
         if ( this.hasNext() ){
            print( "Type \"it\" for more" );
            ___it___  = this;
        }
        else {
            ___it___  = null;
        }
   }
    catch ( e ){
        print( e );
    }

},
	"pretty" : function (){
    this._prettyShell = true;
    return this;
}
}
```
