<h1> Creating Azure Workspace </h1>

1. In the searchbar, search for "Azure Machine Learning" Service. It is one of the Azure services we are going to use to create our workspace to build the model
2. Click on "Create"
3. Select the subscription type
4. Create New Resource Group (eg. education) - This holds related resources for an Azure solution
5. Give a Workspace Name
6. Region - select accordingly as some services are not available in some regions
7. Container Registry - None
8. Next
9. Networking - Should be Public Endpoint. This is so that you can access this workspace from anywhere by simply logging into your account
10. Next
11. Credential Based access (Login using credentials)
12. Microsoft managed keys
13. Next > Review + Create
14. Deploy! (might take a few good minutes)
15. Go & Launch Studio

    <img width="1440" alt="Screenshot 2024-06-21 at 8 31 38 AM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/ce419d95-337f-44e4-ad61-c13ce87ad310">

    <br>

    <img width="1440" alt="Screenshot 2024-06-21 at 8 38 59 AM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/e8f7eca5-2df9-44a1-93f1-ace241056090">


   <h1> Create Compute Resources to run Experiments</h1>
   
   1. Create Compute Instance <br>
   2. Use general purpose one - Standard DS3 V2 6 core CPU, as that's enough for this project; we are not using deep learning to train the model anyway. <br>

<img width="1440" alt="Screenshot 2024-06-21 at 7 04 21 PM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/589825e1-11e0-456c-8288-55940fd23021">


   <h1> Upload the Data</h1>
1. Data (Create) <br>
2. Rename, Data type = Tabular -> Next <br>
3. Upload from Local Files <br>



   <h1> Create a Pipeline in Azure </h1>
   1. Go to Designer <br>
   2. Create new pipeline <br>
   3. Create Compute cluster (to run our experiment) -- Compute Type: Compute Cluster <br>


<h1> Working with Datasets in Azure Studio </h1>

<img width="1095" alt="Screenshot 2024-06-22 at 12 35 10 PM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/4ae923d9-65ef-4a27-b2ff-bb293d2108eb">


1. In the pipeline created above, drag and drop the dataset (say A) from >>> in Designer
2. Previewing the data by clicking on the center circle, to check the columns and what the data looks like.
3. <b> As Var_1 is just categorical column, we dont need this to train the dataset. We need it to categorize/cluster. So we gotta remove it. </b>
4. If you see data profile, there are some missing data as well, gotta remove it.
5. Search "select" to "select columns in dataset" (say B) required for training. Drag and drop into pipeline.
6. Connect/Link both these tables from center.
7. Click on this and click on "edit columns"
8. <b> Include all columns except Var_1 as decided. Also exclude ID column because it is unique - does not add much value  to training. </b>
9. Observe that the data types are different: eg ever_married & graduated are Boolean ; Others are String
10. Now we have seelcted all columns required for ML.

# Preprocessing the Data

<img width="344" alt="Screenshot 2024-06-22 at 12 38 43 PM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/80df0ea2-5581-4e2f-b8f6-2fe7bfd0b302">


<h1> Cleaning the Data </h1> 

1. Search "clean" and select "clean Missing Data" label and drag drop into workspace. Do it Twice to duplicate (As two data types present in dataset)
2. We get Clean_data_1 (say CD1) & Clean_data_2 (say CD2)

<h3> Clean_Data_1 </h3>

1. Link it with B and open edit column to include Column types = Boolean and Column Types = String
2. Cleaning Mode = Replace with Mode

<h3> Clean_Data_2 </h3>

1. Link with CD1
2. Column Type = Numeric
3. Cleaning Mode = Replace with Mean

<h3> Apply One Hot encoding to Categorical Data Type </h3>

<h4> Why? </h4>
We are converting categorical type of variables into numerical type of features because clustering models work well with numeric features (normalized) n.

1. Search "Convert to indicator values" (say CIV) drag and drop - edit and include column types "categorical". Overwrite categorical columns = True.
2. Search "Edit metadata" (say EM) drag and drop - include boolean and string column types - change into categorical. Use default output settings. 
3. Connect EM to CD2 & CIV to EM
4. Search "Normalized data" (say ND) drag and drop - required for scaling in same range- performs well in One hot encoding then
5. Transformation method = minmax (to have range 0 to 1) & Include numeric column types.
6. Connect ND to CIV

   <h3> Submitting the pipeline </h3>
   
<img width="1141" alt="Screenshot 2024-06-22 at 10 24 31 AM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/567fe889-4d8f-4e7b-8737-68888f3934a5">

   1. Experiment create new and submit.
   2. Pipeline Job is created and is running.
   3. You can see the results of this data transformation by "previewing data" at each component.


<img width="729" alt="Screenshot 2024-06-22 at 12 05 50 PM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/860d48b3-0c18-4120-9b44-58626177d4ba">


<h2> K-means Clustering </h2>

1. Drag K-means Clustering (KMC) beside ND. Edit Number of centroids inside = 4 (we want 4 categories/customer segments A,B,C,D, we know this because of dataset that has 4 categ under var_1 column, luckily)
2. Drag Train Clustering Model (say TCM) beneath K-Means Clustering
3. Link KMC and ND

   <h3> Assign Clusters Formed</h3>
   New data which is formed, we gotta assign labels to them. <br>
   Drag and drop! "Assign data to Clusters"
   
<h2> Evaluate Model</h2>

   "Evaluate Model" component - drag & drop!

<h2> Create Inference Pipeline after your pipeline is Ready </h2>

Inference pipeline works on new data. Your pipeline works on given dataset data. <br> So,
Create real-time inference pipeline once your basic pipeline is ready. <br>

Web service input component <br>
Web service output component <br>

are automatically created.
<br>
This means, our pipeline accepts input from the client application and returns the result (i.e cluster assignment, i.e. A,B,C or D) as an output to the client application.  
<br>

<img width="715" alt="Screenshot 2024-06-22 at 9 27 11 PM" src="https://github.com/rxdhikx/Customer-Segmentation-Model-using-Azure/assets/103060090/1d6a33b9-b310-49d6-a535-be7721501575">


<h2> Deploy the Model </h2>

Once inference pipeline is ready, we are good to submit this inference pipeline and deploy to get new endpoints.<br>
Once deployed, we get new Endpoints.
When: <br>
Deplyment rate = healthy <br>
Operation State = succeeded <br>

our endpoint is ready to be consumed by external application!

<h2> Testing in Azure </h2>

Input data/testing data with all the required attributes and test for web service output. <br>
It assigns cluster to each of them. "Assignments =1" <br>
It also displays distance to cluster 0,1,2,3. <br>

<h4> Hence we built and deployed a clustering model using Azure Designer!</h4>

   
