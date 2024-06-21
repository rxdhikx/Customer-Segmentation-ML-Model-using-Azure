<h1> Create Azure Workspace </h1>

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
   1. Create Compute Instance 
   2. Use general purpose one - Standard DS3 V2 6 core CPU, as that's enough for this project; we are not using deep learning to train the model anyway. 



   <h1> Upload the Data</h1>
1. Data (Create)
2. Rename, Data type = Tabular -> Next
3. Upload from Local Files 



   <h1> Create a Pipeline in Azure </h1>
   1. Go to Designer
   2. Create new pipeline
   3. Create Compute cluster (to run our experiment) -- Compute Type: Compute Cluster

   

   

   
