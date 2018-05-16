---
layout: post
title: Welcome to my Blog
---

Hello folks,
Thanks for stopping by. You will find random musings, war stories and everything in between encountered in my technology avatar life.
Hope you will find wisdom in these lines and wishing you all the best.
regards,
cloudhermes



## Making SOAP API calls via action scripts fom VMware vRO 7.4

//** Declare Soap host and get operation, in the original workflow this is set as an input or even attribute. **

var soaphost = Server.findForType("SOAP:Host" , "xx-xx-xx-xx");
if (soaphost == null) throw "SoapHost 'xx-xx-xx-xx' not found!";

var operation = soaphost.getOperation("GetAttributeSelectionsByName");
if (operation == null) throw "Operation 'GetAttributeSelectionsByName' not found!";

//* Next format the SOAP request parameters like headers and parameters
//*I copied the code from existing workflow to to do this. You will need to adjust or declare extra attributes or variables as there are no output parameters and attributes in a scripting action. 

//** invokeSoapOperation**

actionResult = System.getModule("com.vmware.library.soap").invokeSOAPOperation(operation,header,parameter,attribute) ;

//process the result. please note if you read the original action script"("com.vmware.library.soap").invokeSOAPOperation" it returns a property with 2 hashes in it. SOAP_OUT_HEADERS and SOAP_OUT_PARAMETERS

To return or extract the SOAP_OUT_PARAMETERS I used the below code.

var retArray = [];

for (var k in actionResult.SOAP_OUT_PARAMETERS){
	if(actionResult.SOAP_OUT_PARAMETERS.hasOwnProperty(k)){
		retArray.push(actionResult.SOAP_OUT_PARAMETERS[k]);
		}
	} 
return retArray;
