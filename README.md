# HFNZ-Database
Database using Microsoft Access for a food production facility.

# Brief Note
I am currently a second year student at University of Waikato studying data analytics.
By the end of the previous semester which ended in November 2020, I have studied only one freshman-level programming course (C#) and have not learned about database yet. 
Before launching the project, I had spent some time studying relational database and some basic vba programming within Microsoft Access. 
So literally, this is my first ever projecct that I completely did on my own. Any comment or advice is definitely welcomed and thanks in advance.

I would like to thank Claudia Wu and the owners of the business, Terry Robinson and Anthony, for the trust and for giving me the opportunity to attempt the project. It has been a wonderful and fruitful experience from which I have gained a lot.

# Background
The food production facility desires to have a system which can be used to track the supply,the production and the sales when a recall is needed. Below is the requirements that I got from them:

There are 2 kinds of recall

    Trade level – where food distributed to stores is recalled
    Consumer level – where there is public notification of the recall.

The 2 main reasons for recall

    Something has gone wrong in your business
    Something has gone wrong in a suppliers business and you have used an ingredient.

A recall is needed if there is any doubt that the food is safe and some of it has  been sold or distributed.
The requirement under the food act is that in the event of a health alert/breakdown we are able to fully trace both sales and supply.

    Supply  - We must be able to identify and trace where each ingredient came from and be quickly able to identify that supplier.
    Sales  -  We must be able to trace where our product has been distributed to with bulk/wholesale sales, as well as promotions and samples.  Tracing of individual consumer sales is not required.
Tracing needs to be to batch level and will usually be ringfenced by the date manufactured however this may be further batch separated if any of the ingredients (or process) were different. (brand, packet, operator).

# Design
Claudia Wu, the upperclasswoman who introduced me to the project, suggested me to use Microsoft Access because the user interfaces will allow the users to manage the database through forms and reports. 

Based on the requirements provided by the owner, I have built the following tables in the database: 
  1. tblProducts: ProductID; ProductName
  2. tblRawMaterials: RawMaterialID; RawMaterialName
  3. tblSuppliers: SupplierID; SupplierName; Address; Contact; Email; PhoneNumber
  4. tblPurchasingOrders(a table for the supply orders the facility placed): PurchasingOrderID; SupplierID; RawMaterialID; Quantity; OrderDate
  5. tblBatches: BatchNumber; ProductID; Quantity; ProductionDate
  6. tblIngredients(a table that records which purchasing orders the raw materials used to produce each batch come from): BatchNumber; PurchasingOrderId; ProductionDate; Quantity
  7. tblCustomers: CustomerID; CustomerName; Contact; Address; Email; PhoneNumber
  8. tblOrders: OrderID; CustomerID; OrderDate; Comments; StatusID
  9. tblOrderItems (a table that records what are contained in each sales order): OrderItemID; OrderID; BatchNumber; Quantity
  10.tblStatus: StatusID; Description
  11.tblEmployees: EmployeeID; Name; PositionID; Username; Password
  12.tblPositions: PositionID; Postion
  13.tblTemp1&tblTemp2: temporary table that are used when generating reports for selected time intervals
  The joint relationships among the tables can be viewed in the application, under the database tab. 
  
In addition to the tables, I have made forms so that the user of the application can easily manipulate the database. The following describes the forms and the functions: 
 
 1.	Supply Management
 
    •	Supplier Management: 
    
        a)	View the supplier info after searching by either supplier ID (currently 1-3 for testing) or supplier name.
        b)	After search, the user can update the existing supplier.
        c)	With a click on the “Add” button, the user can add a new supplier to the database.
        
    • Purchasing Order Management: 
    
        a)	Filter the purchasing order based on selected criteria.
        b)	With a click on the displayed “Purchasing Order ID”, the user can update/delete the purchasing order. 
        c)	With a click on the “Add” button, the user can add a new purchasing order to the database.
    •	Brief inventory record of each raw material displayed on the footer of the form.

2.	Production Management

    •	Production Management
    
        a)	view all or filtered batches that have been produced.
        b)	With a click on the add button, the user can add a new batch to the database. After saving the new batch, the user is required to add ingredients (raw materials with purchasing order ID) to the batch. 
        c)	With a click on the batch number, the user can view the information about the raw materials used to produce this batch.
        d)	With a click on the “Add Ingredient” button, the user can add ingredients that was used to produce the selected batch.
        e)	With a click on the batch number displayed in the subform, the user can update or delete the selected ingredient.
        f)	With a click on the “Print” button, the user can either print or save a pdf file for the selected batch.
    •	Production Reporting
    
        a)	The user can view a clustered bar graph of the sales after selecting the time interval and how the graph is displayed (by month, by quarter and by year).
        b)	When the graph has been generated, the right-hand side part of the form will display a total count of sales of each product within selected time interval.
        c)	After the graph has been generated, the user can re-choose the group-by method, like switching from Monthly to Quarterly.

3.	Sales Management

      • Customer Management: 
      
        a)	View the customer info after searching by either customer ID (currently 1-3 for testing) or customer name.
        b)	After search, the user can edit or delete the displayed customer.
        c)	With a click on the “Add” button, the user can add a new customer to the database. 
        
      •	Sales Management: 
      
        a)	view all or filtered orders that have been made by customers.
        b)	With a click on the “Add” button, the suer can add a new order to the database. After saving the new order, the user is required to add items (Products with batch number and with quantity) to the order.
        c)	With a click on the order ID, the user can view detailed information about this order, including order items and their associated batch. 
        d)	With a click on the “Add an Order Item” button, the user can add items to the selected order.
        e)	With a click on the order item id in the subform, the user can update or delete the selected order item. 
        f)	With a click on the “Order Summary” button, the user can view a detailed report for the selected order.
        
      •	Sales Reporting
      
        a)	The user can view a cluster bar graph of the sales after selecting the time interval and how the graph is displayed (by month, by quarter and by year).
        b)	When the graph has been generated, the right-hand side part of the form will display a total count of sales of each product within selected time interval.
        c)	After the graph has been generated, the user can re-choose the group-by method, like switching from Monthly to Quarterly.

4.	Tracing

      •	Tracing from production end
      
        a)	When a batch number is selected, the application will pull out all sales orders with customer names that contain products produced in this batch.
        b)	After tracing, the “Notify Customers” button will be visible. With a click on it, the system will automatically open an Outlook email in which the filtered customers will be the recipients. The user then can edit the email body before sending the recall information to the customers.
        
      •	Tracing from supply end
      
        a)	When a purchasing order is selected, the application will pull out all sales orders with customer names that include products which were produced with the materials supplied in the selected purchasing order.
        b)	After tracing, the “Notify Customers” button will be visible. With a click on it, the system will automatically open an Outlook email in which the filtered customers will be the recipients. The user then can edit the email body before sending the recall information to the customers.

  

