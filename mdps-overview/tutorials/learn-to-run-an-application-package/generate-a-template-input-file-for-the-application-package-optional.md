# Generate a template “input file” for the Application Package (Optional)

To run the tutorial application, you need an “input file”,  formatted as a YAML or JSON file, and it defines the set of input parameters and values necessary to execute the application.&#x20;

We have created an input file for unity-example-application, so you can paste this URL into Airflow when running the application: [unity-example-app.test.json](https://raw.githubusercontent.com/unity-sds/unity-example-application/main/test/ogc\_app/unity-example-app.test.yml). Take a look at that file to see what the format looks like.

Skip directly to [Execute the App Package in SPS using Airflow](execute-the-app-package-using-airflow.md) section below if you do not want to generate an “input file”.&#x20;

Otherwise, the information below provides instructions on generating the “input file” necessary to run the application in Airflow.&#x20;

OPTIONALLY you can use `cwltool` to generate a template input file for your application to use as an “input file”. The User will have to generate an "input file" that is associated with the `workflow.cwl`. The "input file" contains all of the parameters and values needed by the application itself to run.&#x20;

Currently, the APG API doesn't automatically generate the "input-file" so User will have to execute the `cwltool` command to generate it manually. The `cwltool` will look at your `workflow.cwl` and generate the input and output parameters that the application is looking for.&#x20;

### Follow the steps below:

1. Install cwltool library in JupyterHub by executing the following command

* `pip install cwlref-runner`

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfvcbE_U407TR0MFfZh8iwyDW0S_LrAi7757qb0UhypxgA7h3k7-js7unBmWcPrRSxEAynyUoZIwvntEvdAH0ozTQONbTuCX2HSng-aoL_xzE1PvstZCFLHWSOYVMYOtoHASzCV8bgMDGrsZ0COvpm31wiyQlmULBpsNmxr?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

2. To run the cwltool, you will need to generate an “input file” template to pass in as an argument

* Command: `cwltool --make-template <URL to your CWL Workflow> > <filename>.yml`

To find the URL to ”workflow.cwl” file:

* Select your application in Dockstore (see [Monitor Docker the App Package Generation Process](monitor-the-app-package-generation-process.md))
* Select the “Files” tab as shown in the figure below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeuj4pUQVCN60xJeRfnLkD-11YmmmXsU682-VCOC3OUkJakYqo0XLxmvbNerx7DrK3ECjwjC-UaUH1TnIIKWs4EJMeUR90Fy2YgCJ_XnD2z2_gOpjuMcVyIlTz2-5BiHe1D_BAGknWyhKn39Ff5CJfxogx4N-8ktPHrobZR?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

Right click the download icon next to “/workflow.cwl” and select “Copy Link Address” to copy the URL as shown in the figure below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcCG_ABtb_ldGiNiTvM9ITxLigFdUc2huw6PzGinmpo_3aEyJQWsh9mYpQtKkRhL5Ts0VrNyIBRkF6bs1aND400pr9DvJx7kn4RTdh3k5mC84WG_WqYpbd9aZUhzUWPnKCOkjhkh8IBtxaURNjtrQRVYenBh-L2NiNOumj6Mg?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

Example command to create an input-file template from the main workflow.cwl file:\
`cwltool --make-template http://awslbdockstorestack-lb-1429770210.us-west-2.elb.amazonaws.com:9998/api/ga4gh/trs/v2/tools/%23workflow%2Fdockstore.org%2Fmike-gangl%2Funity-example-application/versions/5/PLAIN-CWL/descriptor/%2Fworkflow.cwl > unity-example-app.test.template.json`

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfboUL3DWJfmOiaCqE3TLumjEywexjEEi0e4Jo2Rq8AksS4pV3TNpmh8rtggGmEHJb0Oel_G0_Vg0kS-LXYwppKAjTOKvHD5_re8fUsRhL5ZO3DEJQ6sbkWCYO9RhBD8qX6HneUN5ux9E7ufYKGNyYVFw9uL8WMjX8uiHxA9A?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

3. Open unity-example-app.test.template.json to view the output of cwltool. The output will look like this:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeAlb8ggKorzvuFyGdxhA0ko_336-p9XiUyOiLe_1UVN6RPQpbdCaJwQVh4QKnR9FdalSCOafp01hv1AGVEWok3SQK1leMw7BFw8KhnEISEjrin-FvQYcOP7pOHNmyY9PbFQGRnQWbokkuXlxmwhCPSZOUQEYxtKP6dDu4uWw?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

1. Update the unity-example-app.test.template.json with the appropriate values for each field

* Example shown here: [https://raw.githubusercontent.com/unity-sds/unity-example-application/main/test/ogc\_app/unity-example-app.test.json](https://raw.githubusercontent.com/unity-sds/unity-example-application/main/test/ogc\_app/unity-example-app.test.yml)&#x20;

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe7Cfjbr7axJzKAyxNyNgoM5D_yOcCBU-fxi53SZGAc5mxDpoeCNmWknckoS1hNpLm698XT-EEGxzNI-85N7LFCVtGwvge9VEjASzLNtpRZc2ujlcQNoAGtC6NxXSVchP9XG12GGsG5buRRwNimb97aX5sGPh0ITGuzkBQW?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

NOTE: For “collection\_id” and “output\_collection” fields, the recommendation is to follow this format: `urn:nasa:unity:::name___version`
