# Swiggy-Order-History-Analysis
Analyzed My Swiggy Order History 🍔 &amp; found some tasty insights. nom nom. 😋

## Technologies Used 

<img src="https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/fe4f8e1b-5478-4abd-8ccf-1240cb71223e" alt=" " width="5%">
<img src="https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/e43e2c7b-2140-408a-9e18-f399568ccd10" alt=" " width="6%">
<img src="https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/54a3233a-2374-43cb-b842-490bfce652f8" alt=" " width="15%">
<img src="https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/6e6014fa-ea15-4fbc-99dd-c8e8bd27d618" alt=" " width="5%">
<img src="https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/72a1888e-f29b-4318-804c-4eb6bd87f639" alt=" " width="6%">

## Final Dashboard 
![Dashboard 1 (1)](https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/aba496c1-af0b-44ca-869f-93b46f917de3)


## Problem Statement 
The main objectives of this project are:
- To extract and analyze historical food order data from https://www.swiggy.com/
- To visualize this data in a Tableau Dashboard.

## Solution Approach

### 1. Data Extraction

I extracted the data from a website using a JavaScript snippet. This process involved learning JavaScript basics and learning about JSON files, parsing data.

On https://www.swiggy.com/my-account page open chrome Dev Tools and look at some network requests and order_id is the header. 
This simple JS snippet was able to fetch order data using session cookies.
```javascript
var xmlHttp = new XMLHttpRequest();
xmlHttp.open( “GET”, “https://www.swiggy.com/mapi/order/all?order_id=", false );
xmlHttp.send( null );
console.log(xmlHttp.responseText);
```

This is the order_id response: 
![Screenshot 2024-05-27 110541](https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/dd778a8e-fe7e-4b1a-9548-bc2a16f64eea)


Now to aggregate all this data: 
```javascript
order_array=[]
order_id=’’
page = 1
try {
    while(true){
        var xmlHttp = new XMLHttpRequest()
        xmlHttp.open( “GET”, “https://www.swiggy.com/dapi/order/all?order_id="+order_id, false )
        xmlHttp.send( null )
        resText=xmlHttp.responseText
        var resJSON = JSON.parse(resText)
        order_id=resJSON.data.orders[resJSON.data.orders.length-1].order_id
        order_array=order_array.concat(resJSON.data.orders)
        console.log(“On page: “+page+” with last order id: “+order_id)
        page++
    }
}
catch(err) {
    console.log(order_array)
}
```
![Screenshot 2024-05-27 111106](https://github.com/guruuvai/Swiggy-Order-History-Analysis/assets/67874401/b133e712-27bc-4b07-abb4-8db9ea750e68)


