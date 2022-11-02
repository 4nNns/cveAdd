## Huaxia ERP Unauthorized Access Vulnerability

By default, Huaxia ERP integrates users, permissions, menus, routes, classifications, labels, logs, fields, forms, forms, uploads, APIs and other basic functional modules. Huaxia ERP was born for the agile development of WEB applications and the simplification of enterprise application development. The framework, modules, plug-ins and templates can be upgraded and expanded independently.
The background of Huaxia ERP can access the system information by accessing the absolute path in the retail management retail shipment.
You can access the vulnerability path through an unlisted browser to obtain system information.
Vulnerability address: http://ip:8080/index.html#/pages/bill/retail_out_list.html
Code download address: https://gitee.com/jishenghua/JSH_ERP  
Vulnerability location: Obtain the module interface after logging in to the system. After obtaining the interface, obtain the interface before sending a GET request through packet capture. Use login.html/../depotHead/list to obtain the information on this page without cookies.

### Exploit

Click Retail Management - Retail Delivery to obtain the interface information through packet capture.

<img src="./pic/4.jpg" alt="1" style="zoom:200%;" />

<img src="./pic/5.jpg" alt="1" style="zoom:200%;" />

You can directly read data information by accessing this path through an unlisted browser.

<img src="./pic/6.jpg" alt="1" style="zoom:200%;" />
