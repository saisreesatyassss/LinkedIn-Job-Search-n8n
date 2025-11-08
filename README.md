{
  "nodes": [
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        288,
        192
      ],
      "id": "6d1fbb67-5813-4846-94fb-9d7b0faccc1c",
      "name": "When clicking ‘Execute workflow’"
    },
    {
      "parameters": {
        "url": "https://saisreesatyassss.github.io/CV/cv.pdf",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.3,
      "position": [
        528,
        144
      ],
      "id": "332dd6db-4e0b-4bff-9537-6241e460db2a",
      "name": "HTTP Request"
    },
    {
      "parameters": {
        "operation": "pdf",
        "options": {}
      },
      "type": "n8n-nodes-base.extractFromFile",
      "typeVersion": 1,
      "position": [
        768,
        144
      ],
      "id": "7bb20535-c7e3-4c6b-b5b4-dfcce5e08dd9",
      "name": "Extract from File"
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo",
          "mode": "list",
          "cachedResultName": "Job search n8n",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Filter",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo/edit#gid=0"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        976,
        144
      ],
      "id": "537209b0-d4ff-49eb-8520-6c2f4b30811a",
      "name": "Get row(s) in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "03UjQVEYq5NBJI53",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "let url = \"https://www.linkedin.com/jobs/search/?f_TPR=r86400\"\n\nconst keyword = $input.first().json.Keyword\nconst location =  $input.first().json.Location\nconst experienceLevel = $input.first().json['Experience Level']\nconst remote = $input.first().json.Remote\nconst jobType = $input.first().json['Job Type']\nconst easyApply = $input.first().json['Easy Apply']\n\nif (keyword != \"\") {\n  url += `&keywords=${keyword}`;\n}\n\nif (location != \"\") {\n  url += `&location=${location}`;\n}\n\nif (experienceLevel !== \"\") {\n  // we have experienceLevel as a string with values like \"Internship, Entry level\"\n  // we  transform it to \"1,2,3\" where:\n  // Internship -> 1\n  // Entry level -> 2\n  // Associate -> 3\n  // Mid-Senior level -> 4\n  // Director -> 5\n  // Executive -> 6\n  const transformedExperiences = experienceLevel\n    .split(\",\")\n    .map((exp) => {\n      switch (exp.trim()) {\n        case \"Internship\":\n          return \"1\";\n        case \"Entry level\":\n          return \"2\";\n        case \"Associate\":\n          return \"3\";\n        case \"Mid-Senior level\":\n          return \"4\";\n        case \"Director\":\n          return \"5\";\n        case \"Executive\":\n          return \"6\";\n        default:\n          return \"\";\n      }\n    })\n    .filter(Boolean); // filter out any empty strings\n  url += `&f_E=${transformedExperiences.join(\",\")}`;\n}\n\nif (remote.length != \"\") {\n  // we have location as a string with values like \"Remote, Hybrid\"\n  // we transform it to \"2,3,1\" where:\n  // On-Site -> 1\n  // Remote -> 2\n  // Hybrid -> 3\n  const transformedRemote = remote\n    .split(\",\")\n    .map((e) => {\n      switch (e.trim()) {\n        case \"Remote\":\n          return \"2\";\n        case \"Hybrid\":\n          return \"3\";\n        case \"On-Site\":\n          return \"1\";\n        default:\n          return \"\";\n      }\n    })\n    .filter(Boolean); // filter out any empty strings\n  url += `&f_WT=${transformedRemote.join(\",\")}`;\n}\n\nif (jobType != \"\") {\n  // we have jobType as a string with values like \"Full-time, Part-time\"\n  // we transform it to \"F,P\" where:\n  // Full-time -> F\n  // Part-time -> P\n  // Contract -> C\n  // Temporary -> T\n  // Other -> O\n  // Internship -> I\n  const transformedJobType = jobType.split(\",\").map((type) => type.trim().charAt(0).toUpperCase());\n  url += `&f_JT=${transformedJobType.join(\",\")}`;\n}\n\nif (easyApply != \"\") {\n  url += \"&f_EA=true\";\n}\n\nreturn {url}"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1184,
        144
      ],
      "id": "82944a71-d76f-4858-ac68-b16aa45ffc14",
      "name": "Code in JavaScript"
    },
    {
      "parameters": {
        "url": "={{ $json.url }}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.3,
      "position": [
        1392,
        144
      ],
      "id": "e9d5cc84-a103-4953-8a97-62c3293e33ba",
      "name": "HTTP Request1"
    },
    {
      "parameters": {
        "operation": "extractHtmlContent",
        "extractionValues": {
          "values": [
            {
              "key": "Links",
              "cssSelector": "ul.jobs-search__results-list li div a[class*=\"base-card\"]",
              "returnValue": "attribute",
              "attribute": "href",
              "returnArray": true
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.html",
      "typeVersion": 1.2,
      "position": [
        1600,
        144
      ],
      "id": "1a589f02-181f-4f8a-a5e4-7e0f7db730dd",
      "name": "HTML"
    },
    {
      "parameters": {
        "fieldToSplitOut": "Links",
        "options": {}
      },
      "type": "n8n-nodes-base.splitOut",
      "typeVersion": 1,
      "position": [
        1808,
        144
      ],
      "id": "554c70a9-8884-4297-9774-ad493b80f552",
      "name": "Split Out"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        2096,
        80
      ],
      "id": "775ec75c-279c-48a1-ad87-7e272e47c0ac",
      "name": "Loop Over Items"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        2320,
        -32
      ],
      "id": "d4ff356e-fadc-4d0f-ba86-46bce539c789",
      "name": "Wait",
      "webhookId": "f0209dc1-4f20-4622-860b-cd2e58b4bccb"
    },
    {
      "parameters": {
        "url": "={{ $json.Links }}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.3,
      "position": [
        2544,
        -96
      ],
      "id": "24e15910-e94e-438b-a260-c4f7ea2d6074",
      "name": "HTTP Request2"
    },
    {
      "parameters": {
        "operation": "extractHtmlContent",
        "extractionValues": {
          "values": [
            {
              "key": "title",
              "cssSelector": "div h1"
            },
            {
              "key": "company",
              "cssSelector": "div span a"
            },
            {
              "key": "location",
              "cssSelector": "div span[class*='topcard__flavor topcard__flavor--bullet']"
            },
            {
              "key": "description",
              "cssSelector": "div.description__text.description__text--rich"
            },
            {
              "key": "jobid",
              "cssSelector": "a[data-item-type='semaphore']",
              "returnValue": "attribute",
              "attribute": "data-semaphore-content-urn"
            }
          ]
        },
        "options": {}
      },
      "id": "93433757-787d-447b-9e78-f9922dac8ff1",
      "name": "Parse Job Attributes",
      "type": "n8n-nodes-base.html",
      "position": [
        2768,
        -80
      ],
      "typeVersion": 1.2
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "f0d4be65-069f-4270-bf32-c3977becb83f",
              "name": "description",
              "value": "={{ $json.description.replaceAll(/\\s+/g, \" \") }}",
              "type": "string"
            },
            {
              "id": "883cd240-47fc-454c-baa3-bdc685e088f8",
              "name": "jobid",
              "value": "={{ $json.jobid.split(\":\").last() }}",
              "type": "string"
            },
            {
              "id": "96aa68c3-b7c7-4d2b-b278-aa86e0bcad56",
              "name": "applyLink",
              "value": "={{ \"https://www.linkedin.com/jobs/view/\"+ $json.jobid.split(\":\").last() }} ",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": true,
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        2976,
        -96
      ],
      "id": "4a9948f6-0f0d-4e12-91a7-55832edecf5d",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=You are an assistant that helps match candidates to job opportunities. Your task is to review my resume, then analyze both the resume and job description to calculate a job matching score. Additionally, create a cover letter based on my resume and the job description. The cover letter should contain at least 2 paragraphs and should exclude the name, address, and signature sections from the beginning and end.\nWhen using special characters like quotation marks (\"), ensure they are properly escaped with a backslash. The output must be formatted as valid JSON that can be parsed without errors.\n\nfor example your response should be like: {\"score\": 80, \"coverLetter\": \"sample cover letter\" }\n\njob_description: {{ $json.description }}\nmy_resume: {{ $('Extract from File').item.json.text }}",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3,
      "position": [
        3152,
        -80
      ],
      "id": "fa2a0f15-0257-4405-9431-b27aa0381a96",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        3088,
        112
      ],
      "id": "4cfbf85a-2108-4b06-bdc9-da3160c65e8b",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "Zb7IOTNkNlvbeKSq",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "mode": "raw",
        "jsonOutput": "={{ $json.output.replaceAll(/```(?:json)?/g, \"\") }}",
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        3504,
        -80
      ],
      "id": "9035ab6e-042b-4a89-9718-c8655dc56a9e",
      "name": "Edit Fields1"
    },
    {
      "parameters": {
        "operation": "appendOrUpdate",
        "documentId": {
          "__rl": true,
          "value": "13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo",
          "mode": "list",
          "cachedResultName": "Job search n8n",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1086742991,
          "mode": "list",
          "cachedResultName": "Results",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo/edit#gid=1086742991"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "link": "={{ $('Edit Fields').item.json.applyLink }}",
            "Title": "={{ $('Edit Fields').item.json.title }}",
            "Company": "={{ $('Edit Fields').item.json.company }}",
            "Location": "={{ $('Edit Fields').item.json.location }}",
            "Score": "={{ $json.score }}",
            "Cover Letter": "={{ $json.coverLetter }}",
            "description": "={{ $('Edit Fields').item.json.description }}"
          },
          "matchingColumns": [
            "link"
          ],
          "schema": [
            {
              "id": "Title",
              "displayName": "Title",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Company",
              "displayName": "Company",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Location",
              "displayName": "Location",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "link",
              "displayName": "link",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Score",
              "displayName": "Score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "description",
              "displayName": "description",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Cover Letter",
              "displayName": "Cover Letter",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        3680,
        -80
      ],
      "id": "6cc0e376-850e-412e-923d-38c0ed9fff61",
      "name": "Append or update row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "03UjQVEYq5NBJI53",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "18f4ce7e-00dc-4901-9f02-3d62e946146f",
              "leftValue": "={{ $json.Score }}",
              "rightValue": 50,
              "operator": {
                "type": "number",
                "operation": "gte"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        3984,
        144
      ],
      "id": "77fa19b9-4dde-429c-9748-7eb931e8d424",
      "name": "If"
    },
    {
      "parameters": {
        "chatId": "6262839163",
        "text": "=Title: {{ $json.Title }}\nCompany: {{ $json.Company }}\nLocation: {{ $json.Location }}\nJob Score:  {{ $json.Score }}\nApply:{{ $json.link }}\nCopy Cover Letter from here:https://docs.google.com/spreadsheets/d/13vk0ZoBnEr9xCodlItO9ShQtWsPjD11fn_61eCfNxjo/edit?usp=sharing",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        4192,
        320
      ],
      "id": "5f68b63f-de85-4b06-875c-71192f26efcd",
      "name": "Send a text message",
      "webhookId": "1c87e679-e872-4b06-8f8d-6ef4c9d01a54",
      "credentials": {
        "telegramApi": {
          "id": "LyHy2emfosBQ8P4E",
          "name": "Telegram account"
        }
      }
    }
  ],
  "connections": {
    "When clicking ‘Execute workflow’": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "Extract from File",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract from File": {
      "main": [
        [
          {
            "node": "Get row(s) in sheet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get row(s) in sheet": {
      "main": [
        [
          {
            "node": "Code in JavaScript",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript": {
      "main": [
        [
          {
            "node": "HTTP Request1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request1": {
      "main": [
        [
          {
            "node": "HTML",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTML": {
      "main": [
        [
          {
            "node": "Split Out",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Split Out": {
      "main": [
        [
          {
            "node": "Loop Over Items",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Loop Over Items": {
      "main": [
        [],
        [
          {
            "node": "Wait",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Wait": {
      "main": [
        [
          {
            "node": "HTTP Request2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    } 
 
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "fa5dc9e898567d680c6ef361e27b57e9bd5e6e19e45564908230146fe3bbd199"
  }
}