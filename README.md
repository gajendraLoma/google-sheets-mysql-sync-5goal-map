Google Sheets to MySQL Sync
This is a Node.js application that syncs data from a Google Sheet to a MySQL database and updates it automatically using a cron job. The project includes API endpoints to manage the sync process and fetch data.
google sheet
https://docs.google.com/spreadsheets/d/1Bm-AACFJj7d1Ekl48oiX81zgdfUmUIbhydulz9V8kvw/edit?gid=0#gid=0
https://docs.google.com/spreadsheets/d/1Ua0GVJsemKFQh5MjpH7LHvB4tFplA-9OV9Hu0Yed6oI/edit?gid=0#gid=0
Overview
Purpose: Syncs race track data (e.g., raceTrackID, name_vi, name_en, turns, quant, laps, length) from a Google Sheet to a MySQL map table.
Technology: Built with Node.js, Express, Google APIs, and MySQL2.
Automation: Runs every 4 hours via a cron job.
Prerequisites
Node.js (v14 or higher recommended)
MySQL server
Google Cloud credentials (Service Account JSON file)
AaPanel or similar for cron job setup
Setup Instructions
1. Install Dependencies
Clone the repository:
bash

Collapse

Wrap

Run

Copy
git clone https://github.com/gajendraLoma/google-sheets-mysql-sync-5goal-map
cd google-sheets-mysql-sync
Install required packages:
bash

Collapse

Wrap

Run

Copy
npm install
2. Configure Google Sheets API
Create a project in Google Cloud Console.
Enable "Google Sheets API" and "Google Drive API".
Create a Service Account, download the credentials.json file, and place it in the project root.
Share your Google Sheet with the Service Account email.
3. Set Up MySQL Database
Create a database (e.g., 5goal_wordpress) and a map table:
sql

Collapse

Wrap

Copy
CREATE TABLE map (
    id INT AUTO_INCREMENT PRIMARY KEY,
    raceTrackID VARCHAR(50),
    name_vi VARCHAR(255),
    name_en VARCHAR(255),
    turns INT,
    quant VARCHAR(50),
    laps INT,
    length FLOAT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Update index.js with your MySQL credentials (host, user, password, database).
4. Run the Application
Start the server:
bash

Collapse

Wrap

Run

Copy
node index.js
The server will run on http://localhost:3000.
Usage
API Endpoints
Sync Data:
URL: http://localhost:3000/maps/sync
Method: POST
Description: Fetches data from the Google Sheet and syncs it to the MySQL database.
Fetch Data:
URL: http://localhost:3000/maps/all
Method: GET
Description: Retrieves all data from the map table.
Test with Postman
Send a POST request to /maps/sync to sync data.
Send a GET request to /maps/all to view the synced data.
Cron Job Setup
Log in to AaPanel.
Navigate to "Scheduled Task" or "Cronjob Manager".
Add a new task:
Name: Sync Map Data
Command: curl -X POST http://localhost:3000/maps/sync
Timing: 0 */4 * * * (every 4 hours)
Enable the task and save.
Verify the job runs (e.g., check the database after 08:00 PM +07 today).
Files
index.js: Main application file with API endpoints.
credentials.json: Google Service Account credentials (add to .gitignore).
.gitignore: Excludes sensitive files like credentials.json.
