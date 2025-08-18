
**Step 1: Open Cost Explorer**

1. Sign in to the AWS Console
    
2. Navigate to **Billing** from the top search bar
    
3. In the left menu, click **Cost Explorer**
    
4. Click **Launch Cost Explorer**
    

**Step 2: Set Time Period**

1. In the Cost Explorer screen, locate the **Time range** selector at the top
    
2. Choose a preset (Last 7 days, This month, etc.) or set a custom date range
    

**Step 3: Filter to EC2 Service**

1. Click **Filters** on the left panel
    
2. Under **Service**, select:
    
    - **Amazon Elastic Compute Cloud – Compute**
        
    - _(Optional)_ Include **Amazon Elastic Block Store (EBS)** if you want to track EC2-related storage costs
        

**Step 4: Group by Instance Type**

1. At the top of the chart, find the **Group by** dropdown
    
2. Select **Instance Type**
    
    - This will show your EC2 costs broken down by instance type (e.g., t3.micro, m5.large)
        

**Step 5: Choose Cost Type (Optional)**

- On the top-right, choose between:
    
    - **Unblended cost** (default): Raw costs
        
    - **Amortized cost**: Includes RI/Savings Plan allocations
        

**Example Output:**

|Instance Type|Monthly Cost|
|---|---|
|t3.micro|$15.80|
|m5.large|$120.35|
|c6a.large|$78.90|

You can hover over or click on each type to view trends over time.

**Optional: Export or Tag Breakdown**

- Enable and use **cost allocation tags** to track EC2 costs by:
    
    - Instance name
        
    - Environment (e.g., Dev/Prod)
        
    - Team or project
        

**Summary Table:**

|Step|Action|
|---|---|
|1|Open Cost Explorer from Billing Dashboard|
|2|Set your desired time period|
|3|Filter by Amazon EC2 – Compute|
|4|Group by Instance Type|
|5|View and analyze cost by EC2 instance type|

---
