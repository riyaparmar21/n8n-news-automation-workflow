# 🗞️ AI & Tech News Daily Digest (n8n Automation)

An automated, AI-powered news aggregator built with [n8n](https://n8n.io/). This workflow acts as a ruthless "Senior Tech Editor," scraping raw data from Twitter and top tech RSS feeds, filtering out the noise, and using GPT-4o to write a clean, deduplicated daily summary delivered straight to Slack.

## ✨ Features
* **Multi-Source Aggregation:** Pulls the latest news from premium RSS feeds (TechCrunch, The Verge, Ars Technica) and a custom Twitter search via RapidAPI.
* **Smart 24-Hour Filtering:** Automatically drops any articles or tweets older than 24 hours to ensure the digest is always fresh.
* **Advanced Data Normalization:** Deeply parses and cleans messy JSON data from the Twitter API, mapping it to perfectly match the RSS feed structure.
* **AI-Powered Curation (GPT-4o):** Uses an LLM to evaluate the combined news pool, discard off-topic stories (e.g., sports, biology), merge duplicate reports into a single bullet point, and write concise 2-3 sentence summaries.
* **Automated Delivery:** Runs on a strict daily cron schedule (10:00 AM) and formats the final digest in Markdown for a beautifully readable Slack message.

## 🛠️ How it Works (The Workflow)
1. **Trigger:** A cron node wakes the workflow up daily at 10:00 AM.
2. **Fetch & Filter:**
   * **Twitter Path:** Searches for highly engaged AI/Tech tweets from the last 24 hours. Splits the raw JSON array, filters out API "cursors," and extracts the pure text, author, and link.
   * **RSS Path:** Loops through an array of target URLs, downloads the feeds, and runs a Date/Time filter to drop items older than 1 day.
3. **Merge & Aggregate:** Both data streams are combined into a standardized format and bundled into a single JSON list.
4. **AI Processing:** Azure OpenAI (GPT-4o) analyzes the aggregated list based on a strict system prompt to ruthlessly edit, deduplicate, and summarize the news.
5. **Publish:** The final formatted text is pushed to a designated Slack channel.

## 🚀 Prerequisites
To import and run this workflow, you will need:
* A running instance of **n8n** (Local Docker, VPS, or n8n Cloud).
* **Azure OpenAI API Credentials** (or swap the node for standard OpenAI/Anthropic).
* **Slack App Credentials** (with permissions to post to channels).
* **RapidAPI Key** (specifically subscribed to the `twitter241` API endpoint).

## 📦 Installation & Setup
1. Clone this repository or download the `AI & Computer Engineering News Digest.json` file.
2. Open your n8n workspace, go to **Workflows**, and click **Import from File**.
3. Reconnect your credentials:
   * Double-click the **Fetch Twitter (RapidAPI)2** node and input your RapidAPI key in the headers.
   * Double-click the **Azure OpenAI Chat Model2** node and connect your LLM credentials.
   * Double-click the **Post to Slack2** node and select your target Slack channel and workspace.
4. (Optional) Edit the **Code in JavaScript** node to add or remove specific RSS feed URLs.
5. Toggle the workflow to **Active**!

## 💡 Notes
* **Customizing the AI:** You can adjust the "Ruthlessness" of the news filter by editing the System Prompt inside the `News summerizer1` node.
* **Self-Hosting:** If running n8n locally via Docker, ensure your machine is awake and connected to the internet at the scheduled trigger time, or move your deployment to an always-on VPS.
