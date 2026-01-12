---

title: "Agentic AI Assistant for Performing Log Analysis Locally"
date: "2026-01-08 00:00:00"
categories: [Log Analysis, Agentic AI Assistant for Performing Log Analysis Locally]
tags: [Log Analysis]

---

# Overview

Agentic AI assistant helps the SOC Analyst to perform alert triage, investigation, and Log analysis locally. It can also be integrated with SIEM tools like Splunk, and vulnerability management tools like Nessus scanner. In this project, I built the Interactive Agentic AI assistant to perform log analysis locally by uploading the logs directly to the AI assistant, similar to ChatGPT web interface for log analysis. The following are the tools that I used to build the Agentic AI Assistant.

# Tools used to build the Agentic AI Assistant for performing Log Analysis


1. Windows PowerShell tool.<br>
2. Ollama Open Source tool.<br>  
3. Geema3 12b-it-qat LLM.<br>
4. Docker Desktop application.<br>
5. Open WebUI.<br>

With all the above tools, the most important tools that are required to create the Agentic AI assistant are Ollama and Gemma3 LLM. I explained how I used all the tools together to build an Agentic AI assistant.

# Ollama Open Source tool

Ollama is a lightweight open source tool and a framework that is used to run different types of LLM's (Large Language Models) which can be used by developers, analysts, and individuals who want to use AI capabilities for coading, content-creating, data analysis while ensuring privacy, offline access, and costs saving. Because of its offline nature, it is also very popular among SOC Analysts to perform offline log analysis and to integrate it with different security tools. It works both from CLI and also has a GUI.

# Gemma3 12b-it-qat LLM (Large Language Model)

Gemma3 12b-it-qat is a powerful open-weight large language model from Google released in March 2025. It provides "12b" 12 billion parameters that provides the balance of power to perform complex tasks without requiring high-end hardware to run. It is popular among SOC Analysts because of its efficiency and decision-making capabilities, and it is mainly use for alert triage, complex log analysis, and integration with different security tools. Each version of the Geema3 has its own use cases, and it depends on the developers or individual needs. In this project I used the Gemma3 12b-it-qat version that is widely use by SOC Analysts.

# Steps for Building Agentic AI Assistant for Log Analysis

In these steps below I show how I used all the tools that I mentioned above to build Agentic AI assistant to perform log analysis.

# 1. Downloading, Installing and Running Ollama locally using Windows PowerShell

In the first step, I downloaded the Ollama installer for Windows OS from "https://ollama.com/download". After downloading the Ollama tool. I installed Ollama on my local machine. Then I used Windows PowerShell to run Ollama on my computer. I inputted the first command "ollama" from PowerShell, which has the path specified to my local machine's main drive. The command runs successfully and starts Ollama and showed other useful commands to perform further actions.

Command to run Ollama from Windows PowerShell:

```s
    ollama
```

Download and install Ollama: 
![Ollama down](assets/img/AgenticAIassistant/Ollama_down.jpg)

Starting Ollama with the command "ollama" from Windows PowerShell:
![Ollama down](assets/img/AgenticAIassistant/Ollama_CLI_strtt.png)


# 2. Installing and running Gemma3 12b-it-qat LLM using Ollama

After installing Ollama, I required the LLM model to run from Ollama to download the Gemma3 12b-it-qat model. First, I changed the model installation location to another drive on my local machine from the Ollama because the Gemma3 12b-it-qat LLM model requires 8gb of storage space. I went again to the Ollama URL "https://ollama.com/" and from there I clicked on the Models tab and searched for the Gemma3 12b-it-qat LLM model. I clicked on the model and then clicked on the link "https://ollama.com/library/gemma3:12b-it-qat" under "Readme" that directed me to the Gemma3 12b-it-qat model from there I first checked the correct Geema3 12b-it-aqt model version and verified the size of the LLM model, and from there I copied the ollama command "ollama run gemma3:12b-it-qat" that is used to run and install the Gemma3 12b-it-qat LLM model from Ollama. From Ollama in PowerShell, I ran this command, and it takes some time to download and install the Gemma3 12b-it-qat model because of its large size. I finally installed the Gemma3 12b-it-qat model using Ollama in PowerShell. To verify the model in Ollama, I used the command "ollama list", which shows all the lists of installed models. I would like to notify that there are other Gemma3 LLMs versions available that require less storage space. It all depends on individual use cases.  

Commands to run Geema3 12b-it-qat using Ollama in Windows PoweShell:
```s
   ollama run gemma3:12b-it-qat
```

Command to see the lists of installed LLM models in Ollama:
```s
   ollama list
```

Models tab to download different LLM models from Ollama: 
![Ollama mod](assets/img/AgenticAIassistant/Ollama_models.jpg)

Gemma3 12b-it-qat LLM model:
![Gemma3 12bmod](assets/img/AgenticAIassistant/gemma3_12b-it-qat.jpg)

Commands "ollama run" and "ollama list" to check and install Gemma3 12b-it-qat LLM model:
![Ollama runGemma3cmd](assets/img/AgenticAIassistant/Ollama_run_cli_strt_model.png)

# 3. Testing different prompts in Gemma3 12b-it-qat LLM using Ollama from Windows PowerShell

To start giving prompts to Gemma3 12b-it-qat LLM model, I first checked that and confirmed that I was offline. I inputted the first prompt, "Hello there!" and got the response from Gemma3 12b-it-qat "Hi there! How can I help you today? I also tested it with different prompts and was able to get the responses from Gemma3 12b-it-qat. I would want to run this model in an Internet browser which provides me with a GUI interface like ChatGPT and allows me to upload data like log files for analysis. I decided to use Open WebUI, which provides me with a WebUI interface similar to ChatGPT.

First prompt "hello there!" in Gemma3 12b-it-qat using Ollama from Windows PowerShell:
![Ollama Gemma3hellocmd](assets/img/AgenticAIassistant/resp_hello_gemma3_llm.png)

# 4. Using the Docker Desktop container from Ollama to run Open WebUI in the Browser

To use the Open WebUI in the browser, I needed to install Docker Desktop, which is use to run the Open WebUI Docker container. I downloaded and installed Docker Desktop from "https://www.docker.com/products/docker-desktop/". After successfully installing the Docker Desktop on my local machine then I went to the Open WebUI GitHub web page "https://github.com/open-webui/open-webui" and copied the installation command under "If Ollama is on your computer". This command is use to fetch the Open WebUI docker container and run the Open WebUI in the browser.

Ollama Gemma3 12b-it-qat LLM is already running, I opened another Windows PowerShell window and from there I pasted and ran the above commands to fetch and run the Open WebUI docker container. Once the Docker run command has finished, it opens the Docker Desktop application, which contains the Open WebUI container. From there, I clicked under the ports "3000:8080" that which redirected me to the link "https://localhost:3000" which took me to the Open WebUI in my default browser. As Ollama Gemma3 12b-it-qat is already running Open WebUI picked up the Gemma3 12b-it-qat LLM model from the local API's and provides Gemma3 12b-it-qat interactive Web UI from the internet browser similar to ChatGPT.

Command to fetch and run the Open WebUI docker container:
```s
     docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

Downloading and Installing Docker Desktop Application:
![Down Docker](assets/img/AgenticAIassistant/down_dock_desktop.png)

Copy the Docker run command from the GitHub web page to run the Open WebUI docker container:
![Git DockOpenweb](assets/img/AgenticAIassistant/openWeb_UI_doc_git_command.jpg)

Using the Docker run command to run the Open WebUI docker container from the Windows PowerShell:  
![Cpydocker runcmd](assets/img/AgenticAIassistant/docker_run_opeweb_ui.png)

Open WebUI container on Docker Desktop Application:
![Docker runcmd](assets/img/AgenticAIassistant/Open_WebUI_run_dock_desk.png)

Successfully running Gemm3-12b-it-qat web interface using the Open WebUI docker container: 
![Succ Gemm3webui](assets/img/AgenticAIassistant/succes_launch_gemma3_llm_in_OpenWebUI.png)

# 5. Interacting with Gemma3 12-it-qat LLM in Open WebUI and Perform Log analysis

After successfully running Gemma3 12b-it-qat LLM from Open WebUI in my internet browser. I inputted the first testing prompt: "Hello Gemma3, will you explain the CIA triad of Cybersecurity in three lines?" which required very little critical thinking to manage the response in three lines. I again make sure that I am ofline and the LLM model is running locally. I got the response from Gemma3 12b-it-qat, which was able to manage and explain the CIA triad in three lines.

Gemma3-12b-it-qat first prompt in Open WebUI:
![Gemma3 Openwebcmd](assets/img/AgenticAIassistant/Geema3_fst_resp_off.png)

# 5a. Analyzing HTTP traffic Logs using Gemma3 12b-it-qat

To perform the HTTP traffic log analysis, I first uploaded the sample "http_packets.pcap" file with the input prompt: "Will you please analyze and find the type of attack in this "Http_packets.pcap file" which consists of many HTTP GET request packets and I would like to know is there any type of malicious traffic pattern and a type of attack from Gemma3 12b-it-qat LLM model. In my first attempt at uploading the file with the prompt, I figured out it was not able to analyze the file because of the ".pcap" file format. I further figured out that the Gemma3 12b-it-qat is able to analyze the ".pcap" files that are converted into ".csv", ".JSON" and ".txt" formats. 

I then converted the "http_packets.pcap" file into "http_packets.txt" from the Wireshark, and then re-uploaded the file "http_packets.txt" with the input prompt "Will you please analyze and find the type of attack in this "Http_packets.txt file which is converted into .txt file." The input prompt with the uploaded "http_packets.txt" take some time to process by Gemma3 12b-it-qat LLM and finaly I got the response which specified that it was not able to identify the attack type, but some patterns suggest that it could be "reconnaissance or scanning activity" with three important findings "HTTP GET Request", "HTTP 200 OK Responses" and Repetitive Traffic between IPs". 


Analyzing the http traffic log file "http_packets.pcap" converted to "http_packets.http" in Gemma3 12b-it-qat:
![Gemma3 httplog](assets/img/AgenticAIassistant/http_flod_reco_pcap_analys.png)


# Conclusion

In this project, I learned how to use the Gemma3 12b-it-qat LLM model using Ollama open-source tool from Windows PowerShell and then use docker desktop containers to run Open WebUI which provides an interactive web interface similar to ChatGPT to perform log analysis tasks locally. I would also like to share that Ollama is an open-source tool with useful LLM models like Geema3 and Misteral AI. It can also integrate with SIEM tools like Splunk with Splunk AI toolkit as the bridge that uses the Splunk REST API with Ollama Chat API. It can also integrate with vulnerability management tools like Nesus by using "MCP" Model Context Protocol server that acts as a translator between Nessus REST API and Ollama API.
