---
published: true
title: Reflection on GOKb Data Transformation Project
layout: post
---

## Introduction

This post is a self-reflection on a project I've been working on for the past seven months. I'll give some context around the project, detail the work I did, and reflect on ways that I can improve my data assessment + debugging workflow.

## About the Project

The [Global Open Knowledgebase](http://gokb.org) (GOKb) project aims to create a free, open repository of electronic resources metadata. To facilitate this, GOKb transforms metadata about e-resources, including titles, packages, holdings, and organizations, into linked data. The linked data portion of this project began before I was employed by NCSU Libraries. By the time I jumped into the project there was already a data model and data transformation in place.

Here's a diagram of the data transformation process. I'll mostly be talking about the portion in green.

![alt Data Transformation Pipeline](../assets/images/dataTransformationWorkflow.png)

## Detailing the Work

While perusing the triplestore I noticed that the data was sometimes inconsistent or missing and portions of the data model were outdated. Based on this I was tasked to review the data and data model using best practices. Here are some examples of what I found:

- Data missing
- Portions of the data model were from ontologies that were defunct in some form
- Values expected to be resources were actually literals (for example, `owl:sameAs` expects a resource (URI) to be the value)

All of this went into a report that I presented to the project team. We sat down and made some decisions based on the report. Based on those decisions I was tasked to revise the script that transforms the OAI data into GOKb linked data. This script is written in [Groovy](http://www.groovy-lang.org/). Both the data model and data transformation is written in the script.

I went about this work in multiple phases. In phase one I revised the part of the script that formed the data model and applied the data model to the data, which was harder than it seemed. This was due to multiple reasons; here are the primary reasons:

- The script originally did not remove  triples from the triplestore before attempting to add new triples. This meant additional coding.
- The GOKb site and OAI feeds were being actively developed and migrated, which meant there were occasionally roadblocks in the data transformation pipeline.

In the second phase I revised the part of the script that was transforming the data. Some of this was fairly straightforward (ex. turning literal values into resources), while others were not-so-straightforward. For example, I needed to somehow connect Titles, Packages, Platforms, and TIPPs. I had to figure out how to model that from nested XML to RDF, using a Java-based scripting language.

## Post-project Analysis

Looking back this project was somewhat exhausting because of the amount of work I had to put into it. However, I learned a lot about Groovy and re-modeling XML data as RDF data.

**What I Would've Done Differently**

Something that was not apparent to me when I did the data assessment, something that I later realized when revising the script, was the fact that there were two different reasons why some data was not being transformed.

-  There were stop-gaps in the script code that kept data from transforming (i.e. typos in the code)
-  There were data in the OAI feed that were not being targeted in the script

The former was simple: remove the stop-gap, and the data transforms. The latter was trickier, because in addition to the data not being targeted in the script, there were no properties/classes in the data model to represent these data points. This was a problem, because we had already made all of our data modeling decisions. Here's the workflow I had taken to complete my portion of the project:

1. Review the linked data model based on data present in the triplestore
2. Review the linked data based on data present in the triplestore
3. Write report detailing recommendations to update the OAI -> linked data transformation
4. Make final decisions on changes
5. Revise the part of the script that formed the data model and applied the properties/classes to the data
6. Revise the part of the script that was transforming the data

The flaw in my workflow was me not looking at the script before writing up the report. Reflecting on this I would revise my workflow to be something like:

1. Review the linked data model based on data present in the triplestore
2. Review the linked data based on data present in the triplestore
3. *Review the script to see what was going on*
  - This would've hopefully been the point at which I realized there were two reasons why data was not being transformed, and that there were data points that were not being accounted for in the data model
4. Write report detailing recommendations, etc

## Conclusion

In all, I am glad I had the opportunity to put my linked data knowledge into practice. The linked data portion of the project is currently being finalized, and I'll share the results (and some code) when they're ready. Thanks for reading!
