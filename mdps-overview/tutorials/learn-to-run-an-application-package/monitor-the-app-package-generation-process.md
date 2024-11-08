# Monitor the App Package Generation Process

View your Application Package in Dockstore to confirm that it was generated correctly. There is additional detail on how to use Dockstore in Gitbook in the [Exploring the Application Catalog](../exploring-the-application-catalog.md) section.

1. Go to Dockstore and you will see the webpage as shown in the figure below

* URL to Dockstore: [http://awslbdockstorestack-lb-1429770210.us-west-2.elb.amazonaws.com:9998/search?entryType=workflows\&searchMode=files](http://awslbdockstorestack-lb-1429770210.us-west-2.elb.amazonaws.com:9998/search?entryType=workflows\&searchMode=files)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfnZx7uxWOAqfKdecEPlxYmrEJgcV26Pa_AWuQSbiozPNPdPeisnW4sv0AQYIElrI-aIMAkfmPUnnRSOt71FacqMkwrOiq4LEuU85N7lY4HF20dpB1i6adYIZFwk2NniDxtcn61IJE1gaQ6078qiZGGmIeyV0wC0ryjfv_9?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

2. Search for your Application Package by either browsing the list of Available Workflows or using the Search box as shown below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfob7Yz53ljEigI5OfN7qbja4KSfJiYduwkM5SFPnidL3f97VO6ygYHyd02J2DoNvhHwN86TPWcFIHTJmZZj8gR4TAoK_QpW3nDBtLPM1YAf-PnwVYX0bMJE3Kgwv7vuajCjqxMYDKXlm_tgSUUoH7xzZ-TWTn3QwxEfYRkSw?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

3. To check the status of the Application-package Generation, look at the Versions tab, as shown below. Currently there is no status-indicator that an Application Package is being generated; when it is done you should see a new Version of the Application Package show up in the list with a timestamp from a few minutes ago.

<figure><img src="../../../.gitbook/assets/Screenshot 2024-11-07 at 4.57.59 PM.png" alt=""><figcaption></figcaption></figure>

4. Open up the details of your Application Package by clicking on the “Files” tab.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeMxT-nnRjiXQdtqnypE9bEhn9IYjTcPvVIvn7JwXRdPfBzM6wYwEJOhHSsQQPnZR2SEGS5E6C3KmjMIX45bVA-wh-wI5lqji2OOsInuZqbcbCCc-PDiQBQXEyrYw7k5Fgtds3fsqC7JslceClPmS-dqSBWCwI7v_TeilYXXA?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

This will help explain what is in an OGC Application Package:

* `Dockstore.cwl`: This is a boilerplate file needed for Dockstore.It tells the system what the main workflow execution file is&#x20;
  * In this example, under the “step” section with the keyword “run”, the “`workflow.cwl`” will get executed.&#x20;
* `process.cwl`: This tells the system what the underlying executable is, plus inputs and outputs. In this case, it points at the `process.ipynb` [noted in the "Clone the Example Application Code" section](clone-the-example-application-code.md#optional-steps-modify-the-example-application-package).
* `stage_in.cwl` and `stage_out.cwl`: These are created by the Unity system to manage input- and output-data management.
* `workflow.cwl`: This CWL file is the wrapper that captures the `process.cwl`, `stage_in.cwl`, and `stage_out.cwl` to be executed by Airflow. When executing the application within MDPS, as in this Tutorial, you will need to reference “`workflow.cwl`” as an input explained in a later section ([Execute the App Package in SPS using Airflow](execute-the-app-package-using-airflow.md)).&#x20;

\
