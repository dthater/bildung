# fetch-API Übung

## Covid-19 Inzidenz

Lade die Titel der Covid-19 Datei `https://raw.githubusercontent.com/opendataddorf/od-resources/master/COVID_Duesseldorf.csv` und zeige sie in einem Array an.

    // JavaScript
    let src = "https://raw.githubusercontent.com/opendataddorf/od-resources/master/COVID_Duesseldorf.csv";
    fetch(src).then(data => data.text()).then(csv => csv.split("\n")[0].replaceAll("\r", "").split(",")).then(data => console.table(data));

## Verkehrsbehinderungen

Ermittle die aktuelle Lage anhand aller Meldungen zu Verkehrbehinderungen in Düsseldorf. Verwende folgenden Datensatz: `https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=dc891229-7cef-499a-9b61-020af4cbf9a6`

    // JavaScript
    let src = "https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=dc891229-7cef-499a-9b61-020af4cbf9a6&limit=100";
    fetch(src).then(data => data.json()).then(json => json.result.records).then(array => console.table(array.map( t => t.comment )));

## Private Kfz-Zulassungen

Ermittle die Anzahl privater männlicher und weiblicher Düsseldorfer Pkw-Halter aus 2018. Verwende folgenden Datensatz: `https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=00987ac2-7997-4662-a3b2-a4f81be4af56`

    let src = "https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=00987ac2-7997-4662-a3b2-a4f81be4af56";
    fetch(src).then(data => data.json()).then(json => json.result.records).then(array => console.table(array.filter(t => t.jahr == "2018").map(t => ({ maennlich: parseInt(t.davon_maennlich), weiblich: parseInt(t.davon_weiblich) }) )[0]));


## Fahrzeugbestand in Düsseldorf

Berechne den Faktor der verschiedenen Fahrzeuge in Düsseldorf nach Treibstoffart und gebe die Daten in % aus. Orientiere dich an dieser Seite: `https://opendata.duesseldorf.de/dataset/pkw-bestand-nach-antriebsart`

    let src = "https://opendata.duesseldorf.de/api/action/datastore/search.json?resource_id=6a1f1db6-d9e0-4627-b10f-6497abacd627";
    fetch(src).then(data => data.json()).then(json => json.result.records).then(array => array.filter(t => t.jahr == "2018")[0]).then( ds => {
        var keys = Object.keys(ds).filter( u => ["jahr","entry_id"].indexOf(u) < 0 );
        var gesamt = keys.map( u => parseInt(ds[u]) ).reduce( (u,d) => (d||0)+u )
        keys.forEach( u => ds[u] = Math.floor(1000*(parseInt(ds[u])/gesamt))/10 )
        return ds
    } ).then(z => console.log(z));

## Fette Taschenmonster

Ermittle das Lebendgewicht des Pokemon-Universums (der ersten 20 Pokemon). Verwende `https://pokeapi.co/api/v2/pokemon`

    new Array(20).fill(0).forEach( (t,i,arr) => {
    fetch(`https://pokeapi.co/api/v2/pokemon/${i+1}/`).then( data => data.json() ).then( json => json.weight ).then( w => arr[i] = w ).then( t => console.log(arr.reduce( (t1, d) => (d||0)+t1)) )

