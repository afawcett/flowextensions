# FlowProcess 

![Image](https://res.cloudinary.com/hzxejch6p/image/upload/v1524860691/Blog_Graphic_01_v01-02_abpx5x.png)

Manage the execution and interfacing of runtime resolved Flows in Apex
----------------------------------------------------------------------

[![Deploy](https://deploy-to-sfdx.com/dist/assets/images/DeployToSFDX.svg)](https://deploy-to-sfdx.com)

The FlowProcess class wraps the above Apex Flow API and allows you to manage the execution and interfacing of runtime resolved flows in Apex. It focuses on providing support for declaring inputs and outputs as well as integrating with Custom Metadata types as a means to allow admins to control which flows are invoked

```
List<Account> accounts = (List<Account>)
    new FlowProcess().
        named(dynamicFlowName).
        with('SomeRecords', [select Name, Id from Account]).
        returning('FilteredAccounts'));
```

You can also reference a Custom Metadata Type to allow Admins to configure the Flow to run

```
List<Account> accounts = (List<Account>)
    new FlowProcess().
        named('FilterAccounts',
           MyAppFlows__mdt.SObjectType,
           MyAppFlows__mdt.FlowName__c).
        with('SomeRecords', [select Name, Id from Account]).
        returning('FilteredAccounts'));
```

You can also mock Flow invocations during tests.

```
// Given 
FlowProcessRunner mockRunner = 
    (FlowProcessRunner) Test.createStub(FlowProcessRunner.class, new RunnerMock());          
FlowProcess.setMock(mockRunner);
        
// When
List<Account> accounts = (List<Account>) 
    new FlowProcess().named('GetSomeRecords').returning('Records');
            
// Then
System.assertEquals(1, accounts.size());
System.assertEquals('MyAccount', accounts[0].Name);
```

You can read more about this library and Flows in general in this [blog](https://developer.salesforce.com/blogs/2018/04/adding-clicks-not-code-extensibility-to-your-apex-with-lightning-flow.html).
