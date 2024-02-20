# Job Scheduling Overview

### **Accessing the Dashboard**

* **Location:** Find the Nosto plugin dashboard under the Marketing tab in Shopware 6 administration.
* **Path:** Marketing → Nosto Jobs Listing

<figure><img src="../.gitbook/assets/job-navigation.png" alt="" width="294"><figcaption></figcaption></figure>

### **Dashboard Features**

* **Job Scheduling Dashboard:** View and manage scheduled jobs.
* **Job Listing:** Access complete job information and sync products with Nosto.

<figure><img src="https://lh7-us.googleusercontent.com/ibKEVm2vXXU7TKl9FL3IyD2L-aTOFFs6DA_nN_usuwYUYxaySEkjTxTLpx4jpqAxrEEAoD0bN6S327NrKdfLmexWtiVWBZHhfbkWekAe_0KWP4ESvlqTDAZKGx0eU3oc9allpuJdVNStGP9IoQpDSJI" alt=""><figcaption></figcaption></figure>

### **Job Listing Details**

*   **Columns Explained:**

    * **Name:** Job identifier.
    * **Status:** Job status (Success, Failed, Running, Pending).
    * **Timings:** Job creation, start, and finish times.
    * **Child Jobs:** Status indicators (Green for success, Gray for pending, Red for failed).
    * **Messages:** Log messages categorized by type (Info, Warning, Error).

    ![](https://lh7-us.googleusercontent.com/ibKEVm2vXXU7TKl9FL3IyD2L-aTOFFs6DA\_nN\_usuwYUYxaySEkjTxTLpx4jpqAxrEEAoD0bN6S327NrKdfLmexWtiVWBZHhfbkWekAe\_0KWP4ESvlqTDAZKGx0eU3oc9allpuJdVNStGP9IoQpDSJI)

### **Dashboard View Modes**

* **List View:** Default view with filter support.
* **Grouped View:** Organize jobs by status or type.
* **Chart View:** Visualize job data over selected time frames (30 Days, 14 Days, etc.).

<figure><img src="../.gitbook/assets/job-scheduler-listing-view.png" alt=""><figcaption><p>List View</p></figcaption></figure>

<figure><img src="../.gitbook/assets/job-scheduler-grouped-view.gif" alt=""><figcaption><p>Grouped View</p></figcaption></figure>

<figure><img src="../.gitbook/assets/job-scheduler-chart.gif" alt=""><figcaption><p>Chart View</p></figcaption></figure>



{% hint style="info" %}
Enabling **Auto Load** will refresh the listed data automatically once per minute.

![](../.gitbook/assets/image.png)
{% endhint %}

### **Jobs Handled**

* Changelog Entity Sync Operation
  * Marketing Permission Sync Operation (newsletter),
  * Order Sync Operation (New Order, Updated Order events)
  * Product Sync Operation
* Full Catalog Sync Operation: Synchronize all products to Nosto
