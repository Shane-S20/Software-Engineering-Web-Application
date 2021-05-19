<script>

    class Route
    {
        constructor(values)
        {
            this.from = values.from
            this.to = values.to
        }
    }

    let Previous_Routes_List  = [
    ]

    //Add route entry to previous routes list
    const addToList = () => {
        const newDistance = new Route({from: document.getElementById("Origin").value, to: document.getElementById('Destination').value})
        Previous_Routes_List = Previous_Routes_List.concat([newDistance])
    }
    //Delete previous routes from the routes list
    const deleteFromList = () => {
        if(Previous_Routes_List.length > 1)
        {
            document.getElementById("listEntry").remove();
            Previous_Routes_List.pop();
            console.log(Previous_Routes_List);
        }
        else
            alert("All previous entries have been deleted");
    }

    //Google Maps Integration
    //Source for APIs with documentation 
    //https://developers.google.com/maps/documentation/javascript/overview
    var Hofstra = new google.maps.LatLng(40.71684,-73.600433);

    var mapSettings = { 
        center: Hofstra, 
        zoom: 15, 
        mapTypeID: google.maps.MapTypeId.ROADMAP
    };

    var map = new google.maps.Map(document.getElementById("GoogleMap"), mapSettings)

    var marker = new google.maps.Marker({
        position: Hofstra
    });

    marker.setMap(map);

    var directionsService = new google.maps.DirectionsService();

    var directionsRenderer = new google.maps.DirectionsRenderer();

    directionsRenderer.setMap(map);

    function CalculateRoute(){
        
        var OriginValue = document.getElementById("Origin").value;
        var DestinationValue = document.getElementById("Destination").value;

        var request = {
            origin: OriginValue,
            destination: DestinationValue,
            travelMode: google.maps.TravelMode.DRIVING,
            unitSystem: google.maps.UnitSystem.IMPERIAL
        }
     
        directionsService.route(request, function(result, status) {
            if(status == google.maps.DirectionsStatus.OK){
                const outputText = document.getElementById('printResults');
                outputText.innerHTML = "Going from " + OriginValue + " to " + DestinationValue + 
                " would take " + result.routes[0].legs[0].duration.text + " and is " + result.routes[0].legs[0].distance.text + " apart.";
        
                directionsRenderer.setDirections(result);    
            }
            else {
                document.getElementById('printResults').innerHTML = "There was an error processing the route."
            }
        });
    }

</script>

<div id = "Menu">
    <span id = "inputText1">Origin: </span> 
    <input type = "text" id = "Origin" size ="20">
    <span id = "inputText2">Destination: </span> 
    <input type = "text" id = "Destination" size ="20">
    <button id = "CalculateButton" on:click={CalculateRoute} on:click={addToList}> Calculate </button>
    <button id = "DeleteButton" on:click={deleteFromList}> Delete Oldest Entry </button>
</div>
<br>
<br> 
<p id = "printResults"> </p>
<hr>
<h2 id = "history"> Previous Entries </h2>
<div id = "FromAndTo">
    <h3 id = "PrevFrom">From</h3>
    <h3 id = "PrevTo">To</h3>
</div>
<previous_routes_List>
    {#each Previous_Routes_List as Route}
        <routes id = "listEntry">
            <div id = "rows">
                <input type = "text" id = "previousOrigin" bind:value = {Route.from}/>
                <p id = "to"> to </p>
                <input type = "text" id = "previousDest" bind:value = {Route.to}/>
            </div>
        </routes>   
    {/each}
</previous_routes_List>