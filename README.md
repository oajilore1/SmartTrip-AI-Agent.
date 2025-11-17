Team Members
- Owen Eller– Project Manager / Lead
- Olive Ajilore – Agent Developer
- Brielle Picard – API & Data Integrator

✈️ AI Flight Planner: SmartTrip 

The AI Flight Planner is a simple website that helps you find the best flights. It uses Google Flights data (SerpApi) and AI (Gemini) managed by an automation tool (n8n) to quickly find and summarize the best 3 one-way flights based on where and when you want to travel.

The main problem we solve is that normal flight data is too big and confusing for most people to look through.

💡 What This Project Does

Problem: We fix data overload. Raw flight searches give you huge lists of data. We give you a quick, clear answer instead of a data dump.

AI Used: Gemini (AI): It takes the big flight data, filters it, and writes a simple, structured summary (the "Top 3") following specific rules (like listing the price and flight number).

Tools:

SerpApi: Gets the real-time Google Flights data.

Gemini API: Does the data analysis and summarizing.

n8n: This is the tool that connects the website, the flight data, and the AI all together.

Automation: The system runs itself. You type your trip details, and the n8n flow automatically gets the data, filters the JSON, and asks the AI to write the final list.

Goals (Success Metrics): We aim for the whole search to take under 4 seconds, for the AI to follow the clear list format over 95% of the time, and for the AI output to be very small (less than 5% of the original data).

Roles: (Self-Reflect and Fill in the names of the users collaborating on this project.)

🏛️ How It Works (Simple View)

The system has three parts connected by n8n: The website, the data source, and the AI.

Why We Chose These Tools

n8n Automation: We chose n8n because it connects APIs quickly without needing to write a lot of complex backend code.

SerpApi: We use this for stable and structured access to Google Flights data. It's much safer than trying to get the data directly. We force it to be a one-way search (type=2) to prevent errors.

Website (HTML/JS): It's simple and portable. The whole design is in one file (index.html) using Tailwind CSS.

JSON Filtering (Code Node): This is key. It sends only the necessary flight data to the AI. This makes the search much faster and more reliable.

🛠️ How to Set It Up

This project needs the front-end file and your active n8n workflow.

What You Need

n8n: A running n8n server.

SerpApi Key: A valid API key for getting the flight data.

Gemini API Key: An API key for the AI (managed in n8n).

Step 1: Connect the Website (index.html)

In index.html, find the line const N8N_WEBHOOK_URL.

Replace the placeholder URL with the live Production Webhook URL from your n8n workflow.

Step 2: Set Up the n8n Workflow

The flow uses four main parts:

A. Webhook Trigger Node

Method: POST

Activation: Set the workflow to ACTIVE mode so it keeps listening for new searches.

B. HTTP Request Node (SerpApi)

You must pass the user's origin, destination, and date. The key settings are:

engine: Google Flights

Crucial: type: 2 (Forces One Way search).

sort_by: 2 (Sorts results by Price).

C. Code Node (Data Filter)

Job: Takes the huge SerpApi JSON and creates a small, clean object with only the flights_data, price_insights, and airports keys.

Result: This small object is passed to the AI.

D. Gemini LLM Node (The AI)

System Message: Tells the AI to analyze the data and output a readable list with only the top 3 flight details.

User Prompt: Must correctly package the data from the filter node:

{{ JSON.stringify({ other_flights: $json.other_flights, price_insights: $json.price_insights, airports: $json.airports }) }}




Step 3: Use the App

Open index.html.

Enter IATA codes (like LAX, JFK) for the airports and the date.

Click "Search Flights."

The final, simple list appears in the chat.

🧑‍💻 Extending the System

The modular design makes it easy to change things:

Change Data Source: Swap out the HTTP Request Node for a different flight API.

Change Filter Logic: Edit the Code Node to filter flights by things like maximum stops or preferred airlines.

Add Round Trips: The current flow is type=2. To add round trips, update the HTML to ask for a return date, and change the n8n node to use type=1.
