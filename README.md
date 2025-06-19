# DevelopGenerativeAISolutionAzureOpenAI
Setup:
```
Windows credentials:
Use these credentials to sign into Windows:

User name: Admin
Password: Pa55w.rd
Azure credentials
Use these credentials to sign into Azure:

Email address: User1-52350883@LODSPRODMCA.onmicrosoft.com
Password: rKnS8h#1B+m0
Azure resources
Use these names when creating resources in Azure

Resource group: ResourceGroup1
Azure AI Foundry Project: project52350883
Azure AI Project Hub: hub52350883
For all other resources, use the default name.
```

0. Login to https://ai.azure.com, create project and deploy gpt-4o model
1. 1. Go to Visual Studio Code Terminal, git clone the repository
```
git clone https://github.com/microsoftlearning/mslearn-ai-studio
```
2. Setup the environment and install python package
```
python -m venv labenv
./labenv/Scripts/Activate.ps1
pip install python-dotenv azure-identity azure-ai-projects azure-ai-inference
```
3. update the python file in chat-app/chat-app.py
```
import os

# Add references
# Add references
from dotenv import load_dotenv
from azure.identity import DefaultAzureCredential

from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

from azure.ai.projects import AIProjectClient
from azure.ai.inference.models import SystemMessage, UserMessage, AssistantMessage


def main(): 

    # Clear the console
    os.system('cls' if os.name=='nt' else 'clear')
        
    try: 
    
        # Get configuration settings 
        load_dotenv()
        project_connection = os.getenv("PROJECT_ENDPOINT")
        model_deployment =  os.getenv("MODEL_DEPLOYMENT")
        
        # Initialize the project 
        # Initialize the project client
        projectClient = AIProjectClient(            
                    credential=DefaultAzureCredential(
                                    exclude_environment_credential=True,
                                                exclude_managed_identity_credential=True
                    ),
                            endpoint=project_connection,
        )
        

        ## Get a chat client
        # Get a chat client
        chat = projectClient.inference.get_chat_completions_client()

        # Initialize prompt with system message
        # Initialize prompt with system message
        prompt=[
                    SystemMessage("You are a helpful AI assistant that answers questions.")
        ]


        # Loop until the user types 'quit'
        while True:
            # Get input text
            input_text = input("Enter the prompt (or type 'quit' to exit): ")
            if input_text.lower() == "quit":
                break
            if len(input_text) == 0:
                print("Please enter a prompt.")
                continue
            
            # Get a chat completion
            # Get a chat completion
            # Get a chat completionprompt.append(UserMessage(input_text))

            prompt.append(UserMessage(input_text))
            
            response = chat.complete(    
                model=model_deployment,
                messages=prompt)

            completion = response.choices[0].message.content

            print(completion)
            prompt.append(AssistantMessage(completion))


    except Exception as ex:
        print(ex)

if __name__ == '__main__': 
    main()
```
4. Run the python
```
python chat-app.py
```
5. 
