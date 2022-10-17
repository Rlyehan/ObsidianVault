222022-10-13

Style: 

Domain: #Data

Tags: #AWS 

# Data Quality Testing with AWS Deequ

**Analysis Runners:** Here you can mention which analysis you want to run on the column, The result will be given in terms of a data frame.

An Example:
```scala
import com.amazon.deequ.analyzers.runners.{AnalysisRunner, AnalyzerContext}

import com.amazon.deequ.analyzers.runners.AnalyzerContext.successMetricsAsDataFrame

import com.amazon.deequ.analyzers.{Compliance, Correlation, Size, Completeness, Mean, ApproxCountDistinct}

import com.amazon.deequ.profiles.{ColumnProfile, ColumnProfilerRunner, ColumnProfiles}

import com.amazon.deequ.repository.{MetricsRepository, ResultKey}

import com.amazon.deequ.analyzers._

import com.amazon.deequ.metrics._

import org.apache.spark.sql.types._

val df = spark.read.format("csv").option("header", "false").load("dbfs:/databricks-datasets/airlines/")

val dataset = df.limit(1000).select("_c1", "_c2", "_c3", "_c4", "_c5", "_c6")

.withColumn("_c1", '_c1.cast("int"))

.withColumn("_c2", '_c2.cast("int"))

.withColumn("_c3", '_c3.cast("int"))

.withColumn("_c4", '_c4.cast("int"))

.withColumn("_c5", '_c5.cast("int"))

.cache()

/*

Implementation of Analysers

*/

val analysisResult: AnalyzerContext = { AnalysisRunner

// data to run the analysis on

.onData(dataset)

// define analyzers that compute metrics

.addAnalyzer(Size())

.addAnalyzer(Completeness("_c1"))

.addAnalyzer(ApproxCountDistinct("_c1"))

.addAnalyzer(Compliance("_c3", "_c3 != null"))

.addAnalyzer(Correlation("_c4", "_c5"))

.run()

}
```

**Column profiler :**

Here you pass the data frame and the profiler will send you some stats for each column :

How can I use the column profiler :
```scala
val result = ColumnProfilerRunner()

.onData(dataset)

.run()

result.profiles.foreach { case (colName, profile) =>

println(s"Column '$colName':\n " +

s"\tcompleteness: ${profile.completeness}\n" +

s"\tapproximate number of distinct values: ${profile.approximateNumDistinctValues}\n" +

s"\tdatatype: ${profile.dataType}\n")

}

result.profiles.foreach { case (colName, profile) =>

println(s"Column '$colName':\n " +

s"\tcompleteness: ${profile.completeness}\n" +

s"\tapproximate number of distinct values: ${profile.approximateNumDistinctValues}\n" +

s"\tdatatype: ${profile.dataType}\n")

}
```

Normally we would need to costumize the profiler, that would look like this:

```scala
def customAnalysersForColumnStat(schema: StructType): Seq[Analyzer[_, Metric[_]]] = {

/*

Via this method we will customize the generic profilers based on schema as well as column name .

*/

schema.fields.flatMap{

field =>

val name = field.name

if (field.dataType == IntegerType) {

Seq(Completeness(name),

ApproxCountDistinct(name),

CountDistinct(name)

)

}

else {

Seq(Completeness(name),

ApproxCountDistinct(name),

Compliance(s"${name}:empty_field", s"${name} == ''"),

MaxLength(name),

MinLength(name)

)

}

}

}

/*

Here we will pass the whole dataframe schema and then get the analysers in a group of sequences

*/

val customAnalysers = customAnalysersForColumnStat(dataset.schema)

val analysisResult: AnalyzerContext = { AnalysisRunner

// data to run the analysis on

.onData(dataset)

.addAnalyzers(customAnalysers)

.run()

}

// Converting the metrics to a dataframe

val metrics = successMetricsAsDataFrame(spark, analysisResult)

metrics.show()
```



___
# References
[Data Quality testing with AWS Deequ | by Milan Sahu | Medium](https://medium.com/@sahu.milan1988/data-quality-testing-with-aws-deequ-53b6f765de08)