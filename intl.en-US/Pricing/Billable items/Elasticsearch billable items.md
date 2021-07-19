# Elasticsearch billable items

You are charged for your Alibaba Cloud Elasticsearch clusters based on cluster specifications and storage space per node.

## Billable items and billing formulas

|Billable item|Billing formula|
|-------------|---------------|
|Cluster specifications**Note:** Cluster specifications refer to the specifications that you configure based on your business requirements.

|-   Data nodes, Kibana nodes, dedicated master nodes, warm nodes, and client nodes support both the subscription and pay-as-you-go billing methods. The following formulas are used to calculate the fees of these nodes:
    -   Subscription: Fees = Price of the specifications of a single node × Number of nodes × Subscription duration
    -   Pay-as-you-go: Fees = Price of the specifications of a single node × Number of nodes × Usage duration
-   Elastic nodes support only the pay-as-you-go billing method, regardless of the billing method of your cluster. The following formula is used to calculate the fees of elastic nodes:

Pay-as-you-go: Fees = Storage space per node × Number of nodes × Unit price of cloud disks × Usage duration |
|Storage space|-   Subscription: Fees = Storage space per node × Number of nodes × Unit price of cloud disks × Subscription duration
-   Pay-as-you-go: Fees = Storage space per node × Number of nodes × Unit price of cloud disks × Usage duration |

