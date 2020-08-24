# Run locally

This document shows how to run the `watson-catbot-simple` application on your local machine.

## Steps

1. [Clone the repo](#1-clone-the-repo)
2. [Create IBM Cloud services](#2-create-ibm-cloud-services)
3. [Get Connection Information of Watson Assistant](#3-get-connection-information-of-watson-assistant)
4. [Configure Watson Assistant](#4-configure-watson-assistant)
5. [Get Skill ID of Watson Assistant](#5-get-skill-id-of-watson-assistant)
6. [Modify .env file](#6-modify-env-file)
7. [Run the application locally](#7-run-the-application-locally)
8. [Test](#8-test)

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

!["sample app"](images/app01.png)

> **Note:** The context JSON data appears in the window on the right. This can be extremely helpful for debugging.

### 8. Test

#### Order #1

1. You start orderiing by entering `I like to have a big mac please`.

1. The chatbot replies `Thank you for your ordering. Is this for dine in or to go?`

1. You finish the order by entering `to go`.

1. The chatbot completes the order and replies `Enjoy your big mac. We are preparing your to-go order`.

In this order transaction, the chatbot understood that you want to order a burger, so it chose the dialog path of ordering burger and collect dining location information.

In the use case, you did not provide additional inforrmation when you made your order `I like to have a big mac please`. The chatbot had to follow the correct dialog path and collect necessary information to complete the order. In this case, it asked `Is this for dine in or to go?`

The JSON object has the following information.

```
{
  "input": {
    "text": "to go"
  },
  "context": {
    "conversation_id": "691107c5-b308-45f6-9435-7851e60d6d9b",
    "system": {
      "initialized": true,
      "dialog_stack": [
        {
          "dialog_node": "slot_18_1597242570911",
          "state": "in_progress"
        }
      ],
      "dialog_turn_counter": 2,
      "dialog_request_counter": 2,
      "_node_output_map": {
        "Welcome": [
          0
        ],
        "node_8_1494705793073": [
          0
        ],
        "handler_20_1597242570911": [
          0
        ]
      }
    },
    "mac_burger": "big mac"
  }
}

Watson understands

{
  "intents": [
    {
      "intent": "order",
      "confidence": 0.5011244297027588
    }
  ],
  "entities": [
    {
      "entity": "mac_place",
      "location": [
        0,
        5
      ],
      "value": "to-go",
      "confidence": 1
    }
  ],
  "input": {
    "text": "to go"
  },
  "output": {
    "generic": [
      {
        "response_type": "text",
        "text": "Enjoy your big mac. We are preparing your  to-go order"
      },
      {
        "response_type": "text",
        "text": "......"
      },
      {
        "response_type": "text",
        "text": "Welcome to basic burger chatbot demonstration, you can order burgers, drinks and shakes from menu. Ask for Help if needed."
      }
    ],
    "text": [
      "Enjoy your big mac. We are preparing your  to-go order",
      "......",
      "Welcome to basic burger chatbot demonstration, you can order burgers, drinks and shakes from menu. Ask for Help if needed."
    ],
    "nodes_visited": [
      "slot_18_1597242570911",
      "handler_19_1597242570911",
      "response_22_1597242570911",
      "node_1_1597242570861",
      "Reset",
      "Welcome"
    ],
    "log_messages": []
  },
  "context": {
    "conversation_id": "691107c5-b308-45f6-9435-7851e60d6d9b",
    "system": {
      "initialized": true,
      "dialog_stack": [
        {
          "dialog_node": "root"
        }
      ],
      "dialog_turn_counter": 3,
      "dialog_request_counter": 3,
      "_node_output_map": {
        "Welcome": [
          0
        ],
        "node_8_1494705793073": [
          0
        ],
        "handler_20_1597242570911": [
          0
        ],
        "response_22_1597242570911": [
          0
        ],
        "Reset": [
          0
        ]
      },
      "branch_exited": true,
      "branch_exited_reason": "completed"
    },
    "mac_burger": null,
    "mac_place": null,
    "mac_size": null,
    "mac_type": null,
    "mac_shake": null,
    "mac_beverage": null,
    "mac_mcflurry_size": null,
    "mac_mcflurry_flavor": null
  }
}
```

The context variables 

```
    "mac_burger": null,
    "mac_place": null,
    "mac_size": null,
    "mac_type": null,
    "mac_shake": null,
    "mac_beverage": null,
    "mac_mcflurry_size": null,
    "mac_mcflurry_flavor": null
```

should have correct information at each step. They are all nulled at this time because when the order #1 completes, the chatbot clears itss context. Right before the clearance, `mac_burger` variable should have content `big mac`, variable `mac_place` should have content `to go`.

#### Order #2

1. You start orderiing by entering `I like to have one big mac to go`.

1. The chatbot replies `Thank you for your ordering. Enjoy your big mac. We are preparing your to-go order`.

1. And, the order is completed.

This example illustrates how `Natural Language Understanding` and `Natural Language Process` extract information from casual conversation. In this order transaction, the chatbot understood that you want to order a burger, so it chose the dialog path of ordering burger. It also understood that this is a to-go order. So, it did not collect the dining locatiion inforrmation as it did in order #1.

Because all required iinformation to complete a burger order were provided when you made the purchase, the chatbot diid not collect further information before it completes the order #2.

The JSON object confirms it.

```
{
  "intents": [
    {
      "intent": "order",
      "confidence": 0.9388424396514894
    }
  ],
  "entities": [
    {
      "entity": "category",
      "location": [
        19,
        26
      ],
      "value": "burger",
      "confidence": 1
    },
    {
      "entity": "burger",
      "location": [
        19,
        26
      ],
      "value": "big mac",
      "confidence": 1
    },
    {
      "entity": "mac_place",
      "location": [
        27,
        32
      ],
      "value": "to-go",
      "confidence": 1
    }
  ],
  "input": {
    "text": "I like to have one big mac to go"
  },
  "output": {
    "generic": [
      {
        "response_type": "text",
        "text": "Thank you for your ordering."
      },
      {
        "response_type": "text",
        "text": "Enjoy your big mac. We are preparing your  to-go order"
      },
      {
        "response_type": "text",
        "text": "......"
      },
      {
        "response_type": "text",
        "text": "Welcome to basic burger chatbot demonstration, you can order burgers, drinks and shakes from menu. Ask for Help if needed."
      }
    ],
    "text": [
      "Thank you for your ordering.",
      "Enjoy your big mac. We are preparing your  to-go order",
      "......",
      "Welcome to basic burger chatbot demonstration, you can order burgers, drinks and shakes from menu. Ask for Help if needed."
    ],
    "nodes_visited": [
      "mac ordering",
      "node_8_1494705793073",
      "node_1_1597242570861",
      "handler_5_1597242570910",
      "handler_19_1597242570911",
      "response_22_1597242570911",
      "node_1_1597242570861",
      "Reset",
      "Welcome"
    ],
    "warning": "DialogNode: No valid slots specified for dialog node ID [mac ordering]. The node cannot gather information. (and there is 1 more warning in the log)",
    "log_messages": [
      {
        "level": "warn",
        "msg": "DialogNode: No valid slots specified for dialog node ID [mac ordering]. The node cannot gather information.",
        "node_id": "mac ordering",
        "node_title": "Order"
      },
      {
        "level": "warn",
        "msg": "Invalid input event handler is specified for dialog node ID [mac ordering]. The node cannot gather information.",
        "node_id": "mac ordering",
        "node_title": "Order"
      }
    ]
  },
  "context": {
    "conversation_id": "691107c5-b308-45f6-9435-7851e60d6d9b",
    "system": {
      "initialized": true,
      "dialog_stack": [
        {
          "dialog_node": "root"
        }
      ],
      "dialog_turn_counter": 4,
      "dialog_request_counter": 4,
      "_node_output_map": {
        "Welcome": [
          0
        ],
        "node_8_1494705793073": [
          0
        ],
        "response_22_1597242570911": [
          0
        ],
        "Reset": [
          0
        ]
      },
      "branch_exited": true,
      "branch_exited_reason": "completed"
    },
    "mac_burger": null,
    "mac_place": null,
    "mac_size": null,
    "mac_type": null,
    "mac_shake": null,
    "mac_beverage": null,
    "mac_mcflurry_size": null,
    "mac_mcflurry_flavor": null
  }
}
```

The Watson chatbot understood `order` is your `intent`. It also extracted the information of entity `category`, `burger type`, `dining-place`(mac_place).


[![return](https://raw.githubusercontent.com/IBM/pattern-utils/master/deploy-buttons/return.png)](https://github.com/IBM/watson-assistant-slots-intro#deployment-options)
