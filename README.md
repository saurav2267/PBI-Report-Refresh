# Power BI Report Refresh Alternative

## Overview
When working with very large datasets in Power BI, there is an alternative method to implement partial table refreshes without using incremental refresh. This method is especially effective and results in a shorter refresh time compared to incremental refresh with a Datestamp column, particularly when the static data volume is significantly higher than the incremental values. This is a common scenario in financial planning and forecasting, where data is frozen in each round and current metrics are compared with static rounds.

## Files Used
This sample utilizes the following files:

1.  Static Data: Old versions that do not change.
2.  Live Data: The latest version that updates on an hourly basis.
3.  Power BI Report: Contains the above two Excel files and creates appropriate relationships.

Note:
Static Data and Live data can be any type of source, it can be a Datamart/Sharepoint Lists/Excels or CSV/ or any database.In the Power Query editor, ensure that you uncheck the "Include in Report Refresh" option for the static data table. This prevents the static data table from running every time the refresh is triggered.![image](https://github.com/saurav2267/PBI-Report-Refresh/assets/55321375/f11024c6-e1a9-407b-9e50-788beb6dfdd5)

We create a DAX Calculated Table which is an union of the tables : UNION ('Live Data','Static Data').Sometimes, the order of the columns might not be in the intended order, which can mess up the schema. To overcome this issue, use SELECTCOLUMNS to adjust the column order as shown in the final code snippet above.
Finally to achieve the desired scenario. The code is as follows:

Calculated Table = 
UNION(
    SELECTCOLUMNS('Live Data',
        "ID", 'Live Data'[ID],
        "Name", 'Live Data'[Name],
        "City", 'Live Data'[City],
        "Mobile Number", 'Live Data'[Mobile Number]
    ),
    SELECTCOLUMNS('Static Data',
        "ID", 'Static Data'[ID],
        "Name", 'Static Data'[Name],
        "City", 'Static Data'[City],
        "Mobile Number", 'Static Data'[Mobile Number]
    )
)


## Outputs:

### Live Data:
![image](https://github.com/saurav2267/PBI-Report-Refresh/assets/55321375/a3fbb7ca-83ff-489c-8426-e34147a2b897)

### Static Data:
![image](https://github.com/saurav2267/PBI-Report-Refresh/assets/55321375/feaac42a-a5c5-4c20-93c3-418bae8ab592)

### Final Table Data:
![image](https://github.com/saurav2267/PBI-Report-Refresh/assets/55321375/c86d3ff6-795d-491e-a89a-5be21c05f48d)



