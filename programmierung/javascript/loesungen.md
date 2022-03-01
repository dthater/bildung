# prototype Übungen

## Bequemer Array Zugriff

    Array.prototype.first = function(){ return this[0] }
    Array.prototype.last = function(){ return this[this.length-1] }

## Hund, Katze, Dino

    function Lebewesen(alter){ 
        this.alter = alter 
    }

    function Haustier(alter, name){
        Lebewesen.call(this, alter) 
        this.name = name
    }
    Haustier.prototype = new Lebewesen()
    Object.assign( Haustier.prototype, {
        constructor: Haustier
    })

    function Hund(alter, name){
        Haustier.call(this, alter, name) 
    }
    Hund.prototype = new Haustier()
    Object.assign( Hund.prototype, {
        constructor: Hund
    })

    function Katze(alter, name){
        Haustier.call(this, alter, name) 
    }
    Katze.prototype = new Haustier()
    Object.assign( Katze.prototype, {
        constructor: Katze
    })

    function Maus(alter, name){
        Haustier.call(this, alter, name) 
    }
    Maus.prototype = new Haustier()
    Object.assign( Maus.prototype, {
        constructor: Maus
    })

    console.log([ new Hund(12, "Rex"), new Katze(10, "Miau"), new Maus(1, "Piep") ])

# fetch-API Übungen

## Covid-19 Inzidenz

    // JavaScript
    let src = "https://raw.githubusercontent.com/opendataddorf/od-resources/master/COVID_Duesseldorf.csv";
    fetch(src).then(data => data.text()).then(csv => csv.split("\n")[0].replaceAll("\r", "").split(",")).then(data => console.table(data));

## Verkehrsbehinderungen

    // JavaScript
    let src = "https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=dc891229-7cef-499a-9b61-020af4cbf9a6&limit=100";
    fetch(src).then(data => data.json()).then(json => json.result.records).then(array => console.table(array.map( t => t.comment )));

## Private Kfz-Zulassungen

    let src = "https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=00987ac2-7997-4662-a3b2-a4f81be4af56";
    fetch(src).then(data => data.json()).then(json => json.result.records).then(array => console.table(array.filter(t => t.jahr == "2018").map(t => ({ maennlich: parseInt(t.davon_maennlich), weiblich: parseInt(t.davon_weiblich) }) )[0]));

## Fahrzeugbestand in Düsseldorf

    let src = "https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=6a1f1db6-d9e0-4627-b10f-6497abacd627";
    fetch(src).then(data => data.json()).then(json => json.result.records).then(array => array.filter(t => t.jahr == "2018")[0]).then( ds => {
        var keys = Object.keys(ds).filter( u => ["jahr","entry_id"].indexOf(u) < 0 );
        var gesamt = keys.map( u => parseInt(ds[u]) ).reduce( (u,d) => (d||0)+u )
        keys.forEach( u => ds[u] = Math.floor(1000*(parseInt(ds[u])/gesamt))/10 )
        return ds
    } ).then(z => console.log(z));

## Fette Taschenmonster

    new Array(20).fill(0).forEach( (t,i,arr) => {
    fetch(`https://pokeapi.co/api/v2/pokemon/${i+1}/`).then( data => data.json() ).then( json => json.weight ).then( w => arr[i] = w ).then( t => console.log(arr.reduce( (t1, d) => (d||0)+t1)) )

