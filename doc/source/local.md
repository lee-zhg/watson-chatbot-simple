# Run locally

This document shows how to run the `watson-assistant-slots-intro` application on your local machine.

## Steps

1. [Clone the repo](#1-clone-the-repo)
2. [Create IBM Cloud services](#2-create-ibm-cloud-services)
3. [Configure Watson Assistant](#3-configure-watson-assistant)
4. [Get IBM Cloud credentials and add to .env](#4-get-ibm-cloud-services-credentials-and-add-to-env-file)
5. [Run the application](#5-run-the-application)

### 1. Clone the repo

Clone `watson-catbot-simple` locally. In a terminal, run:

```
$ git clone https://github.com/lee-zhg/watson-catbot-simple.git

$ cd watson-catbot-simple
```

We’ll be using the file [`data/skill-watson-burger-simple.json`](../../data/skill-watson-burger-simple.json) to upload the Assistant Intents, Entities, and Dialog Nodes.

### 2. Create IBM Cloud services

Create the following service and name it `burger-asssistant-service`:

* [**Watson Assistant**](https://cloud.ibm.com/catalog/services/conversation)

### 3. Get Connection Information of Watson Assistant

If your Watson Assistant serrvice is provisioned in IBM Cloud, you'll need its `API key` and `URL` when calling the service.

* Find and open the `Watson Assistant` service in your IBM Cloud Dashboard.
* Write down 
    - API key
    - URL

### 4. Configure Watson Assistant

The following instructions will depend on if you are provisioning Assistant from IBM Cloud or from an IBM Cloud Pak for Data cluster. 

Choose one:

<details><summary>Provision on IBM Cloud</summary>
<p>

* Find and open the `Watson Assistant` service in your IBM Cloud Dashboard.
* Go to the `Manage` tab and then click on `Launch Watson Assistant`.
* Select the `Skills` tab in the left navigation tab.
* Click `Create skill`
* Select the `Dialog skill` option and then click `Next`.
* Go to the `Import skill` tab.
* Click the link `Drag and drop file here or click to select a file`.
* Go to your cloned repo dir, and `Open` file [`data/skill-watson-burger-simple.json`](../../data/skill-watson-burger-simple.json).
* Click `Import`.

</p>
</details>

<details><summary>Provision on IBM Cloud Pak for Data</summary>
<p>

* Find the Assistant service in your list of `Provisioned Instances` in your IBM Cloud Pak for Data Dashboard.
* Click on `View Details` from the options menu associated with your Assistant service.
* Click on `Open Watson Assistant`.
* Go to the `Skills` tab.
* Click `Create skill`
* Select the `Dialog skill` option and then click `Next`.
* Click the `Import skill` tab.
* Click `Choose JSON file`, go to your cloned repo dir, and `Open` the workspace.json file in [`data/watson-pizzeria.json`](../../data/skill-watson-burger-simple.json).
* Select `Everything` and click `Import`.

</p>
</details>

### 5. Get Skill ID of Watson Assistant 

To find the `Skill ID` for Watson Assistant:

* Go back to the `Skills` tab.
* Find the tile for the workspace you would like to use. Look for `watson-burger-simple`.
* Click on the three dots in the upper right-hand corner of the card and select `View API Details`.
* Copy the `Skill ID` GUID.

!["Get Workspace ID"](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-assistant/assistantPostSkillGetID.gif)

* In the next step, you will put this `Workspace ID` into the `.env file as ``WORKSPACE_ID``.

### 6. Modify .env file

* Create `.env` file by copying `env.sample` file.

    ```
    cp env.sample .env
    ```

* Update the `Workspace ID` in the `.env file to the value of ``Skill ID``.

    ```bash
    WORKSPACE_ID=<put skill id here>
    ```

The remaining credentials will depend on if you are provisioning Assistant from IBM Cloud or from an IBM Cloud Pak for Data cluster. 

Choose one:

<details><summary>Provision on IBM Cloud</summary>
<p>

* Use the `apikey` and `url` from your Watson Assistant service credentials in the `.env` file.

!["Assistant Credentials"](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-assistant/watson_assistant_api_key.png)

```bash
# If Assistant service is hosted on IBM Cloud, uncomment and use these variables for IAM Authentication
CONVERSATION_APIKEY=<put assistant  API key here>
CONVERSATION_URL=<put assistant url here>
```

</p>
</details>

<details><summary>Provision on IBM Cloud Pak for Data</summary>
<p>

* Use the `URL` from your Watson Assistant service details to set the `CONVERSATION_URL` value in the `.env` file.

!["CPD Credentials"](images/cpd-assistant-details.png)

```bash
# If Assistant service is hosted on CP4D Cluster, uncomment and use these variables for CP4D Authentication
CONVERSATION_AUTH_TYPE=cp4d
CONVERSATION_AUTH_URL=<put cp4d url here>
CONVERSATION_AUTH_DISABLE_SSL=true
CONVERSATION_USERNAME=<put cp4d username here>
CONVERSATION_PASSWORD=<put cp4d password here>
CONVERSATION_URL=<put assistant url here>
CONVERSATION_DISABLE_SSL=true
```

`CONVERSATION_AUTH_URL`, `CONVERSATION_USERNAME` and `CONVERSATION_PASSWORD` are related to the URL and login credentials for accessing your IBM Cloud Pak for Data cluster.

</p>
</details>

### 7. Run the application locally

To run the app locally,

```bash
npm install
npm start
```

The application will be available in your browser at http://localhost:3000

[![return](https://raw.githubusercontent.com/IBM/pattern-utils/master/deploy-buttons/return.png)](https://github.com/IBM/watson-assistant-slots-intro#deployment-options)
