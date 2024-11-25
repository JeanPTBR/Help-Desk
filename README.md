# <p align="center"> 💻 **API-TTS - HELP DESK**

## **👥 Developers**
| [<img loading="lazy" src="https://avatars.githubusercontent.com/u/167718668?v=4" width="115" alt="Jean Carlos Penha da Conceição">](https://github.com/JeanPTBR) <br>[Jean Carlos Penha da Conceição](https://github.com/JeanPTBR) | [<img loading="lazy" src="https://avatars.githubusercontent.com/u/114765722?v=4" width="115" alt="Silvio Cabral de Melo">](https://github.com/SilvioCMJ) <br>[Silvio Cabral de Melo](https://github.com/SilvioCMJ) | [<img loading="lazy" src="https://avatars.githubusercontent.com/u/167145673?v=4" width="115" alt="Roger Santos Vargas">](https://github.com/Rogerdev02) <br>[Roger Santos Vargas](https://github.com/Rogerdev02) | [<img loading="lazy" src="https://avatars.githubusercontent.com/u/95103547?v=4" width="115" alt="Monique da Silva Borges">](https://github.com/niqueborges) <br>[Monique da Silva Borges](https://github.com/niqueborges) |
|:---:|:---:|:---:|:---:|


---




## **📑 Table of Contents**
- [📈 Project Status](#-project-status)
- [✨ Features](#-features)
- [⚙️ Architecture and Workflow](#-architecture-and-workflow)
- [🤖 Help Desk Chatbot - Details](#-help-desk-chatbot---details)
- [🗃️ Database](#-database)
- [⚙️ Environment Variables](#-environment-variables)
- [📦 How to Run the Application](#-how-to-run-the-application)
- [🚀 Deployment](#-deployment)
- [💻 Technologies Used](#-technologies-used)
- [📂 Directory Structure](#-directory-structure)
- [📐 Standards Used](#-standards-used)
- [📅 Development Methodology](#-development-methodology)
- [😿 Main Challenges](#-main-challenges)
- [📝 License](#-license)

---


## **📈 Project Status**
🚀 **Status**: Completed

This project aims to develop a **Text-to-Speech (TTS) API** using **Amazon Polly** to convert text into audio, store the audio files in **S3**, and manage metadata with **DynamoDB**. Additionally, it integrates with **Amazon Lex v2** and **Slack**, where a chatbot named **Help Desk** captures and processes user messages, returning audio responses related to **Notebook and Desktop hardware issues**.

---

## **✨ Features**
1. **Text-to-Speech Conversion**:
   - The API receives a phrase via POST, generates a unique hash for the phrase, and checks in DynamoDB if the audio has already been created. If not, Polly generates the audio, which is stored in S3.
   
2. **Integration with Amazon Lex Chatbot and Slack**:
   - The **Help Desk** chatbot, integrated with Lex v2, allows Slack users to send queries about **Notebook** and **Desktop** hardware. The bot processes intents, captures relevant slots, and provides responses through **cards** with text and audio returned by the TTS API.

3. **Data Persistence**:
   - Phrases and metadata are stored in **DynamoDB**, while audio files are saved in **S3**.
---



## **⚙️ Architecture and Workflow**
The project's architecture involves the following components:

1. **TTS API**:
POST request example:
   ```json
   {
     "phrase": "Please convert this text to audio"
   }
   ```

   Response example:
   ```json
   {
     "received_phrase": "Please convert this text to audio",
     "url_to_audio": "https://my-s3-bucket/audio-xyz.mp3",
     "created_audio": "2023-09-20 17:00:00",
     "unique_id": "phrase-hash"
   }
   ```
---

## **🤖 Help Desk Chatbot - Details**
The **Help Desk** chatbot was designed to interact with users experiencing Notebook and Desktop hardware-related issues. It provides detailed responses in text and audio, helping users identify and resolve common problems.

- To use our chatbot, follow this link to join the Slack Workspace and select **DeskBotFinal** under **Apps**:
   https://join.slack.com/t/compassuolsede/shared_invite/zt-2r678ikhe-zOvQVfLK0CzbX~BFRurEQg

Below are the chatbot's main features:

### **Intents**:
1. **Greeting**:
   - **Example interaction**: "Hi, I have a problem with my device."
   - **Response**: "Hello! I’m HelpDesk bot, your technical hardware assistant. Describe the issue you’re facing, and I’ll help you resolve it!"

2. **Black Screen Issue**:
   - **Example interaction**: "My computer has a black screen. How can I fix it?"
   - **Response**: "Check the monitor connection: Ensure the monitor cable is properly connected to the video card or motherboard video output."

3. **Overheating Issue**:
   - **Example interaction**: "My computer is overheating. What can I do?"
   - **Response**: "Internal Cleaning: Open the device and remove dust from fans, heat sinks, and components with compressed air. Accumulated dust can block airflow."

4. **Device Not Turning On**:
   - **Example interaction**: "My computer won’t turn on. What might be wrong?"
   - **Response**: "Check the power connection: Ensure the power cable is securely plugged in and the outlet is working."

5. **Slow Performance**:
   - **Example interaction**: "My computer is slow. How can I fix it?"
   - **Response**: "Check storage: Ensure the hard drive (HDD or SSD) isn’t full. Overloaded drives can reduce performance. Consider using an SSD if you’re using an HDD."

6. **Device Restarting**:
   - **Example interaction**: "My computer keeps restarting. How can I resolve this?"
   - **Response**: "RAM: Remove and reseat RAM modules. Test one at a time in different slots. Use tools like MemTest86 to check for RAM errors."
  

### **Slots**:
- **Device_Slot**: Identifies the user’s device.
- **HardwareProblems**: Captures the type of issue.
- **YesNo**: Captures user responses when additional issues exist.

### **Cards**:
- For each response provided, the chatbot returns a **visual card** with:
   - **Black Screen**
   - **Overheating** 
   - **Device Not Turning On**
   - **Slow Performance**
   - **Device Restarting**
  
   **Card Structure Example**:
   ```
   {
     {"text": "Black Screen", "value": "black_screen_issue"},
     {"text": "Overheating", "value": "overheating_issue"},
     {"text": f"{device} Not Turning On", "value": "not_turning_on_issue"},
     {"text": "Slow Performance", "value": "slow_performance_issue"},
     {"text": f"{device} Restarting", "value": "restarting_issue"}
   }
   ```

## **🗃️ Database**
The project uses two main storage services:

- **DynamoDB**: Stores the hash of phrases and the URLs of audio files in S3. Example structure:
  - `unique_id`: Hash generated from the phrase.
  - `phrase`: The original phrase.
  - `audio_url`: URL of the audio file in S3.
  - `timestamp`: Creation date.

- **S3**: Stores audio files generated by Polly.

---

## **⚙️ Environment Variables**
Development variables are automatically configured when running the API.

---

## **📦 How to Run the Application**

### **Prerequisites**:
- **Serverless Framework** installed.
- Correctly configured AWS credentials.

### **Steps**:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/JeanPTBR/Help-Desk.git
   ```

2. **Create the development environment**:

   Windows:
   ```bash
   creation: python -m venv inference
   activation: .\inference\Scripts\activate.bat
   ```

   Linux:
   ```bash
   install python venv: sudo apt install python3-venv
   creation: python -m venv inference
   activation: source inference/bin/activate     
   ```

3. **Install the Serverless Framework**:
   ```bash
   npm install -g serverless
   ```

4. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

5. **Set up environment variables**:
   ```bash
   task config
   ```

6. **Deploy the application**:
   ```bash
   task deploy
   ```

7. **Check the generated endpoints** and use the `/v1/tts` route to transform text into audio.

8. **Test the API locally**:
   ```bash
   task run
   ```

---

## **🚀 Deployment**
The deployment is carried out via the **Serverless Framework**, which configures and manages the required AWS services. This process will create the API in **API Gateway**, the functions in **Lambda**, and configure the **S3** buckets and **DynamoDB** tables.

- **Lambda** for the conversion logic.
- **API Gateway** to expose the routes.
- **Polly** for audio generation.
- **DynamoDB** and **S3** for storing data and files.

To test the endpoint used by the chatbot:
https://rmbshe6lg3.execute-api.us-east-1.amazonaws.com/v1/tts

---

## **💻 Technologies Used**
- **Amazon Polly**: Text-to-Speech service.
- **Amazon Lambda**: Executes code in response to events without server management.
- **Amazon S3**: Scalable and durable storage for audio files.
- **Amazon DynamoDB**: NoSQL database used to store metadata.
- **Amazon Lex v2**: Processes natural language to build conversational chatbots.
- **Slack**: Messaging interface used for chatbot interactions.
- **Python 3.9**: Language used for application development.
- **Serverless Framework**: Orchestrates the deployment of serverless services.
- **Git**: Version control system for tracking changes and collaboration.

---

<div align="center">
      
![Amazon Polly](https://img.shields.io/badge/Amazon%20Polly-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-569A31?style=flat-square&logo=amazonaws&logoColor=white)
![Amazon DynamoDB](https://img.shields.io/badge/Amazon%20DynamoDB-4053D6?style=flat-square&logo=amazonaws&logoColor=white)
![Amazon Lex](https://img.shields.io/badge/Amazon%20Lex-4B8BBE?style=flat-square&logo=amazonaws&logoColor=white)
![Slack](https://img.shields.io/badge/Slack-4A154B?style=flat-square&logo=slack&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Serverless Framework](https://img.shields.io/badge/Serverless%20Framework-23B17B?style=flat-square&logo=serverless&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)
![AWS Lambda](https://img.shields.io/badge/AWS%20Lambda-FF9900?style=flat-square&logo=amazonaws&logoColor=white)

</div>

---

## **📂 Directory Structure**

```bash
Help-Desk/
├── api-tts/                        # Text-to-Speech API directory
│   ├── aws/                        # AWS services used by the API
│   │   ├── dynamodb/               # Interactions with DynamoDB
│   │   ├── polly/                  # Interactions with AWS Polly
│   │   └── s3/                     # Interactions with S3
│   ├── dev/                        # Development-related files
│   │   ├── collections/            # Postman collections for testing
│   │   └── tasks/                  # Scripts and utilities for development
│   ├── handlers/                   # Request handlers for the API
│   ├── utils/                      # Utilities and helper functions
│   └── serverless.yml              # Serverless configuration file
├── assets/                         # Resources such as images and static files
├── lex_chatbot/                    # Directory related to the Lex chatbot
│   ├── DeskBotFinal.zip            # Final chatbot file
│   └── lambda_function.py          # Chatbot's Lambda function
├── package.json                    # Node.js configuration and dependencies
├── pyproject.toml                  # Poetry configuration for Python dependencies
├── requirements.txt                # Python dependencies for the project
├── README.md                       # Main project documentation
├── .env                            # Environment variables file
└── .gitignore                      # Git configuration file
```

---

## **📐 Standards Used**
- **Semantic Commits**: For maintaining a clear and organized history.
- **RESTful API**: For communication between services.
- **Serverless Architecture**: The entire application uses AWS-managed services.
- **Python PEP8**: Adheres to Python code formatting standards.

---

## **📅 Development Methodology**
The development followed an **Agile** methodology with the following practices:

1. **Sprints**: Development cycles with defined goals.
2. **Daily Meetings**: Daily progress tracking meetings.
3. **Code Review**: Regular code reviews to ensure quality and consistency.

---

## **📝 License**

This project is licensed under the [MIT License](LICENSE).

---
