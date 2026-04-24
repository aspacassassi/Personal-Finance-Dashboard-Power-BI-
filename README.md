# Summary
This project allows users to transform manually logged income and expenditure into meaningful visualisations using Power BI, generating insights into personal saving and spending behaviour. Here, I present results from analysing my own logs during my student years, registered using simple Excel tables. Personal results indicate poor spending discipline, characterized by a high variability between months in both spending and saving. Additionally, monthly budgets were more often than not exceeded, the saving goal was rarely met, and non-essential subcategories such as "Eating Out" and "Vacation/Travel" featured routinely in the most expensive subcategories per year. As a result, net cash flow decreased year after year, leading to low saving during the analysed period. The results highlight the importance of adhering to predetermined budgets, the need for increased prioritisation of spending, and the necessity for a steady stream of income to allow consistent saving and accurate saving estimates. Instructions on how to use the dashboard are detailed at the end of this ReadMe file.

### Tools used
Excel, Power BI, DAX

# Problem Description
Keeping track of income and spending is key to achieving financial goals. Although manually logging transactions with Excel can already provide an overview of cashflow, clear visualisations are what truly drive financial insights. The current project addressed this issue by complementing the ease of use offered by simple Excel/Google Sheets tables with more extensive visualisations generated in Power BI.

# Goal
- Uncover spending trends
- Create a budget control page
- Find out how spending and saving was distributed per month
- Discover which (sub)categories were the most expensive per year
- Keep the sheets and dashboard usable by anyone (i.e., it should not require any programming or statistical knowledge)

# Methods
This project required obtaining easy to use input sheets where spending and income could be logged, transforming the data to a format usable within Power BI, and finally creating visualisations and measures that could answer the research question. These steps are outlined in detail below.

## Input Sheets
Excel input sheets were downloaded from https://www.vertex42.com/ExcelTemplates/personal-budget-spreadsheet.html and have been manually updated since 2022. Categories and subcategories were adapted to better fit actual spending behaviour. Totals and averages were removed from the tables to avoid double counting in Power BI, but were kept in the sheets for debugging. Table names were adapted to reflect their contents as this would make working in the BI environment easier.

_An Example of an Input Sheet_
![An Empty Input Sheet](/Images/EmptyInputSheet.png)

## Data Wrangling
Since the Excel tables were created to provide an easy method of logging spending, the data was initially in a wide format. Thus, the first step towards generating meaningful visualizations required data wrangling. Although this step could have been accomplished with Power Query within Excel, I chose to complete it within Power BI so that future users would only have to upload their sheets into Power BI, which would in turn format the tables automatically.  
Because each input sheet holds multiple separate tables, the query was first completed on a sample table and then applied to all tables equally.

_Transforming the Data to Long Format_
![Unpivotting](/Images/UnpivotingTables.png)  

With the tables unpivoted, further data wrangling only required changing column names to more informative names and adding a type that differentiated logs as either expenses, income, or tranfers. Finally, all tables were then appended to a Full Table, which held the information from all categories in a single table

## Creating the Dashboard
With a workable table complete, actual analysis could finally begin. To complete all of the analysis goals, I decided to create multiple pages where income and expenses could be analysed at differing levels of depth. This was achieved by designing a **Yearly Cash Flow & Savings** page , a **Budget** control page, a **Trends** page, and finally a **Spending Anatomy** page. More detail on each page can be found below. DAX code for all custom measures can be found in the file "DAX Measures".

### Yearly Cash Flow & Savings
This page offers a quick glimpse into financial health, instead of a a deeper analysis into the destination of spent income. As such, it seemed appropriate to include cards highlighting **Total Yearly Income**, **Total Yearly Expense**, **Total Yearly Net**, as well as some trend indicators such as **Year-over-Year Percent Change (YoY%)**, **Monthly Averages**, **Percentage Change in Comparison to the Previous Month**, and a line chart showing Income vs Expenses over the entire year. Finally, information on saving was visualised by a gauge meter measuring the **Saving Rate** and a pie chart. A slicer on the top left allows users to choose different years, which can be helpful in identifying trends across years.

_Year Cash Flow & Savings Page_
![First Dashboard Page](/Images/YearlyCashFlowAndSavings.png)

### Budgets
Next, I created a second page where **Essentials** and **Non-Essential** monthly budgets could be checked. This included gauges meters where the maximum value is equal to the budget, cards showing how much of the budget is remaining in absolute values, and which percentage of it has already been spent. These visualisations offer a simple way of checking the current status of a monthly budget, thereby allowing a clear aid for policing spending behaviour. Stacked bar charts below help users identify which subcategories place the most burden on the budget, which can support a prioritisation of expenditure.

_Budgets Page_
![Second Dashboard Page](/Images/Budgets.png)
_215% spent ... Oops!_ :sweat_smile:

Importantly, the values for each budget are calculated dynamically depending on the monthly income and percentages registered by the user. The default is set to 70% of income towards **Essentials**, 20% for **Savings**, and 10% for **Non-Essentials**, following a common rule of thumb regarding personal finance. Users can change these values at any time in the input sheets.

### Trends
The trends page gives a higher level of analysis into spending behaviour by computing measures that span multiple months or the entire year. Nevertheless, it is still more specific than the **Yearly Cash Flow & Savings** page due to the distinction between **Essential** and **Non-Essential Spending**. Line charts detailing spending and the budget threshold provide visual information on the adherence to monthly budgets, while a **3-month rolling average** can provide a forecasting-like measure. Similarly, a line chart showing the **Saving Rate** per month shows how strictly the 20% saving rate rule was followed. These charts are supplemented by cards that show the adherence to budgets numerically, the decreasing or increasing trend of spending/saving as well as the subcategory with the most increase in spending. The latter informs what type of spending may have gotten out of hand in the past 3 months, which again can guide a better prioritisation of cash allocation. 

_Trends Page_
![Third Dashboard Page](/Images/Trends.png)
_1600% increase in Warhammer expenditure ... I blame GW for making pretty models_ 

### Spending Anatomy
Finally, the **Spending Anatomy** page shows expenditure at a more granular level. It requires more interaction by the user to get the most out of it compared to other pages, but it provides the most detailed view of where cash has been spent. A large stacked column chart with spending by month separated by categories is meant to be the first step in this analysis stage, showing which categories might have disproportionally large spending on a particular month, or across months. Next, a pie chart details spending by subcategory. This chart interacts with two slicers which allow the user to select a specific category and month. For instance, in the image below it is clear that "Transportation" was the category with the most spending in February, and by selecting this category and month with sliders, it becomes clear that a large sum was spent on "Motorcycle" as well as "Cambio" (a carsharing app). Given that both subcategories relate to private transportation, these results invite the user to reflect on the necessity of mainting two means of personal transportation. However, looking into different months by using the slider reveals that these were unusual spikes of spending within these subcategories, suggesting that such spending might not be problematic, although the usage of the carsharing app might require a more cautious approach due to its high cost.

_Spending Anatomy Page_
![Fourth Dashboard Page](/Images/SpendingAnatomy.png)

Zooming out, a stacked column chart highlights the top 5 most expensive subcategories of the selected year, providing a granular view of where the most cash has been spent.

# Limitations
Although the current project already provides analyses at varying levels of depth, it lacks the capability of producing forecasts. Such a function is likely easy to implement using R or Python by predicting future monthly income and expenditure based on previous years, and then uploading the forecast back to Power BI. However, this requires more extensive knowledge by the user, which is against the goal of creating an easy to use dashboard.
Further, the current project does not track net worth since there is no way to track savings balance, assets value, or an investment portfolio. Although useful, these features are beyond the intended use of the dashboard.
Importantly, the self-imposed deadline for this project has already been reached and so there are no plans (yet) to add any new features. Feel free to reach out if you have any feedback or ideas for the dashboard!

# How to Use the Dashboard With your Own Data
Using this dashboard to uncover your own spending and saving patterns is quite straightforward. Firstly, download all the files in the repository. Then use the Empty Input Sheet found in the Data folder. Importantly, rename this file to the year you are registering. For instance, if you are logging your transactions of 2026, name the file "2026". This is important since the name of the file is used to identify the year of transactions in Power BI. Don't forget to use a new file for each year, using new sheets within the same file will not work.  
You will have to manually register all your income and expenses in the sheet. If you don't have Microsoft Excel, use Google Sheets or Excel for Web instead since those are free. Don't forget that you can add new subcategories within the existing categories to better fit your needs. You can do this by adding a new row in the table and naming it however you want. To log transactions, simply double click on a cell and add a value to it. This proces is easiest if you open your banking app and then add transactions one by one in the right categories. You can first enter a "=" sign and in the cell and then log each transaction with a "+" in between. That way Excel/Google sheets will compute the total value for that cell for you.  
When you have registered your data, open  Personal Finance Dashboard.pbix that you can find in the repository files. If you don't have Power BI yet, you can download it for free at the Microsoft Store. With the .pbix file open, head to "Transform Data", click on the arrow, and select "Data Source Settings". From there, select "Change Source" and select the folder storing all your sheets. Finally, close the popup and click refresh to visualise your own data.

_Where to Find Transform Data_
![Changing Source](/Images/TransformDataButton.png)
