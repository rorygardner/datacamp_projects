![Parked motorcycle](motorcycle.jpg)

You're working for a company that sells motorcycle parts, and they've asked for some help in analyzing their sales data!

They operate three warehouses in the area, selling both retail and wholesale. They offer a variety of parts and accept credit cards, cash, and bank transfer as payment methods. However, each payment type incurs a different fee.

The board of directors wants to gain a better understanding of wholesale revenue by product line, and how this varies month-to-month and across warehouses. You have been tasked with calculating net revenue for each product line and grouping results by month and warehouse. The results should be filtered so that only `"Wholesale"` orders are included.

They have provided you with access to their database, which contains the following table called `sales`:

## Sales
| Column | Data type | Description |
|--------|-----------|-------------|
| `order_number` | `VARCHAR` | Unique order number. |
| `date` | `DATE` | Date of the order, from June to August 2021. |
| `warehouse` | `VARCHAR` | The warehouse that the order was made from&mdash; `North`, `Central`, or `West`. |
| `client_type` | `VARCHAR` | Whether the order was `Retail` or `Wholesale`. |
| `product_line` | `VARCHAR` | Type of product ordered. |
| `quantity` | `INT` | Number of products ordered. | 
| `unit_price` | `FLOAT` | Price per product (dollars). |
| `total` | `FLOAT` | Total price of the order (dollars). |
| `payment` | `VARCHAR` | Payment method&mdash;`Credit card`, `Transfer`, or `Cash`. |
| `payment_fee` | `FLOAT` | Percentage of `total` charged as a result of the `payment` method. |


Your query output should be presented in the following format:

| `product_line` | `month` | `warehouse` |	`net_revenue` |
|----------------|-----------|----------------------------|--------------|
| product_one | --- | --- | --- |
| product_one | --- | --- | --- |
| product_one | --- | --- | --- |
| product_one | --- | --- | --- |
| product_one | --- | --- | --- |
| product_one | --- | --- | --- |
| product_two | --- | --- | --- |
| ... | ... | ... | ... |


```python
SELECT product_line,
	CASE EXTRACT (MONTH FROM date)
	WHEN 6 THEN 'June'
	WHEN 7 THEN 'July'
	WHEN 8 THEN 'August'
	ELSE 'other'
	END AS month,
	warehouse,
	(SUM(total)-SUM(payment_fee)) AS net_revenue
FROM sales 
WHERE client_type='Wholesale'
GROUP BY product_line, month, warehouse
ORDER BY product_line, month, net_revenue DESC;

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>product_line</th>
      <th>month</th>
      <th>warehouse</th>
      <th>net_revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Braking system</td>
      <td>August</td>
      <td>Central</td>
      <td>3039.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Braking system</td>
      <td>August</td>
      <td>West</td>
      <td>2500.67</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Braking system</td>
      <td>August</td>
      <td>North</td>
      <td>1770.84</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Braking system</td>
      <td>July</td>
      <td>Central</td>
      <td>3778.65</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Braking system</td>
      <td>July</td>
      <td>West</td>
      <td>3060.93</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Braking system</td>
      <td>July</td>
      <td>North</td>
      <td>2594.44</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Braking system</td>
      <td>June</td>
      <td>Central</td>
      <td>3684.89</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Braking system</td>
      <td>June</td>
      <td>North</td>
      <td>1487.77</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Braking system</td>
      <td>June</td>
      <td>West</td>
      <td>1212.75</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Electrical system</td>
      <td>August</td>
      <td>North</td>
      <td>4721.12</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Electrical system</td>
      <td>August</td>
      <td>Central</td>
      <td>3126.43</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Electrical system</td>
      <td>August</td>
      <td>West</td>
      <td>1241.84</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Electrical system</td>
      <td>July</td>
      <td>Central</td>
      <td>5577.62</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Electrical system</td>
      <td>July</td>
      <td>North</td>
      <td>1710.13</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Electrical system</td>
      <td>July</td>
      <td>West</td>
      <td>449.46</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Electrical system</td>
      <td>June</td>
      <td>Central</td>
      <td>2904.93</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Electrical system</td>
      <td>June</td>
      <td>North</td>
      <td>2022.50</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Engine</td>
      <td>August</td>
      <td>Central</td>
      <td>9528.71</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Engine</td>
      <td>August</td>
      <td>North</td>
      <td>2324.19</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Engine</td>
      <td>July</td>
      <td>Central</td>
      <td>1827.03</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Engine</td>
      <td>July</td>
      <td>North</td>
      <td>1007.14</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Engine</td>
      <td>June</td>
      <td>Central</td>
      <td>6548.85</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Frame &amp; body</td>
      <td>August</td>
      <td>Central</td>
      <td>8657.99</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Frame &amp; body</td>
      <td>August</td>
      <td>North</td>
      <td>7898.89</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Frame &amp; body</td>
      <td>August</td>
      <td>West</td>
      <td>829.69</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Frame &amp; body</td>
      <td>July</td>
      <td>North</td>
      <td>6154.61</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Frame &amp; body</td>
      <td>July</td>
      <td>Central</td>
      <td>3135.13</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Frame &amp; body</td>
      <td>June</td>
      <td>Central</td>
      <td>5111.34</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Frame &amp; body</td>
      <td>June</td>
      <td>North</td>
      <td>4910.12</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Frame &amp; body</td>
      <td>June</td>
      <td>West</td>
      <td>2779.74</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Miscellaneous</td>
      <td>August</td>
      <td>North</td>
      <td>1841.40</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Miscellaneous</td>
      <td>August</td>
      <td>Central</td>
      <td>1739.76</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Miscellaneous</td>
      <td>August</td>
      <td>West</td>
      <td>813.43</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Miscellaneous</td>
      <td>July</td>
      <td>Central</td>
      <td>3118.44</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Miscellaneous</td>
      <td>July</td>
      <td>North</td>
      <td>2404.65</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Miscellaneous</td>
      <td>July</td>
      <td>West</td>
      <td>1156.80</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Miscellaneous</td>
      <td>June</td>
      <td>West</td>
      <td>2280.97</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Miscellaneous</td>
      <td>June</td>
      <td>Central</td>
      <td>1878.07</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Miscellaneous</td>
      <td>June</td>
      <td>North</td>
      <td>513.99</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Suspension &amp; traction</td>
      <td>August</td>
      <td>Central</td>
      <td>5416.70</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Suspension &amp; traction</td>
      <td>August</td>
      <td>North</td>
      <td>4923.69</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Suspension &amp; traction</td>
      <td>August</td>
      <td>West</td>
      <td>1080.79</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Suspension &amp; traction</td>
      <td>July</td>
      <td>Central</td>
      <td>6456.72</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Suspension &amp; traction</td>
      <td>July</td>
      <td>North</td>
      <td>3714.28</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Suspension &amp; traction</td>
      <td>July</td>
      <td>West</td>
      <td>2939.32</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Suspension &amp; traction</td>
      <td>June</td>
      <td>North</td>
      <td>8065.74</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Suspension &amp; traction</td>
      <td>June</td>
      <td>Central</td>
      <td>3325.00</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Suspension &amp; traction</td>
      <td>June</td>
      <td>West</td>
      <td>2372.52</td>
    </tr>
  </tbody>
</table>
</div>


