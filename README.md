![1739893667251](https://github.com/user-attachments/assets/31678ba1-a5ff-4c4d-8cf8-265ee2f5f4dc)

![image](https://github.com/user-attachments/assets/84adad3d-d61d-479e-a554-c3793d302287)  

<h1 align="center">
  <font color="#4682B4">GEOL0069: Artificial Intelligence For Earth Observation (AI4EO) 24/25</font>
</h1>

<h2 align="center">
  <font color="#6082B6">Week 4: Colocation and Unsupervised Classification</font>
</h2>

This repository showcases my learning experience in working with collocated satellite data and performing unsupervised classification. Through this project, I gained experience in:

*   Finding and accessing collocated satellite imagery (Sentinel-3 and Sentinel-2).
*   Handling and processing altimetry data.
*   Implementing and applying unsupervised classification algorithms.

**Learnings:**

*   Understanding the challenges of working with different satellite data resolutions.
*   The importance of data preprocessing for optimal classification results.
*   The practical application of unsupervised learning techniques in remote sensing.


# Colocating Satellite Data and Applying Unsupervised Learning

This repository contains code and results for a project that involves colocating Sentinel-3 OLCI and Sentinel-2 optical data, and then applying unsupervised learning techniques.

## Part 1: Data Acquisition and Colocation

This part of the project focuses on acquiring and preparing the necessary satellite data for analysis.  The goal is to combine the high spatial resolution of Sentinel-2 with the spectral coverage and altimetry data of Sentinel-3.

**Steps:**

1.  **Setup and Function Loading:**
    *   Import necessary functions for data retrieval, format conversion, and preprocessing.
    *   Ensure proper environment setup and authentication.

2.  **Identify Collocated Sentinel-2 and Sentinel-3 Pairs:**
    *   Query Google Earth Engine to find matching Sentinel-2 and Sentinel-3 filenames based on:
        *   Area of Interest (AOI)
        *   Time Frame
        *   Other relevant parameters (cloud cover, etc.)
    *   From the list of matches, select a single pair for download.
![image](https://github.com/user-attachments/assets/5114dc8e-c2a7-4a3c-9496-878cfbed3d4f)
![image](https://github.com/user-attachments/assets/4b1f42d1-7ee7-47f8-990d-a96a127462c0)

3.  **Download Sentinel-3 OLCI Data:**
    *   Convert filenames to the appropriate format.
    *   Retrieve the data from the Copernicus dataspace.

4.  **Download Colocated Sentinel-3 OLCI Altimetry Data:**
    *   Along with the OLCI optical data, retrieve the corresponding altimetry measurements from Sentinel-3.
  
![image](https://github.com/user-attachments/assets/727dd04f-8784-4320-a15f-2a6dcd7f5910)


## Part 2: Unsupervised Learning for Sea Ice and Lead Discrimination

This section details the application of unsupervised learning algorithms to the collocated satellite data acquired in Part 1.  The primary goal is to discriminate between sea ice and leads (open water areas within sea ice) using both optical (Sentinel-2) and altimetry (Sentinel-3) data.

**Introduction to Unsupervised Learning**

We focus on practical applications of unsupervised learning for classification, specifically clustering.  These methods are crucial for identifying patterns and structures within data *without* predefined labels.  We'll utilize two primary algorithms:

*   **K-means Clustering:** A simple and efficient algorithm that partitions data into *k* clusters based on minimizing the distance between data points and their assigned cluster centroid.
*   **Gaussian Mixture Models (GMM):** A probabilistic model that assumes data is generated from a mixture of Gaussian distributions.  GMMs provide "soft" clustering (probabilities of belonging to each cluster) and can model clusters with varying shapes and sizes.

**Steps:**

1.  **Data Preparation (Preprocessing):**

    *   **For Image Classification (Sentinel-2):**
        *   Prepare the Sentinel-2 imagery for clustering. This likely involves:
            *   Selecting relevant spectral bands.
            *   Masking clouds and other non-ice/water areas.
            *   Potentially performing dimensionality reduction (e.g., PCA).
        * No data loading is needed as we have collocated data

    *   **For Altimetry Classification (Sentinel-3):**
        *   Load necessary functions for altimetry data processing.
        *   Transform raw Sentinel-3 altimetry data into derived variables suitable for classification, such as:
            *   Peakiness
            *   Stack Standard Deviation (SSD)
            *   Other relevant altimetry parameters.

2.  **Algorithm Implementation:**

    *   **K-means:**
        *   Implement the K-means algorithm (or use a library like scikit-learn).
        *   Choose an appropriate value for *k* (number of clusters). The elbow method or silhouette analysis can help.
        *   Initialize centroids randomly or using a more informed method (e.g., k-means++).
        *   Iteratively:
            *   Assign each data point to the nearest centroid.
            *   Update centroid positions based on the mean of the assigned points.
            *   Repeat until convergence (centroids no longer move significantly).
<center>
  <img src="https://github.com/user-attachments/assets/36f3fb5e-d74d-4749-bac1-0ac412c85459" alt="image">
</center>

    *   **Gaussian Mixture Models (GMM):**
        *   Implement the GMM algorithm (or use a library like scikit-learn).
        *   Choose the number of Gaussian components.
        *   Select a covariance type (e.g., 'full', 'tied', 'diag', 'spherical') based on the expected cluster shapes.
        *   Use the Expectation-Maximization (EM) algorithm to fit the GMM:
            *   **E-step:** Calculate the probability of each data point belonging to each Gaussian component.
            *   **M-step:** Update the parameters (mean, covariance, and mixing coefficients) of each Gaussian to maximize the data likelihood.
            *   Repeat until convergence.
<center>
  <img src="https://github.com/user-attachments/assets/58567681-6672-4152-a8cb-2913627acf4f" alt="image">
</center>

3.  **Application to Specific Datasets:**

    *   **Image Classification (Sentinel-2):**
        *   Apply both K-means and GMM to the prepared Sentinel-2 imagery.
        *   Visualize the resulting cluster assignments .
<center>
  <img src="https://github.com/user-attachments/assets/5bf47c1f-71e8-4e6e-9546-3e238853df70" alt="image">
</center>

<center>
  <img src="https://github.com/user-attachments/assets/0857a5e8-510c-4160-a491-a9fc6e235160" alt="image">
</center>


    *   **Altimetry Classification (Sentinel-3):**
        *   Apply both K-means and GMM to the derived altimetry variables.
        *   Visualize the clustering results .
        *   Plot the mean waveform for each identified cluster to understand the characteristics of each group.
![image](https://github.com/user-attachments/assets/fc4e2dbc-5603-4233-9ff8-63d7390e7d86)


4.  **Evaluation and Comparison (Crucial!):**

    *   **Visual Inspection:**  Examine the classification maps and scatter plots to assess the quality of the clustering.  Do the clusters make sense physically?
    *   **Comparison with ESA Data:**
        *   Compare the unsupervised classification results (for both Sentinel-2 and Sentinel-3) with the ESA's official sea ice/lead product.  Note: Adjust labels as needed (ESA: sea ice = 1, lead = 2; ).
![image](https://github.com/user-attachments/assets/51780381-c002-4b6e-9ba9-bc754f16a2cf)
![image](https://github.com/user-attachments/assets/95376b55-7b28-46f8-a8a6-7fe8f4a7719a)

