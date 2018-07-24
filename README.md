# IntroToML
In this repository, we will be exploring and focusing on the IBM Watson Data Platform to dive into working with the Machine Learning pipeline. This will include performing activities from data cleansing using the IBM Data Refinery service to creating a simple machine learning model using the IBM Watson Machine Learning service and creating an interactive dashboard using the Cognos Dashboard Embedded service to visualize data.

This repository used the following resource, which can be explored to look at each part in more depth: 
https://developer.ibm.com/code/labs/Data-Science-Data-Refinery
https://developer.ibm.com/code/howtos/ml-in-minutes
https://developer.ibm.com/code/howtos/create-interactive-dashboards-on-watson-studio

## Sign up on IBM Cloud
An IBM Cloud account - A lite account, which is a free of charge account that doesn’t expire, can be created through going to [IBM Cloud](http://ibm.biz/sheraaml). Make sure to set the region to US South.

## Create a Watson Studio service instance
- Select **Catalog**
- Click on **AI** from the menu on the left
- Select **Watson Studio**.

![Watson Studio service](images/1.png)

- Enter the **Service name** or keep the default value and make sure to select the **US South** as the **region/location**
- Select **Lite** for the **Plan**, which you can find under **Pricing Plans** and is already selected. Please note you are only allowed one instance of a Lite plan per service
- Click on **Create**

![Create Watson Studio service](images/2.png)

- You will be taken to the main page of the service. Click on **Get Started**. This will take you to the **Watson Studio**

![IBM Watson Studio service (Get Started)](images/3.png)

platform. If this is your first time on this platform and you don't have an associated account, you will be asked to **Confirm your IBM Cloud organization and space information**

![IBM Watson Studio service (Get Started)](images/4.png)

## Create a New Project
- On the **IBM Watson Watson** main page, click on **New project** Under **Get started with key tasks**
- Select **Complete** and click on **Ok**

![New project](images/6.png)

- Enter a **Name** and **Description** for your new to-be-created project
- Under **Define storage**, add a new **IBM Cloud Object Storage** instance by clicking on **Add** under **Select storage service**
- In the new window that gets opened, select **Lite** as the **Plan** and click **Create**
- Enter the **Service name** or keep the default value
- Click on **Confirm**

![New project](images/7.png)

- Click on **Refresh** to see the newly created service instance and get it selected
- You can select to **Restrict who can be a collaborator** under **Choose project options** if you wish to do so at this stage
- Click on **Create**

![Create a new project](images/8.png)

## Adding the data assets
- You should be taken to a page showing an **Overview** of the project you just created
- Click on **Assets** on the panel found under the name of your project at the top of the page
- At the top right of the page, click on the icon that has zeros and ones (two of each)
- Click on **Load** and drag and drop the files **adult_income.csv**, which can be found this GitHub repository under the folder **Data sets**.

![Adding data assets](images/9.png)

- You will notice that once the files are uploaded, they will be added under **Data assets**.

## Part 1: Data Refinery
- Go to the triple dot menu next to next to **adult_income.csv** under **Data assets** and select **Refine**

![Data tab](images/11.png)

- On the panel on the right, you will find **Details** including the project the data asset belongs to, and description of the resulting data set we will get after the refining process. Close it for the time being

![Data tab](images/12.png)

- Click on **Steps**, which you can find right hand-side of the page. This is where you will see each operation you will define while transforming the data. It shows the data flow defining the operations to be done on the entire data set

- Click on the **Profile** tab and talk quick look at data summary and get a feel of you data (do this after skimming through your data displayed in the **Data** tab)

![Profile tab](images/13.png)

- Click on the **Profile** tab and take a closer look at the column *GENDER*. You will notice some additional values other than *Male* and *Female*, mainly ones that we want to change to *Male*.

![Harmonization data in the *GENDER* field](images/14.png)

- Click on **+Operation** and select **Replace substring**, which you can find under **CLEANSE**.
- Choose *GENDER* as the **Selected column**. Under **Pattern** tab, type <i>^(?!(Male|Female))([Mm].*)</i> under **Regular expression** and *Male* under **Enter the string replace with**. Make sure to select **Replace all occurrences**.

What is meant by <i>^(?!(Male|Female))([Mm].*)</i> is to find any expression that doesn't start with *Male* or *Female* and starts with the letter *M* or *m*, which could be followed by any character.

![regex](images/regex.png)

- Click **Apply** and go to the **Profile** tab again to for a final check.

![Harmonization data in the *GENDER* field 2](images/16.png)

- Click on the **Profile** tab and take a closer look at the column *AGE*

![Harmonization data in the *AGE* field](images/17.png)

- Click on **+Operation** and select **Split column**, which you can find under **ORGANIZE**.
- Choose *AGE* as the **Selected column**. Under **POSITION** tab, type *2* under **Positions** and *AGE_num,AGE_str* under the **Names of new columns**. Make sure to unselect **Keep original column**
- Click **Apply**.

Bear in mind that this is not the best approach to handle this. This is just provide an example of how to use the **split column** operation.

![Harmonization data in the *AGE* field 2](images/18.png)

- Go to the **Data** tab and remove the newly created column called *AGE_str*, which only contain the string part of the age.

![Harmonization data in the *AGE* field 2](images/19.png)

- Go to column called *AGE_num* and rename it to *AGE*

![Harmonization data in the *AGE* field 2](images/20.png)

- Go to the **Profile** tab again to for a final check.

- Click on the **Profile** tab and take a closer look at the column *MARITAL_STATUS*

![Removing empty rows](images/21.png)

- Go to the **Data** tab
- Go to the column called *MARITAL_STATUS* and remove rows with any empty values by clicking on the triple dot menu next to the column name and selecting **Remove empty rows**

![Removing empty rows](images/22.png)

- Go to the **Profile** tab to check if all empty values have been removed.

- Go to the **Data** tab.
- Go to the column called *AGE* and change its type to Integer by clicking on the triple dot menu next to the column name and selecting **CONVERT COLUMN TYPE** followed by selecting **Integer**.

![Change the column data type](images/23.png)

- In the same way, change the data type of *HOURS_PER_WEEK* and INCOME_NUM* to **Integer**, and *CAPITAL_GAIN* and *CAPITAL_LOSS* to **Decimal**.

![Change the column data type](images/24.png)

- At this point, you should have 10 **Steps**
- Click on the play button to run the data flow as seen below.

![save&run](images/25.png)

- Change the **Name** under **Data flow details** to *adult_income.csv_flow* and the **Name** under **Data flow output** to *adult_income_shaped.csv*.
- Click on **Save and Run**

![Running the data flow 2](images/26.png)

- In the window that pops up, click on **View Flow** to track the progress of the running data flow.

![Running the data flow 2](images/27.png)

- The data flow should start running, executing each of the operations we defined. If things goes well, you should see the page similar to the one displayed below.

![Running the data flow 3](images/28.png)


## Part 2: Interactive Dashboard
- Go to the Dashboards section and click on **New dashboard**

![image](images/29.png)

- Enter a **Name** and **Description** for your new to-be-created dashboard
- Under **Associate a Cognos Dashboard Embedded service instance**, add a new **Cognos Dashboard Embedded** instance by clicking on the link

![image](images/30.png)

- In the new window that gets opened, select **Lite** as the **Plan** and click **Create**
- Enter the **Service name** or keep the default value

![image](images/31.png)

- Click on **Confirm**
- Click on **Refresh** to see the newly created service instance and select it
- Click **Save**

![image](images/32.png)

- Select a template for your dashboard. You have 3 options: Single page, Tabbed, or Infographic. Select **Infographic**

![image](images/33.png)

- Click **OK**

- From the panel on the left in the Data section, click **Selected sources** to define the data source
- Click on *adult_income_shaped.csv* and click **Select**

![image](images/34.png)

- Click on the added data set to expand its field and start working with it

![image](images/35.png)

- To create the first visualization, select *NATIVE_COUNTRY* and *INCOME_NUM* and drag them onto the infographic template

![image](images/36.png)

- You will see that a Map as selected as the default type of visualization in this case. Keep it
- Click on the small window with an arrow at the top left of the vissualization to explore more options
- Click on the triple dots beside *INCOME_NUM*, select **Summarize** and click on **Average**

![image](images/37.png)

- Select *MARITAL_STATUS* and drag onto the templete to create the next visualization
- Set the visualization to a **Pie** chart
- Configure it and select **Count** under **Summarize**

![image](images/38.png)

- Continue to add more visualizations to explore your data and gain valuable insights

![image](images/39.png)

- Add a title to your infographic
- Click Save once finished editing 
- Click on the Share button to create a Permalink to a Read-only version of the dashboards you created

![image](images/40.png)

- You can check an example dashboard that you can interact with this [link](https://dataplatform.cloud.ibm.com/dashboards/bd6c1afc-52c2-4fba-b2ce-5d485c370a16/view/4428a479379c0c9e6abdd0e4079a7e577c617108b6bb8501d1d67b495e367897a8601496c82a4953dd175031fbeb1a0c9c)


## Part 3: Deploy a Machine Learning Model
- Click on **New Watson Machine Learning model** in the Watson Machine Learning models section

![image](images/42.png)

- Enter a **Name** and **Description** for your new to-be-created model
- Under **Machine Learning Service**, add a new instance by clicking on the link

![image](images/43.png)

- In the new window that gets opened, select **Lite** as the **Plan** and click **Create**
- Enter the **Service name** or keep the default value

![image](images/45.png)

- Click on **Confirm**
- Click on **Refresh** to see the newly created service instance and select it

- Under **Spark Service**, add a new instance by clicking on **Associate an IBM Analytics for Apache Spark instance**

![image](images/44.png)

- In the new window that gets opened, select **Lite** as the **Plan** and click **Create**
- Enter the **Service name** or keep the default value

![image](images/46.png)

- Click on **Confirm**
- Click on **Refresh** to see the newly created service instance and select it
- Select **Model builder** as the model type
- Select **Manual** to allow you to prepare your own data and select the model to train
- Click **Create**

![image](images/47.png)

- Select the data set to work with (in this case *adult_income_shaped.csv*)

![image](images/48.png)

- Click **Next**
- Select *INCOME(String)* as the label column and everything else excluding *UNIQUE_ID* and *INCOME_NUM* as the feature columns
- Select **Binary Classification** and leave the **Validation Split** as it is

![image](images/49.png)

- Click on **Add Estimators**
- Select all estimator from which the best performing one will be selected later

![image](images/50.png)

- Click **Next**
- Select **LogisticRegression** and click **Save** to save the model best fit to the data

![image](images/51.png)

- You can deploy the model by going to **Deployments** tab and clicking on **Add Deployment**

![image](images/53.png)

- Insert a **Name** and **Description** for the deployment 
- Select **Web service** as the **Deployment type** 

![image](images/54.png)

- You can check sample code that can be used for implementation purposes by going to **Implementation** tab

![image](images/55.png)

- You can test out the model by going to **Test** tab and filling in the values of the features (a json object can also be used)

![image](images/56.png)


And this marks the end of the exercise
