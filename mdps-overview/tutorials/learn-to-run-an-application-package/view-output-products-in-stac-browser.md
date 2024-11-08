# View Output Products in STAC Browser

To view the generated science data products from your application, log into the STAC Browser: [STAC Browser](https://www.mdps.mcp.nasa.gov:4443/data/stac\_browser/) ([https://www.mdps.mcp.nasa.gov:4443/data/stac\_browser](https://www.mdps.mcp.nasa.gov:4443/data/stac\_browser))

Follow the steps below:

1. Go to the STAC Browser link referenced above ([https://www.mdps.mcp.nasa.gov:4443/data/stac\_browser](https://www.mdps.mcp.nasa.gov:4443/data/stac\_browser))&#x20;
2. Log in using your Unity account.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXc21gq306IOBmHkOiseRYzaOC9osw_EuVzIf3GANqJ1msqGnr8Mla1mUaZYdN7opJD-5ItbAHNSmNr4-WdAlSPZmjBRsNbU8TyCE-FelR_SRJk32lWLJx2mxkchPMqOmJAVqTeTShoyl2WtxVDykJwMQRvkN-wMppCIzvAK3g?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

3. Once you log in, it will take you to the STAC Browser page shown below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfZ1aBQ1an3AvMmLXhTvOI2H9-C26xEV1ymjh1M3QbS6Tujjn8bygLvoMVNbZQUfTihN2jDTtQ6d-OrlapsOWZZlat0EM3pEBhljZsayfdyZh7VLFQ9qOSF0tG8frNPbcG-dcsrwhgthaP0Y9_cX_uFOreE3B43wLEfu2kiag?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

4. Select the STAC index as shown below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe1Kz4sAS4dB_dZ5FXG1Xayt2tGKZX0BeihnhS_TDVVzmFlTSoGxwZaTnisB3GPi55L_2cNq66ZyuubPRk7bqPHHWKBq9qbuu6JNgJ0J_GDrS2nxnr82abEhbDFok8TVt9YNFjxSi1k7Oi8PtWSMyUnRvnhR14Qqj48AhLa?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

5. A dialog will pop up asking for Authentication.  Paste in your Unity Token.

* NOTE: **Retrieve your Unity Token** by executing the following from a Jupyter notebook—or a Python script—in your environment, then copy the token that is printed out (make sure not to grab extra spaces around it) and paste it into the input box in STAC Browser:

`import requests`

`from unity_sds_client.unity import Unity`

`from unity_sds_client.unity import UnityEnvironments`

`s = Unity(UnityEnvironments.PROD)`

`token = s._session.get_auth().get_token()`

`print("Here's your token: \n" + token)`

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeF5-7eLPAhMVePlwIO3zMBauoh5nklV3rZGfiBnFVo7brOhB762cULJhllqn3bSIAKEWlXE-dbH64Ceax927wcyAUnvbMz7TBhn3HW0QwNrfvcwDtMrdzFnXmlZgp9BJn7pKBizHSvvhh7UcmcmcszQYUNajNhHjLmuYZ_3Q?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption><p>Paste your Unity Token into the input box shown in this screenshot</p></figcaption></figure>

6. After authenticating with your Unity Token, the page will direct you to the following page to search for your Output Product as shown below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfZPgS4nAosNJqsP_TlntseYOBnIjWKsNEGIiEV-NdNGStAfD1723wVwUpYSqnbQz4r_54MFybvxWJ79_ACclbGiH1pEQru4-FUJu4XRv1iM2ScBCAD6XzsVFm_l-tfbtrtydYbJzfuU23vOCR8FfzWpW3NdwePGxtWfldtsA?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

7. Search in the “Unity DS Catalog” for your Collection ID as shown in the figure below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdDU9Vy0qM5_O6UkcG6Mkx6Pl5sZIbRH3MMszskLy4IQyoqQGIxkKHVk36q-Nsycfn4uaD53jE5UH9o_HmANiADcBZS4CrZ9n_0ZCZoWqhJ22i4Sp94erudklTubnONhUYliE3oxIm1RaXut3nhc_5fXKtD_eSOD0qSykoZMQ?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption><p>Search for "unity-tutorial" and you should see the output Collection</p></figcaption></figure>

NOTE: The “Unity DS Catalog” lists the Collection ID defined in the “input file” so you can search for the Collection ID (or a part of the ID, e.g. "tutorial" will still find this Collection) as shown in the figure below.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfLuvTHLh9qHZ9BpzASvlaAP87-vRIIsp3Aa-PqJVJ3p93v5bVDyoXqbIrOlx3wesqpOyUjejmw5-8yQAQjm4IROHguo7BeGrOaDqux55n41neP9Qo6mFtsKp48IdmkTaWrQ2qFhmplj9yLkkk8DZonqL2t1v1vomOZkMm?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

8. Select the Collection by clicking on the Collection ID as shown in the figure below

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcPjPgbj-BX42iYHQImOZBEFPf-9T2D_OGoHCKrX6VapkZthhO0jCQQM9-ReEiaLfr-Rp3cfB1Jx7wQrfobCHhqRnWpO5Aq4liaqwI7qFrdSkAGWfUoQ2qiFsmeN4BkijnJLQiNiDAcvObb38w0ZeK3QC5OtFzsBlb4oXx3?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

9. Select the Item (Output Product) for more details.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfPHoZRlEAGRsb5PdA47wJnD_lGv8iEKzwYsggLaxjaGglEIGJh0E5reLdRzjTZ2oS422DI0lBLBfx3CGgtT1GzMv5g5I5Hy0zpqfwTcfLM5-6jaUJJVfvwlLejCvi53G5YzHM2d3EzEuGsfZWBenp62p_PDyHFfLgSmGPW?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

10. View all of the assets and the Amazon S3 URL for the output product file

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeUA1VYMunjhIW6b3bszqCjZ3CPZ_OqHPGCUA0UDfL0c_cGtp4EFQAj7kQbSr397h3ED6Nkfez70_uMsWHUzeKXw9kQ4uXvqbKrgZp-tIYhUV8DT_99p7cwUqn6_T1KBjfyY844xCmcyyWc6LE8LIKAWq9Vvzt2mDL8b7ElXQ?key=K2x_DkLuOSzQLgkvGhINiA" alt=""><figcaption></figcaption></figure>

## Downloading your Output Data

To download the files that you see with Amazon S3 URLs...
