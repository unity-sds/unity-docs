# Clone the Example Application Code

This is an optional section that will let you practice _editing_ an application before registering it with the Unity system. If you wish to skip this step, you may simply use the example application in the [next step](submit-the-app-package-to-the-application-package-generator.md).

In order to create a new Application Package for the Tutorial, you first need to create a copy of the Tutorial application, which we will do by forking the Example Application. Then we can build and register your Application using the Unity tools, and view it in the Application Catalog (Dockstore).

Here is a handy reference to Github commands, in case you are not familiar with it, or don’t use it often: [https://education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)

1. Go to the Github repository for the example application: [Unity-Example-Application](https://github.com/unity-sds/unity-example-application) ([https://github.com/unity-sds/unity-example-application](https://github.com/unity-sds/unity-example-application))
2. Select “Fork” on the top right of the screen as shown in the figure below:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdJaBsykl_FBQE-ZgHHYldqWQjy59LTrnyO1w8L0Z0TD4qtE3e6r3rRXNJOJNnBYYjljVdDzRfeL1jili97YdHjan5wQptw2vPEqAw0N8E33ND1cEOqd-Y__odYuXqPY8xwKOTH2WclJ2qCesBcRrYnomL2n7YvR92uiAxD4Q?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

3. Make sure to fill in the “Owner” dropdown, “Repository name” field, “Description” field and check the “Copy the main branch only” as shown in the figure below:\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcsi3yQiks8vj0nb3pE7-DwsvegmxbplSjFH2aIS4MoR4ViE_5qNGTV7keOyBHAuphh69ywrJnbh3NJ4p9USVSRmLXHbn-3mDEUJtvba6XVH3TB6Ve2jdkqOTsC78NhEYtccDS5n0eaInQEUwNi84qQBeDuFlhEFKD6p-tM?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

4.  Select “Create fork” as shown in the figure below:\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXenCJ2bezCIyC2H2L8QSPoHVFYozWc65g9FwMrF2goMb27HLJBByRoPB4bycBRJ5hpuMHe4mh_Srg-aiONxE41ioJJu4qlnW1voEwaOori8MhfL0oFjz7qnc5ZP5e7VSUmKC4XdczUuVdzij0gGK8xlgbNJCFaSD6VrgJ3pCw?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>
5. Select “Code” and copy the https link as shown in the figure below:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeoAhd8EKff-99U0xU6ItjpC5dsBUX07Ww9r-ITs6VOhHmRPxKkXWUguaUF3gYP-eldtXyyHNE1Vm2BdFpn5btaWrkkCrLlhAushw97CznymgPvTRGX07TEZgl2W51WfVGEqP44IhabPiayibZpj2e44Y8x9KFqY_uDwQsaKg?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

6. Go to your [JupyterHub session](log-in-to-jupyter-hub.md).&#x20;
7. Select the <mark style="background-color:blue;">+</mark> symbol on the top left corner, which represents the “Launcher”, and select “Terminal” from the Launcher tab as shown below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXevtP4fZslZlV0zLfXe1bwuHukYZBfbReKzniUGnhpph_Qjq9uGHXpBbS27Zrgh9lxODItZwB9WPDGnKBChzfsN5rzOKOFwN5nfL0W5VJggJ9Kz-vEEVF0OSOAJodw6aKq3VjriDiy-NH_eindR79Cjrk-Ypvtyp4glwUBH?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

8. From the Terminal: Clone the git repository using the https URL you copied from the Github interface (Step 5 above) as shown in the figure below:

* Ex: `git clone https://github.com/unity-sds/unity-example-application.git`

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe4uGzzX5d__lK8x3VikHtW3ISgwB7ndZ_-Q4neHq3Zl8E_ok1Wlf7fi43WoIm-YyJ5AVooKdXmt0un6cFYLOJ99DjUnfK2FkfxWSfPS7sNtOJK2qYgeFN4-_RJB8St-qcX2NZSP7lWQqGg4DNtejNvz2-SEL6CGNtYhPQu5A?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

NOTE: You should see the cloned files in the Jupyter file browser as shown in the figure below. You can also see these files by going into the folder and listing in the Terminal (`cd unity-example-application`; `ls`) as shown in the figure below:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBGEkX9LPSBQZ2SdIByBFUCuTCBa4oTYXUDa_zNihd-pjVLYxsHsBoaM1O-6D9nLBSRGst60ynO5nlKk5CROZYnBHJ1qh9nhbUvyXonccIiyKSvRf4ghE-5cvyNphZJJG9yHNfaVOUp6YcAl-56RTb41rpbab_qv1n3Rf9Gg?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

NOTE: Information about the files that are in the `unity-example-application` folder:

* `execute_app_pack_gen.ipynb`: this is a Jupyter notebook that runs Unity commands to prepare the demo application to run at scale on the Unity platform. It will package the demo application into an OGC Application Package.
* `process.ipynb`: this is the example application that executes when the App Package is run.
* `requirements.txt`: this contains a list of the libraries needed to run the example application; these are packaged into the App Package so that it has all the necessary dependencies. Each application will have a similar file so that the runtime environment is properly set up when running at scale in the SPS (Airflow).
* `test/ogc_app/unity-tutorial-app.test.yml`: this is an example input file for the tutorial application.

## Optional Steps: Modify the Example Application Package

If you would like to edit the example application to better understand how the system works, you will need to do the following. Otherwise, skip to the next section: [Submit the example application for building](submit-the-app-package-to-the-application-package-generator.md).

Follow the steps below:

1. From the Jupyter terminal: set the environment needed to execute the application as shown in the figure below

* Run the following command: `pip install --user -r requirements.txt`

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd-DQGkQMbo0gj-ymJiUOGRsd1v9RtkKpU3g_FHPdd-v1kFBznzhQX2MAuUHpibQS8qmA87rUAXkiMDCW1cwTVCiZf5mwn4Ta_BgaAH90yTgzT9AZw7hWRpaeJRYWd9Eohf2VzDf-nNPjY5tPJwUOMINCQ0WikcZ03KFj3KyQ?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

2. Open `process.ipynb` by double-clicking the file in the Jupyter file browser as shown in the figure below. This is where you can make changes to the core algorithm.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfa7yZFlstnsJSFAtCliyXewc-fVubkRunLw92eXziy2PuAGA8nv9B4CEjpdgMgzUjvrul5LWtHcCEiuv6bPmaWZ_51NskgR0IiWpiJhnWaMZOsQmbbJaa-vxZdFtRvH_VoAOl_8PxTUF8rOYaquAbF0NX0TPk4jyaDMIdHxg?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

3. Once you have made some changes, Commit them to the Git repository with the commands below.&#x20;

* Note that in order to authenticate a push to Github, you will need to use your github.com username and your Personal Access Token (classic)—_not_ your github password. See the help on github.com for more information ([Managing your personal access tokens - GitHub Docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic))
* In the Jupyter Terminal:&#x20;
  * `cd </path/to/your/unity-tutorial-application directory>`
  * `git add .`
  * `git commit -m "your_commit_message"`
  * `git push -u -f origin main`

Now your changes are pushed into the Github repo, and are ready to be built into an Application Package.
