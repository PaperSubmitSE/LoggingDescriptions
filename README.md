# WhatToLog

Each pair is extracted from a single function and composed of two parts, the first part is a logging statement with some descriptive texts, while the second part is the corresponding previous at most 10 code lines.

A concrete example is shown as follows (to simplify demonstration, we focus only on former 6 code lines):

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

Two code-log pairs can be generated from the above function, which are:

a)  
public void catchexception() {	try {	operation 1;	operation 2;	}	catch (exception1 e1) {  
exception 1 happens

b)  
operation 2;	} catch (exception1 e1) {	} catch (exception2 e2) {	logger.error(e2);	} catch (exception3 e3) {  
exception 3 happens

Some remarks:
1. All empty lines are skipped;
2. All English characters are converted to their lower cases;
3. In code part, code lines are separeted by tab character;
4. Logging statement "LOGGER.error(e2);" can not produce a pair since it does not contain any descriptive text, only a variable. This kind of statement is treated as a normal code line, see example b, while others with descriptive text will not appear in the code part of any pairs, see example a;
5. The scope of consideration is a single function, so in example a, even its code part contains only 5 (<6) code lines, it will only include code outside the function.
