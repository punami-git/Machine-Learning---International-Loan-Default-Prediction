
# **International Debt Statistics and Projection**
## <a name="_5afctxl8crr"></a>**Team Members and Github Ids**
Jill Karia (Github ID: jillkaria2709)

Janelle Bauske (GitHub ID: jbauske01)

Punami Chowdary (GitHub ID: pcdukkip)

Nandita Ghildyal (GitHub ID: gappy401) (Point of contact)

Sandra Chiwike (Github ID: SandraNgozi)
## <a name="_2qxuawuy760l"></a>**Introduction**
Our overall goal is to develop a model that predicts the likelihood of a country defaulting on its external international loans via the World Bank or IMF. Unlike traditional models that rely solely on the financial factors of a country, our approach considers  the country’s historical debt statistics through a time series in addition to various socio-economic factors to assess its ability to repay external loans. 

Our model aims to identify the root causes of economic instability affecting a country, which, in turn, affects its ability to repay a loan. Once these factors are identified, organizations such as the World Bank can work proactively with countries to target areas of instability via monetary aid to reduce the likelihood of a default occurring. This model will assist in facilitating more sustainable lending practices for organizations such as the World Bank or IMF. 

Additionally, our model can be used by government institutions or relevant policy makers to inform decisions regarding external borrowing and monitoring outstanding debt. Our model can also be employed by NGOs or to be considered for initiatives like the HIPC (Heavily Indebted Poor Countries) to help target their interventions by identifying the underlying causes of the problem.

## <a name="_k908qhrj3mxk"></a>**Literature Review** 

In our review of existing literature, we found that Luckner and Farah-Yacoub's 2023 study<sup>[^1]</sup> emphasized the significant consequences of global debt defaults, highlighting an increase in defaults and the associated human hardships. The urgency conveyed by this study prompted our commitment to addressing this critical issue.

Our examination revealed that the prevailing approach to Debt Sustainability Analysis (DSA) primarily relies on economic metrics, qualitative evaluations, and historical debt records. However, we identified a notable gap in integrating the broader impacts of debt on human flourishing. This perspective was supported by a comprehensive study conducted by the Human Flourishing Program at Harvard<sup>[^2]</sup> , which involved about 200,000 participants and illustrated how the six domains of human flourishing are intertwined with financial growth. This study underscores the necessity of considering both economic and social factors in evaluating debt sustainability.

The primary stakeholders of our research are institutions such as the World Bank and international lending organizations, as well as various governmental organizations and financial sectors involved in the lending process. These stakeholders, especially lending organizations like the World Bank, require precise insights into the underlying causes of loan defaults to tailor their funding strategies effectively to the specific needs of different countries. Similarly, governmental bodies engaged in these lending processes would benefit from enhanced data-driven decision-making capabilities, facilitating more effective policy formulation and implementation. 

To address these needs, our methodology incorporates time series modeling, inspired by the Bangladesh Development Studies' paper<sup>[^3]</sup>, which uses historical data and ARIMA models to predict future economic conditions. This approach was compared to the World Bank’s ‘judgmental projection’ method and found to offer a more objective means of predicting economic indicators. By adopting this statistical modeling technique, we aim to provide a robust, data-driven foundation for forecasting debt repayment capacities, thus enhancing the efficacy of DSA processes and supporting our stakeholders in making informed decisions that account for both economic indicators and human impacts.
## <a name="_oyh6x34ftkix"></a>**Data and Methods** 
### <a href="https://docs.google.com/spreadsheets/d/1rEyVoZ-jHyZ6lElWPyO34NCAvKP93XqF/edit?usp=drive_link&ouid=118034390497098356446&rtpof=true&sd=true" target="_blank">Data</a>


Our dataset primarily draws data from the World Bank and the U.S. Department of the Treasury regarding debt metrics. The group collected diverse data covering various sectors such as health, employment, social issues, corruption factors, and the debt levels themselves. Initially, data were compiled from a select group of countries, which we then integrated to form a consolidated dataset, aimed at testing the feasibility of our proposal.

In our last data collection iteration, the dataset expanded to include data from the 1960s to 2022, covering 170 countries, resulting in a substantial dataset of 9,676 columns and 40,000 rows. Upon reviewing the limitations for our modeling, we identified an imbalance in the column-to-row ratio. To rectify this, we used XGBoost on the individual datasets to identify and retain only the most critical features. This refinement process significantly streamlined our dataset to 114 columns and 11,000 rows.

The features of our dataset encompass a broad range of indicators across multiple domains—including socio-economic, educational, health, labor, demographic, and other miscellaneous factors. The target variable of our analysis is the national debt expressed as a percentage of GDP.
<p align="center">
  <img src="./image3.png" alt="Description of Image">
  <br>
  <em>2.1 Average Debt as % of GDP for 95 Percentile Countries</em>
</p>


The map above illustrates global debt levels as a percentage of GDP, with varying degrees of debt burden across different regions over the past few decades. Guyana in South America are shown with very high debt levels, indicated by deep red. North American countries exhibit moderate debt levels that don’t fall in the 95 percentile. European countries vary widely with some like Estonia showing low debt, while central regions appear to have higher debt levels. 

Africa predominantly features low to moderate debt levels with countries like Guinea-Bissau and Liberia specifically highlighted for debt overr 100% of its GDP. In the Middle East and Central Asia, countries such as Azerbaijan and Kazakhstan also show low debt levels, whereas Cyprus is an exception with higher debt.

On looking at the explanatory variables we find high positive correlation between age-related demographics such as the old-age dependency ratio and the percentage of the male population aged 65 and above, suggesting that regions with a higher proportion of elderly males tend to have higher dependency ratios. Education-related metrics, particularly gender parity indices (GPI) for primary and secondary education, also show strong correlations. This implies that gender parity in primary education is a good predictor of parity in combined primary and secondary levels. Other notable correlations involve birth rates, neonatal deaths, and age populations, indicating significant relationships between demographic changes and dependent population ratios.

<p align="center">
  <img src="./image2.png" alt="Description of Image">
  <br>
  <em>2.2 Trends in explanatory variables over time</em>
</p>

### <a name="_oyh6x34ftkix">Methods</a>


### Data Collection and Initial Setup
Our research project commenced by sourcing original data from three distinct databases provided by the World Bank and the U.S. Department of the Treasury, specifically focusing on debt metrics. Initially, we extracted data exclusively for the United States, organizing our features as columns and years as rows in Excel. This arrangement resulted in a dataset comprising 63 rows and 86 columns.

### Feature Selection and Data Imputation
We employed XGBoost to distill the dataset to the top 100 most critical features. We encountered significant issues with missing data—a common challenge in large economic datasets. Traditional imputation methods were insufficient due to the nuanced nature of economic variables.

### Choice of Modeling Technique: VAR
Given the intricacies of economic forecasting, we opted for the Vector Auto Regression (VAR) model due to its robust capability in managing multiple interdependent time series. The model cannot handel null values so we utilised the iterative imputer to fill in the null values. The VAR model excels in forecasting multiple series by acknowledging their interdependencies, such as the inverse relationship observed between unemployment and GDP.

However, VAR models are constrained by the ratio of features to observations, dictated by the formula \( n > k \times p \) (where \( n \) is the number of observations, \( k \) is the number of variables, and \( p \) is the number of lags). For our data, this limitation meant we could either incorporate 21 features with 3 lags or 14 features with 4 lags, which significantly restricted our ability to analyze a more comprehensive set of variables.

### Transition to LSTM Models
To overcome the limitations of the VAR model, we shifted our focus to Long Short-Term Memory (LSTM) networks. An initial experiment with a simple LSTM model on U.S. data produced promising results. To address potential biases of using data solely from the U.S., we expanded our dataset to include socio-economic, health, and educational data from an additional 170 countries. This expansion culminated in a comprehensive dataset, which we consolidated using an outer join by country and year, resulting in a data frame with over 10,000 columns.



### Data Masking Strategy
Due to the limitations of traditional imputation methods, we decided on a data masking approach to handle missing values. We masked the missing data with a placeholder value of -999, ensuring that these entries would not impact the model’s performance. This method allowed us to preserve the integrity of the dataset while effectively managing missing data, which was critical for maintaining the reliability of our predictive models.

### Refining the LSTM Approach
We further refined our approach by building a split-sequences function that created a rolling window with 10 steps in and 5 steps out. This enabled the LSTM model to manage data more effectively, processing sequences in digestible chunks. This setup, combined with data normalization (min-max scaler) and strategic splitting into training and testing sets, prepared our data for more effective deep learning applications.

Despite various iterations, including testing bi-directional and stateful LSTMs, we finalized an LSTM that consisted of 2 RELU layers that included a time-distributed layer and a repeat vector layer, essential for capturing the dynamic nature of the sequences.

### Model Training and Evaluation
We tested three different custom loss funtions - scaled MSE, Asymmetric Error and Mean Squared Logarithmic error but decided to go with  the default loss funtion which is MSE. However, we did write our own custom metric funtion, which is mean cude error. We also selected an 'adam' optimizer. After training, we tested the model using our designed test set, reshaping our target variables to meet LSTM requirements and conducting a thorough analysis through mean absolute error metrics and graphical comparisons of predicted versus actual values.

### Testing on Unseen Data
In a final validation step, we applied our refined model to unseen data from Argentina, which had been excluded from the initial training phase. This test provided additional evidence of our model’s predictive accuracy and its applicability to different economic contexts across various countries.

## <a name="_oyh6x34ftkix">Results</a>
**_XGBoost Results:_** 

The XGBoost model gave us the most important features for predicting debt. The 25 most important predictors are shown in the screenshot below. Health and education indicators proved to be more predictive of a country’s debt than economic and financial factors. This confirms our hypothesis that the root cause of economic instability is more often than not, social issues. 
<p align="center">
  <img src="./image4.png" alt="Description of Image">
  <br>
  <em>4.1 Results from Feature Importance through XGBoost</em>
</p>

**_LSTM Results:_** 

Our final LSTM model successfully predicted a country’s Debt as a percentage of its GDP.  predicted values of a country’s Debt as a percentage of its GDP. The overall Mean Average Error for our model is 0.027. This means that on average, our model’s predicted values differ from the actual values by approximately 0.027 units. It's important to note that the unit for our y values is a percentage of the country's GDP.

<p align="center">
<span style="display:inline-block; background-color: #000; color: white; padding: 8px 15px; text-align: center; border-radius: 0%; font-size: 16px;">MAE: 0.027</span>
</p>

The below graph depicts the actual y values plotted against the predicted y values.

<p align="center">
  <img src="./image1.png" alt="Description of Image">
  <br>
  <em>4.2 Results from the LSTM</em>
</p>

Our model is able to accurately predict the trend of the debt variable, however, it occasionally struggles to capture the full complexity of the trends. This could be due to two reasons: the model is not complex enough to account for all the nuances or there are too many null values in our data. Our attempts to increase the model complexity by increasing the number of layers in the LSTM only worsened the results. This suggests that the model’s limitations are most likely due to the large number of null values in our data.  Additionally, the predicted value for our very last time step is consistently higher than the actual value because it's at the end of the rolling window. 

## Analysis

The business interpretation of our results is threefold: we have a quantitative, qualitative, and a post hoc analysis. For instance, if the World Bank/IMF wants to determine the likelihood of a country defaulting on its external loan, they would use the following evaluation methods. Let's run the analysis using Argentina as an example:

### Quantitative Factors: 
**Debt as Percentage of GDP:**
- Our LSTM model outputs a predicted value for Debt as a percentage of GDP. For instance, Argentina has a value of 48.02661871. This is almost 50% of the country's GDP, which is above the threshold we set for a developing country like Argentina. This suggests that Argentina is most likely going to default. 
- Evaluation: To evaluate this, we can compare this predicted outcome to the actual outcome. Argentina did indeed default on its IMF loans in 2001.

### Qualitative Factors:
We also use qualitative factors to support our verdict that Argentina is going to default on its loan. 
- **Type of Loan:** Argentina's IMF loan was a Stand By Arrangement, which essentially means a short-term loan. 
- **Terms of the loan:** The IMF's loan was conditional on Argentina implementing a series of austerity measures and structural reforms, including reducing government spending, reforming the tax system, and restructuring the financial sector. The loan had specific repayment schedules and interest rates determined by the IMF.
- **Projects funded by the loan:** The loan was of $40 billion, specifically aimed at projects that would strengthen its financial sector. 
- **Country’s current economic and political status:** In the years leading up to the default, Argentina was in the middle of a deep recession, with high levels of unemployment and poverty. The economic crisis, in turn, led to political unrest. 

### Post Hoc Analysis:
After obtaining predictions, we conducted a post hoc analysis to understand the reasons behind the predictions. 
- The economic and political conditions in Argentina in the years leading up to the default and the strict terms and conditions imposed by the IMF led to Argentina defaulting. 
- While these strict terms and conditions are intended to stabilize the economy and reduce debt, they can also have negative consequences, such as deepening recessions and increasing unemployment. The government's inability to grow the economy under these conditions made it difficult to generate the revenue needed to repay its debts, leading to the default.

- As we hypothesized, Argentina had a higher number of neonatal deaths compared to other countries with similar population sizes. Neonatal deaths was one of the top most important features from our XGBoost model.

### Conclusion:
A combination of factors led Argentina to default on its IMF loan: a deep recession, political unrest, etc. But the most actionable insight for our stakeholders is that the strict terms and conditions and austerity measures imposed by the IMF on Argentina causing the economic collapse of Argentina and leading it to default on its international loans. 

Instead of focusing solely on fiscal consolidation, the IMF could have focused on policies that promoted economic growth, job creation, improving the public sector, etc.


## <a name="_oyh6x34ftkix">Discussion</a>


- Using a continuous variable to represent debt as a percentage of GDP, rather than a binary classification of default (yes/no), provides more nuanced insights. This continuous measurement allows for a more detailed analysis of a country's financial health, offering stakeholders a spectrum of risk levels rather than a simplistic default indicator. This approach enhances the utility of our model by providing deeper, actionable insights that better inform decision-making processes.

- While our model demonstrates robust performance, characterized by low error rates, it's important to acknowledge that the complexities of economic stability cannot be fully captured by a single metric. The nature of this problem extends beyond mere numerical data to include qualitative factors such as governmental policies and socio-economic conditions. These elements require a broader analytical approach to accurately assess and predict outcomes, underscoring the need for a model that integrates both quantitative and qualitative data for a comprehensive evaluation.



## <a name="_oyh6x34ftkix">Limitations</a>

- The black box nature of our model hinders transparency and interpretability,  which are crucial for stakeholders to trust and effectively use the predictions it generates. If we could access and analyze the model’s internal weights, it would enable us to conduct detailed post hoc analyses to explore potential interactions between different sectors. By quantifying these interactions and assessing their impact, we could not only enhance our understanding of how various factors contribute to the model's predictions but also improve the model’s accuracy and reliability. This approach would allow for a more transparent assessment of the model.

- Without expertise of social scientists and economists, handling null values diminishes its practical utility, emphasizing the need for domain knowledge integration.
- Implementing a rolling window for debt analysis is a success, but translating insights into strategies remains challenging. Refining the model architecture and integration with decision-making processes are still required.
 
## <a name="_d3spbtui6nye"></a>**Future work**

Incorporating methods such as LIME (Local Interpretable Model-agnostic Explanations) can help traslate how different features influence predictions. Additionally, hosting educational workshops for stakeholders can demystify the model's mechanics, thereby boosting user confidence in its application.

Expanding our model to use a multi-class classification system, rather than just a continuous target variable, could significantly enhance our analysis of default risks. This transition would enable us to categorize countries into more specific risk profiles—such as "weak social sector" or "strong healthcare"—rather than simply quantifying debt as a percentage of GDP. By doing so, stakeholders would gain a more detailed understanding of various risk dimensions, facilitating the development of targeted intervention strategies that are tailored to the unique needs and strengths of each country. This approach not only refines risk assessment but also supports more effective and customized policy recommendations.

Lastly, developing a user-friendly dashboard that presents dynamic risk assessments based on the rolling window in the model will enable decision-makers to access and utilize insights efficiently. Additionally, ensuring that the model's development aligns with ethical standards and addresses potential biases will maintain its integrity and fairness, particularly in its application across diverse international contexts. These improvements are essential not only to increase the model’s strategic utility but also to foster trust and collaboration between technologists and global economic stakeholders.





[^1]: Clemens Graf von Luckner, J. P.-Y. (2023). *The Human Cost of a Failing Global Debt System*. Open Society Foundations. [Link](https://www.opensocietyfoundations.org/uploads/d3b9702b-68dd-4083-95ba-f49c8b6fe705/the-human-costs-of-a-failing-global-debt-system-20230620.pdf)

[^2]: *The Human Flourishing Program*. (n.d.). Retrieved from [Harvard University](https://hfh.fas.harvard.edu/global-flourishing-study)

[^3]: Goswami, G. G., & Hossain, M. M. (2013). *From Judgmental Projection to Time Series Forecast: Does it Alter the Debt Sustainability Analysis of Bangladesh?* The Bangladesh Development Studies, 36(3), 1-41. [Link](https://www.jstor.org/stable/44730018)


