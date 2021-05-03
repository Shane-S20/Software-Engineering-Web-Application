<script>

    class Distances_Calc
    {
        constructor(values)
        {
            this.from = values.from
            this.to = values.to
        }
    }

    let routes_List  = [
        new Distances_Calc({from: "Point A", to: "Point B"})
    ]

    const addToList = () => {
        const newDistance = new Distances_Calc({from: document.getElementById("Origin").value, to: document.getElementById('Destination').value})
        routes_List = routes_List.concat([newDistance])
        

    }

    const CalcRoute = () => {
        document.getElementById('printResults').innerHTML = "Going from " + document.getElementById("Origin").value + " to " + document.getElementById('Destination').value + 
        " would take ___ hours and ___ minutes  and is ___ miles apart.";
    }


//Default location is Hofstra, and a roadmap
var defaultLatLong = new google.maps.LatLng(40.71684,-73.600433);
function initialize()
{
    var mapSettings = {
    center: defaultLatLong, zoom: 15, mapTypeID: google.maps.MapTypeId.ROADMAP};

    var map = new google.maps.Map(document.getElementById("GoogleMap"), mapSettings)

    var marker = new google.maps.Marker({
        position: defaultLatLong,
    });

    marker.setMap(map);
}

google.maps.event.addDomListener(window, 'load', initialize);

//var directionsService = new google.maps.DirectionService()

//var displayDirections = new google.maps.DirectionsRenderer();

//displayDirections.setMap(map);

// var options = {
//     types: ['(cities)']
// }

// var originInput = document.getElementById("Origin");
// var originAutocomplete = new google.maps.places.Autocomplete(originInput, options);
// var destInput = document.getElementById("Destination");
// var destAutocomplete = new google.maps.places.Autocomplete(destInput, options);


</script>


<span id = "inputText1">Origin: </span> 
<input type = "text" id = "Origin" size ="20">
<span id = "inputText2">Destination: </span> 
<input type = "text" id = "Destination" size ="20">
<button id = "button" on:click={CalcRoute} on:click={addToList}> Calculate </button>
<br>
<br> 
<p id = "printResults"> </p>
<hr>
<h2 id = "history"> Previous Entries </h2>

<routes_List>
    {#each routes_List as Distances_Calc}
        <distances-Calc>
            <input type = "text" id = "previousOrigin" bind:value = {Distances_Calc.from}/>
            <p id = "to"> to </p>
            <input type = "text" id = "previousDest" bind:value = {Distances_Calc.to}/>
        </distances-Calc>   
    {/each}
</routes_List>