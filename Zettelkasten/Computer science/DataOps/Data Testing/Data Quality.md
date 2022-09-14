222022-08-30

Style: #Research 

Domain: #DataOps 

Tags: #Data 

# Data Quality
 Data quality may be easy to recognize but difficult to determine. For example, the entry of Mr. John Doe twice in a database opens several possibilities. Maybe there are two people with the same name. Or, the same person’s name is entered again mistakenly. It can also be the case of the database not being validated after migration or integration. High-quality data eliminates such ambiguities and ensures that each entity is represented correctly and uniquely. In another example, the entry of Mr. John Doe, Waterloo, is incomplete without the country’s name. If the dataset shows the height of Mr. John Doe as 6 Meters, it can be an error in the measuring unit. 

These data quality examples demonstrate how you cannot rely on just one metric to measure data quality. You can consider multiple attributes of data to get the correct context and measurement approach to data quality. For example, patient data in healthcare must be complete, accurate, and available when required. For a marketing campaign, customer data needs to be unique, accurate, and consistent across all the engagement channels. Data quality dimensions capture the attributes that are specific to your context.

### 1.      Completeness

This dimension can cover a variety of attributes depending on the entity. For customer data, it shows the minimum information essential for a productive engagement. For example, if the customer address includes an optional landmark attribute, data can be considered complete even when the landmark information is missing.

For products or services, completeness can suggest vital attributes that help customers compare and choose. If a product description does not include any delivery estimate, it is not complete. Financial products often include historical performance details for customers to assess alignment with their requirements. Completeness measures if the data is sufficient to deliver meaningful inferences and decisions.

### 2.  Accuracy

Data accuracy is the level to which data represents the real-world scenario and confirms with a verifiable source. Accuracy of data ensures that the associated real-world entities can participate as planned. An accurate phone number of an employee guarantees that the employee is always reachable. Inaccurate birth details, on the other hand, can deprive the employee of certain benefits.

Measuring data accuracy requires verification with authentic references such as birth records or with the actual entity. In some cases, testing can assure the accuracy of data. For example, you can verify customer bank details against a certificate from the bank, or by processing a transaction.  Accuracy of data is highly impacted on how data is preserved through its entire journey, and successful [data governance](https://www.collibra.com/us/en/data-governance) can promote this dimension of data quality.

High data accuracy can power factually correct reporting and trusted business outcomes. Accuracy is very critical for highly regulated industries such as healthcare and finance.

### 3.  Consistency

This dimension represents if the same information stored and used at multiple instances matches. It is expressed as the percent of matched values across various records. Data consistency ensures that analytics correctly capture and leverage the value of data.

Consistency is difficult to assess and requires planned testing across multiple data sets. If one enterprise system uses a customer phone number with international code separately, and another system uses prefixed international code, these formatting inconsistencies can be resolved quickly. However, if the underlying information itself is inconsistent, resolving may require verification with another source. For example, if a patient record puts the date of birth as May 1st, and another record shows it as June 1st, you may first need to assess the accuracy of data from both sources. Data consistency is often associated with data accuracy, and any data set scoring high on both will be a high-quality data set.

### 4.  Validity

This dimension signifies that the value attributes are available for aligning with the specific domain or requirement. For example, ZIP codes are valid if they contain the correct characters for the region. In a calendar, months are valid if they match the standard global names. Using business rules is a systematic approach to assess the validity of data.

Any invalid data will affect the completeness of data. You can define rules to ignore or resolve the invalid data for ensuring completeness.

### 5.  Uniqueness

This dimension indicates if it is a single recorded instance in the data set used. Uniqueness is the most critical dimension for ensuring no duplication or overlaps. Data uniqueness is measured against all records within a data set or across data sets. A high uniqueness score assures minimized duplicates or overlaps, building trust in data and analysis.  

Identifying overlaps can help in maintaining uniqueness, while data cleansing and deduplication can remediate the duplicated records. Unique customer profiles go a long way in offensive and defensive strategies for customer engagement. Data uniqueness also improves data governance and speeds up compliance.

### 6.  Integrity

Data journey and transformation across systems can affect its attribute relationships. Integrity indicates that the attributes are maintained correctly, even as data gets stored and used in diverse systems. Data integrity ensures that all enterprise data can be traced and connected.

Data integrity affects relationships. For example, a customer profile includes the customer name and one or more customer addresses. In case one customer address loses its integrity at some stage in the data journey, the related customer profile can become incomplete and invalid.

![[Pasted image 20220830092324.png]]

Data quality checks determine metrics that address both quality and integrity. 

The common data quality checks include:

-   Identifying duplicates or overlaps for uniqueness.  
-   Checking for mandatory fields, null values, and missing values to identify and fix data completeness. 
-   Applying formatting checks for consistency.
-   Using business rules with a range of values or default values and validity.
-   Checking how recent the data is or when it was last updated identifies the recency or freshness of data. 
-   Validating row, column, conformity, and value checks for integrity.

![[Pasted image 20220830092559.png]]

Following are lists of data quality dimensions as defined by the DAMA team. Blue marked are the most commonly used ones.

![[Pasted image 20220830094645.png]]

![[Pasted image 20220830094704.png]]

![[Pasted image 20220830094723.png]]

![[Pasted image 20220830094735.png]]



___
# References
[The 6 Dimensions of Data Quality | Collibra](https://www.collibra.com/us/en/blog/the-6-dimensions-of-data-quality)
[https://www.dama-nl.org/wp-content/uploads/2020/09/DDQ-Dimensions-of-Data-Quality-Research-Paper-version-1.2-d.d.-3-Sept-2020.pdf](https://www.dama-nl.org/wp-content/uploads/2020/09/DDQ-Dimensions-of-Data-Quality-Research-Paper-version-1.2-d.d.-3-Sept-2020.pdf)
[https://5690064.fs1.hubspotusercontent-na1.net/hubfs/5690064/Content Offers (WPs, ebooks, Guides)/EBOOK - Top Ten Quality Metrics You Need to Know/The Top Data Quality Metrics You Need to Know_Databand.pdf](https://5690064.fs1.hubspotusercontent-na1.net/hubfs/5690064/Content%20Offers%20(WPs,%20ebooks,%20Guides)/EBOOK%20-%20Top%20Ten%20Quality%20Metrics%20You%20Need%20to%20Know/The%20Top%20Data%20Quality%20Metrics%20You%20Need%20to%20Know_Databand.pdf)
