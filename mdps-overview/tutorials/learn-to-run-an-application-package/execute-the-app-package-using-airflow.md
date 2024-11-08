# Execute the App Package using Airflow

Use the Airflow UI to execute your application package. To do this you will use an Airflow DAG (Directed Acyclic Graph), a process that manages the execution of an application or workflow, that can run any Application Package by specifying the two inputs described below.

NOTE: In the future, each Application Package or workflow (series of Application Packages) will be able to have its own DAG in Airflow with a custom input form.

Follow the steps below:

1. Log into Airflow UI using the link below.

* Link to Airflow UI: Airflow UI&#x20;
* NOTE: There isn’t a Username and Password for each User yet. Ask for the temporary Username and Password.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfEPcxITLMK9Mvm7RHVaO-OK_mNhF--uhJAfXKEbDEdhzg1mnztT_90ApJvH1XNTS_wyteYKEVp_oYL-ZY8s62punDH89LePbFKJ_J1VlgfUwt4gaAuLz597cMzZztyYhkmWy21UGKy6WpRjlcrtOunGWV8N1JpBgzaHplX?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption><p>The login for Airflow is not the same as your Unity login. Please ask a Unity team member for the login.</p></figcaption></figure>

2. Select `cwl_dag` from the Airflow DAG-list screen as shown in the figure below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeS44n-_g_VMHEz7QcfxD1-cfG89uQ-6yjKzriXRtK9XmQArok5-Ao5z8QV1b4QNWo2lRsxT8uT8IwtSf_9QebB4BdzjcKE7NHeu0gpELZJ5sbSs2zEYiSdYUH5rFYems730k5nbdttO4QE5Ne355s7mxsBKrdup4IpNqDJ5g?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption><p>Choose cwl_dag</p></figcaption></figure>

3. In the upper right is a “▶” button to trigger (execute) this DAG as shown in the figure below.

* NOTE: The page for “cwl\_dag” will show a history of runs in a bar-chart at the left, and several informational panels in the middle.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdhcAdeOO4AXhGShc-we9cPQHPR9lVeEqIRBBBgQK9rnvC2fLaIo2Vr-f2nhdlpXpCf2ykZYZ2_DPb97SlEyFXpMCEgiZvQUDjthJ9gObyegbTWWYBo62-jizThojLjvKCOwYSU1LgSbHkqjssIxIBUrY0Hs0dOoOI3FeEs1Q?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption><p>Press the Play button in the upper right</p></figcaption></figure>

4. This will open up the Trigger DAG page with five inputs as shown below as shown below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfJZl-SmQFGQjeFCN7uFpbSDaM1zmiowWBymf6q9BDoiGkQ_1ufE8og4NDOJstv0aKLbSRD0pofPC1c55ktTZHpFyMLbLFWyoHlDAm1rLT9koxG-tmxx4qut52mzxJDLhovZwHGn82KxeTZFPf_SisfYMPA8vLNDJ5VfHoF?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption><p>DAG execution page (Trigger DAG)</p></figcaption></figure>

5. Paste the URL to the “unity-example-application” `workflow.cwl` file into “CWL\_Workflow” field:&#x20;

* NOTE: Review Step 2 under [Optional Steps: Modify the Example Application Package](clone-the-example-application-code.md#optional-steps-modify-the-example-application-package) to find out how to retrieve the URL.
* Here’s the URL for this exercise: `http://awslbdockstorestack-lb-1429770210.us-west-2.elb.amazonaws.com:9998/api/ga4gh/trs/v2/tools/%23workflow%2Fdockstore.org%2Fmike-gangl%2Funity-example-application/versions/5/PLAIN-CWL/descriptor/%2Fworkflow.cwl`

6. Paste either the URL of the “input file” or the JSON string content directly into the “CWL\_Workflow” field:

* Here’s the URL of the “input file” for this exercise: `https://raw.githubusercontent.com/unity-sds/unity-example-application/main/test/ogc_app/unity-example-app.test.template.json`
* See the figure below for example:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXf26QrOSaVGfkGBV_z0naa5jE8DtWZJPmc1pUkqvDb1orOgdgRFQLcEvant8BUtJFQRXMh0CaupsSyBE1OsifdwUTAETIHLpa9O0mH0KiqqiJbFA1qa73ykCmrZShCgnyxWxRfraPWKAP36CsA-Obmz6wV3PP-n6kWIt-3eWQ?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

* If you need to modify the JSON file:&#x20;

1. Copy the [`unity-example-app.test.template.json`](https://raw.githubusercontent.com/unity-sds/unity-example-application/main/test/ogc\_app/unity-example-app.test.template.json) locally
2. replace \<PROJECT> with your project (e.g. emit, asips, unity)
3. replace \<venue> with your venue (e.g. dev, int, ops)
4. copy and paste the new json (with your project, venue substitutions) into the required “CWL workflow parameters” box

* See the figure below for example:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXc6sqzTL-hLe288RHVpF0-F6p5x_C0Q0E6Wb0V9gEpWznmi1iagowdejo-nAtcRSLMRV4XmXoHKtCWczOriuX3SEPHKIR6KObRvtmDIfVET5k3gBuNvx66_XPjtwZ7rcMgy2lLKGzRfJGvRPvwqcYTa8tLDFbiqSHJpXCS7?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

7. Leave the remaining three fields with their default values for this exercise as shown in the figure below (container memory: 4Gi, CPUs: 4, and container storage: 10Gi)

* NOTE: These fields allow User to modify the computing resources available to a Job.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeseQ0JnW9qeBq4cdcwl3oxEbMiwIfxrPI7vL0xpxaX-QZnGabF4VjG-3hwH1jmLS1V2ZIdPPLMfhWo0WIjrveuYCtmgXD8kAkvf1cUcXLiKO3dC9tQ7zYsHQRbqSJn4AP8IE_kckue7NP6McgMJgokRc52LRprBWeyW9dczA?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

8. Press the <mark style="background-color:blue;">Trigger</mark> button to execute a job as shown in figure below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcnUYse22Z5WMXqAwW1PvvThbtZFQp7VnF21BpQDDZqa3yiD7BwopKd2fy0fapIQgQxvZ7HnnjreftShb1FSsLNiZ670ZEt9-w9Zzxs5F7xphDItiq9u59Tcqu3Ermc9mQKeWyPlfQYJsEMbwwkXQ3d53J3nau08IfkSfWJ1Q?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

9. You will return to the DAG detail page and you’ll see a new light-green bar representing the new job as shown in the figure below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeysxNkR_pWkDC-QYPR9lY9PN44YQgVJDMU9Uick2MsHqO-nHUIpnkhBqx-VXEt1iW0RmZjD8-HP8y6MLW22PH8WGz_iP5cFqSiHEV-O1PupOEMQq-3dMfUtOCxxw8zQUMfpyFK52aPDvWjVM0qMqvzgnPKMifKKKmMrgB46w?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

10. If you click the new light-green bar, or the squares under it, you will see the main information panel update with status details on your new job as shown in the figure below.

* NOTE: Wait until the job finishes. If any of the squares turn red, that step has failed. You can see more by going through the panels such as Graph or Audit Log.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcCIFrAV1C-qe8vyH7kmKasS5yZqiGO2YYkoftB-U24tx-hL8gQp1JDGnl87JuRlDqxjd4bqcZkHkBLEY_LOHyFFgmEUWToyN1AAwH9eOVAeILB1h4_KW1kE9GFOTdGbr5ifdm3Ym3RLNobRJE0kh4qU1R4Ol-TLsn73mINwA?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

\
