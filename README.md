# Ensuring Quality Releases (Quality Assurance)

Below are the high level steps performed for this project.

- Creation of a resource group and storage account in azure to store a terraform state file.
- Publishing the provided package called FakeRestAPI as an artifact.
- Build the following azure resources using terraform:
	○ Resource group
	○ App service & App service plan
	○ Network interface & Network security group
	○ Public IP address
	○ Linux based Virtual machine, Virtual network and Disc

- Deployment of FakeRestAPI as an azure app service.
- Running of postman/newman data validation tests against the http://dummy.restapiexample.com API 
- Publishing a selenium script (written in python) as an artifact.
- Installing selenium on the virtual machine created via execution of terraform and use it to run functional tests against the https://www.saucedemo.com website
- Perform stress and endurance test using Jmeter and upload the results as artifact.
- Setting up of email based alerting for the app service (manually in azure portal).
- Setting up custom logging in log analytics to gather selenium logs from the VM (manually in azure portal).

Note
1. All required inputs (including the public key for the VM, created via puttygen) is configured as variables in pipelines. 
2. Storagekey variable is kept as a 'placeholder', as it will be updated with the real value when the pipeline runs.
3. Replacetokens task of the pipeline takes care of replacing the variables in terraform files (.tf and .tfvars) during run time.
	
	
Creation of ServicePrincipal (with contributor role) is done with below command:
	```
	az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription ID>"
	```

From the results of the above command, we obtain the values for below variables as

	- appId is the client_id defined above.
	- password is the client_secret defined above.
	- tenant is the tenant_id defined above.


In-order for selenium based tasks to run on VM, we need to manually configure the VM to allow the pipeline to connect to it. In the Environments section of Pipeline (select Linux VM), we need to copy the registration key and execute in the VM (connect via  the private key corresponding to the public key used for creating the VM)



The project is organized into 3 stages. Refer the below screen shots for tasks in each of the stages

	1. Build
	2. Deploy
	3. Post-Deployment
	
	

References

	1. https://www.srijan.net/blog/manual-api-testing-using-postman
	2. https://medium.com/@gabriel.starczewski/jmeter-and-azure-pipelines-55f0594239ac
	3. https://medium.com/swlh/running-jmeter-load-tests-and-publishing-jmeter-report-within-azure-devops-547b4b986361
	4. https://azuredevopslabs.com/labs/vstsextend/selenium/
	5. https://cloudskills.io/blog/terraform-azure-devops
	
