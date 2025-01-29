# Submit the App Package to the Application Package Generator

This section will utilize the App-Pack-Gen API for building your application to generate Docker image(s) and CWL files. This process takes the code from a public Github repository (in this case, your fork of the tutorial application) and packages it into an OGC Application Package (more info at the OGC web site: [OGC Standard](https://docs.ogc.org/bp/20-089r1.html#toc16))—a standardized container for an executable algorithm—that can be executed at scale in the Unity SPS (Science Processing Service). The OGC Application Package will be put into DockerHub and Dockstore, the Application Catalog used by Unity.

#### **What happens when I run App-Pack-Gen?**

Two main things happen when app-pack-gen is run.

1. An executable container is  built around your code that can run across multiple platforms and has standardized interfaces.
2. CWL files are being generated to describe your application and how it should be executed, including automatically generated data-input and data-output management steps.

* More details can be found in the [Packaging an Algorithm Tutorial](../algorithms-and-algorithm-development/packaging-an-algorithm.md).

#### **What is CWL?**

CWL (Common Workflow Language) is a syntax to instruct a series of execution steps to occur. Typically there is a series of steps described by `workflow.cwl`. For our purposes, this file sets up the interface to the Application Package (i.e., input parameters).

### Follow the Steps Below

1. Navigate to the folder containing your cloned repository and add the `execute_app_pack_gen.ipynb`file from the link below:

{% file src="../../../.gitbook/assets/execute_app_pack_gen.ipynb" %}

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdFCvNMuQHYogOiIG0ginKN9KPZYzW-e8xSJRpaNRqNt91HEUtNfSYjM94TYnlLNcmAszh_gtopkuI0RTbTam0Jd8q6_Wbh3I48hdgiGZD47LBRmDDg9FB5_uyf7Q1yQnEITnlcDjrxLQUgh3BVoOyNL_cgXYb4mIh4O8fBxw?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

2. Open the `execute_app_pack_gen.ipynb` notebook in the unity-example-application folder by double-clicking it in the file-browser panel as shown in the figure below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXftj4r8aL1ylBt7XMSn98Ya9qtcEWcb2FQtv9u0dttNu9uKEmTBR5DmLkBvD0XDoVTSwSm8v0SkB1KKStHM_ZvkzixvW8KzYsSOLuW_n3HRgSUk3-C_tXrFbetleVmSaYpS1Rxj8tqFDl_i5CSlxy_5_bT6E0nfl0yrlcuG?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

3. In the notebook, update the `clone_url` variable to your clone Github repository URL as shown in the figure below. If you did not make changes, or prefer to use the original demo application, you can leave the `clone_url` alone.

* NOTE: The URL would be the forked Github repository from the previous section: [Clone the Example Application Code](clone-the-example-application-code.md).
* If you want to use the original application URL, it is in that notebook by default: [https://github.com/unity-sds/unity-example-application](https://github.com/unity-sds/unity-example-application)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeHqtPIOgl8gXT-V0lI6ReItn4uTX3dBokjCV6d9K5N4AjHdoRvLt6VOjg51SpclgznO2M9ygV-VZYq_KODxH73WRpG0EMHC3o_FlhpHRi1WZ7Nzfx9tb8tFAMD688gmg4eG8KZGQdHBdtBlDbGW0b6H-ChctBSMBVtdud6?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

4. Step through the Python notebook

* You “step through” by executing each cell, starting at the top. You can press the “▶” button at the top of the editor window, or press shift-enter. As a cell executes you will notice the \[number] on the left turn into an asterisk \[\*], indicating that the cell is being executed.
* Enter your Unity username and password when prompted as shown in the figure below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfo-LzCwUSzxl72xIgPPtuMkkWxfxhriAMzHQOS62tYDDhPCVoW84ZB6Rbq3aTa_y57k4kxfk_roNOKqb13e22FJCecNEf0dKPAGJbMebNC164Ndx9v4QPBsn41X4XKJshP6gJqZ9-XW_hyLftYWXeGcbYQCzRESvbruBZPvg?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

* NOTE: A `response.status_code` of 200 and the `response.content` reiterating the `clone_url` will be shown if the submissions was successful as shown in the figure below. This does not necessarily mean that the Application Package Generation will be successful, only that your request was successfully submitted to the service.
  * If you do not see this, please contact a Unity MDPS team member for help.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdOjFp-Du8x4A9S3pZKj5q9_vCbShxBELk_nJmyfi19j_Gl4RKjb_c-y4DDqAk7gonO7YLniR-mC1WeLB78iQlzQtm1uMCoAK68hJO5qz00ZRhT557UIdpbjIQ_Hxpem_BHIQmLsTHa8ixxpTDN_Is4Ogq6B4TE7bEpPWQgJg?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

At this point you have successfully submitted your code to be packaged into an OGC Application Package by the App-Pack-Gen system.
