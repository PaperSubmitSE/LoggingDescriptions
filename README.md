# Characterizing the Natural Language Descriptions in Software Logging Statements 
## (_ASE Paper Submission #217_)

## Code-Log Project List
The repository contains _<code, log>_ pairs that are extracted from 10 Java projects and 7 C# projects. The projects are listed as follows:

| No | Java Projects        | C# Projects            |  
| :------:|:-------------: |:-------------:|
| 01  |ActiveMQ		|Azure SDK	|
| 02  |Ambari		|CoreRT		|
|  03 |Brooklyn		|CoreFX		|
|  04 |Camel      	|Mono		|
|  05 |CloudStack 	|MonoDevelop	|
|  06 |Hadoop    	|Orleans	|
|  07 |Hbase     	|SharpDevelop	|
|  08 |Hive		|		|
| 09  |Synapse		|		|
| 10  |Ignite  		| 		|

Details including project description, version, LOC, # of Logs, etc can be found in the submitted paper

## Data Extraction Pipeline
Each _<code, log>_  pair is extracted from a single function and composed of two parts, the first part is a logging statement with some descriptive texts, while the second part is the corresponding previous at most 10 code lines.

A concrete example is shown as follows:
```java
public	void catchException() {
try {
		operation 1;
		operation 2;

	} catch (Exception1 e1) {
		LOGGER.error("Exception 1 happens", e1);

	} catch (Exception2 e2) {
		LOGGER.error(e2);

	} catch (Exception3 e3) {
		LOGGER.error("Exception 3 happens", e3);
	}
}
```

(To simplify the demonstration, we focus only on former 6 code lines)
Two _<code, log>_ pairs can be generated from the above function, which are:

* **Sample 1:**

Code Text:
<pre>
public void catchexception() {     try {     operation 1;     operation 2;     } catch (exception1 e1) {
</pre>

Logging Description:
<pre>
exception 1 happens
</pre>

* **Sample 2:**

Code Text:
<pre>
operation 2;     } catch (exception1 e1) {     } catch (exception2 e2) {     logger.error(e2);     } catch (exception3 e3) {
</pre>

Logging Description:
<pre>
exception 3 happens
</pre>

Some remarks:
1. All empty lines are skipped;
2. All English characters are converted to their lower cases;
3. In code part, code lines are separeted by tab character;
4. Logging statement "LOGGER.error(e2);" can not produce a pair since it does not contain any descriptive text, only a variable. This kind of statement is treated as a normal code line, see example b, while others with descriptive text will not appear in the code part of any pairs, see example a;
5. The scope of consideration is a single function, so in example a, even its code part contains only 5 (<6) code lines, it will only include code outside the function.
