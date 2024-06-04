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

Right-click on the data> Store as a global variable. Use command copy to copy data in the clipboard. 

```
copy(temp1)
```
Paste it in notepad and save. And, now you have your entire Swiggy history.

### 2. JSON Parsing with Python

``` Python
import json
import csv

# Load the JSON data
data = json.load(open('data_m.json'))

# Create a dictionary to store the data for all orders
orders_dict = {}

# Loop through each order in the data
for order in data:
    # Extract the necessary information for each order
    order_id = order['order_id']
    order_time = order['order_time']
    total_tax_including_platform_fee = order['total_tax_including_platform_fee']
    packing_charges = order['charges']['Packing Charges']
    delivery_address = order['delivery_address']
    address = delivery_address['address']
    lat = delivery_address['lat']
    lng = delivery_address['lng']
    order_items = order['order_items']
    amount = order['order_total']
    restaurant_id = order['restaurant_id']
    restaurant_lat_lng = order['restaurant_lat_lng']
    order_status = order['order_status']
    restaurant_name = order['restaurant_name']

    # Create a list to store the item names for each order
    item_names = []

    # Extract item names for each order
    for item in order_items:
        item_name = item['name']
        item_names.append(item_name)

    # Append item names to the dictionary based on order_id
    if order_id in orders_dict:
        orders_dict[order_id]['Item Names'].extend(item_names)
    else:
        orders_dict[order_id] = {
            'Order Time': order_time,
            'Total Tax Including Platform Fee': total_tax_including_platform_fee,
            'Packing Charges': packing_charges,
            'Delivery Address': address,
            'Latitude': lat,
            'Longitude': lng,
            'Amount': amount,
            'Restaurant ID': restaurant_id,
            'Restaurant Latitude Longitude': restaurant_lat_lng,
            'Order Status': order_status,
            'Restaurant Name': restaurant_name,
            'Item Names': item_names
        }

# Write the data to a CSV file
with open('output_items_combined_with_attributes.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(['Order ID', 'Order Time', 'Total Tax Including Platform Fee', 'Packing Charges', 'Delivery Address', 'Latitude', 'Longitude', 'Amount', 'Restaurant ID', 'Restaurant Latitude Longitude', 'Order Status', 'Restaurant Name', 'Combined Item Names'])

    # Write the combined item names and additional attributes for each order
    for order_id, order_data in orders_dict.items():
        writer.writerow([order_id, order_data['Order Time'], order_data['Total Tax Including Platform Fee'], order_data['Packing Charges'], order_data['Delivery Address'], order_data['Latitude'], order_data['Longitude'], order_data['Amount'], order_data['Restaurant ID'], order_data['Restaurant Latitude Longitude'], order_data['Order Status'], order_data['Restaurant Name'], ', '.join(order_data['Item Names'])])
```
### 3. Using GPT-3 for Assistance
GPT-3 was instrumental in generating code snippets and providing guidance throughout the project.

### 4. Creating the Tableau Dashboard
With the cleaned and parsed data, I created a Tableau dashboard to visualize the key insights. The dashboard includes charts for:

Top 10 restaurants by order count
Average amount spent
Order distribution by time of day

# Conclusion
The project demonstrates the end-to-end process of extracting, parsing, and visualizing data. It highlights how tools like GPT-3 can assist in coding and how learning new skills (JavaScript) can enhance data projects.
