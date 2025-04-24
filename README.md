# Microsoft Fabric Lakehouse
I recently got hands-on with Microsoft Fabric and created a Lakehouse—essentially a combination of a data lake and a data warehouse. In a Fabric Lakehouse, files and tables are stored in OneLake, and you can query managed tables using SQL. What’s cool is that those tables automatically feed into a semantic model for Power BI reporting. Here's what I did step by step:

# Step 1: Set Up the Workspace 

Before working with data in Fabric, create a workspace with the Fabric trial enabled.

I started by signing into Microsoft Fabric. From there, I created a new workspace, making sure to choose a licensing mode that supported the Fabric trial. Once created, the workspace was empty and ready for action.

![Image](https://github.com/user-attachments/assets/823415bb-b675-4528-9c98-81193c325d3f)

## Step 2: Create the Lakehouse 

Now that you have a workspace, it's time to create a data lakehouse for your data files.

Inside the workspace, I went to Create > Lakehouse (under Data Engineering) and gave it a unique name. After a minute or so, my new lakehouse was ready. On the left pane, I could see two main sections—Tables (for SQL-queryable Delta Lake tables) and Files (for raw data and shortcuts to external data sources).

 ![Image](https://github.com/user-attachments/assets/14e20966-5605-4ee2-b298-3a7f5d87cb94)

Currently, there are no tables or files in the lakehouse.
## Step 3: Upload a File

To get some data in there, I downloaded a sample file called sales.csv from this GitHub link. Then, in my lakehouse, I created a subfolder under Files named data, and uploaded the CSV into it. It showed up right away and I was able to preview the contents.

![Image](https://github.com/user-attachments/assets/12dcd09c-82c5-4737-b594-8abaf693028b)


## Step 4: Peek at Shortcuts
Just for exploration, I checked out the New shortcut option under the Files menu. It lets you reference external data sources directly, without needing to copy them into your lakehouse. I didn’t create one, but it’s a useful feature for the future.

## Step 5: Load Data into a Table
Next, I converted the CSV into a managed table. From the sales.csv file menu, I selected Load to Table > New Table, named it sales, and confirmed. Once loaded, it appeared in the Tables section. I even viewed the underlying Parquet files and saw the _delta_log folder used for Delta Lake transactions.

![Image](https://github.com/user-attachments/assets/eeceb923-2721-4ad8-9f0e-63a32083421d)

In the Lakehouse explorer pane, select the sales table that has been created to view the data.

![Image](https://github.com/user-attachments/assets/b7eb73ff-cd4a-4d71-b71c-caa10b0fedde)

In the "..." menu for the sales table, select View files to see the underlying files for this table

![Image](https://github.com/user-attachments/assets/8583cdda-9534-43e8-8f74-8cc8eceed32a)

## Step 6: Use SQL to query tables

When you create a lakehouse and define tables in it, a SQL endpoint is automatically created through which the tables can be queried using SQL `SELECT` statements.

Fabric automatically sets up a SQL analytics endpoint for every lakehouse, so I switched to that view and ran this query to calculate total revenue by item:
SELECT Item, SUM(Quantity * UnitPrice) AS Revenue
FROM sales
GROUP BY Item
ORDER BY Revenue DESC;
The results came up in seconds, showing the revenue breakdown clearly.

![Image](https://github.com/user-attachments/assets/3a26ba99-7e6f-4c02-890a-e1ccc54844f1)

## Step 7: Try a Visual Query
Just for fun, I tried the visual query editor too—very Power BI-like. I dragged the sales table into the pane, selected just the SalesOrderNumber and SalesOrderLineNumber columns, and grouped by SalesOrderNumber to count distinct line items per order. Super intuitive.

![Image](https://github.com/user-attachments/assets/ad4ef541-57a0-47bb-b04c-ec009437b1b3)


## Step 8: Build a Quick Report 

Fabric also builds a default semantic model from the lakehouse tables, which makes reporting easy. I jumped into the Reporting tab, created a new report, and visualized Item-wise quantity using a clustered bar chart. After saving it as Item Sales Report, it appeared alongside the lakehouse, SQL endpoint, and semantic model in my workspace.

![Image](https://github.com/user-attachments/assets/2e6f4a21-4618-477d-bb8c-115e2bfdb120)



## Step 9: Clean Up
After exploring, I went back to the workspace settings and deleted the workspace to keep things tidy.

Overall, the experience was smooth. Microsoft Fabric feels like a powerful blend of SQL + big data + Power BI, all in one. Definitely a good tool for modern data analytics.

