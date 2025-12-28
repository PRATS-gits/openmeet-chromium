# OpenMeet Recorder: General Description
## Hackathon Name: TechSprint - Leveraging the Power of AI

## Project Name: OpenMeet Recorder - An extension for Recording Google Meet

## Project Features
1) Extension Name: OpenMeet Recorder (alternative to Awesome Screen Recorder & Screenshot)
2) Extension Compatibility: For Chromium-based browsers (Google Chrome, Microsoft Edge and Brave)
3) Extension Working:
- Google Sign-In for Authentication via Firebase
- Extension Pop-up on screen allowing users to manage their connections to Google Drive for storing their WEBM/MP4 screen recordings directly
- User dashboard and management at (https://openmeet.panotech.uk/dashboard) similar to that of (https://www.awesomescreenshot.com/my_items)
- The extension will automatically show a popup once the user/admin/meeting host joins a Google Meet
- The extension will then ask the permission to record (This Tab, Window, Desktop, etc.) which are standard requirements
- The extension will primarily record in WEBM format for better handling of web format
- Once the recording starts, a small control bar will be shown in the bottom-left corner of the tab/windows or desktop
- The control bar will have mic sync option (default) (i.e. if the user/host speaks by turning on his mic in Google Meet the mic sync option will automatically turn on the OpenMeet option of mic sync without needing the user to explicitly turn on the mic option to get his voice recorded in the OpenMeet recording)
- The control bar will have the pause/play button (to stop and resume the recording without needing to create new one), stop button (to stop the recording)
- Once the recording is stopped the user will be given the option to save it in connected Google Drive (default My Drive) option (we'll show a popup of Google Drive window to manage/move/create any folder for the recordings to be saved)
- The extension will default record the meet in 720p with an option of 1080p if needed
- The user dashboard will provide an option to give summary of the Google Meet from the recording using its audio/transcript (irrespective of the language used the summary will be in English)

4) Extension Technology:
- The extension will be using Google Firebase for authentication and user management 
- The Google Cloud Platform will be used for calling various API and Services
- The Google's AI infrastructure services (like Gemini, AI Studio and Vertex AI) might be used for summarization as the audio/transcript may not be always in English and will be a mixture of local dialects and languages as well (i.e. Hindi, Hinglish, Marathi, Indian Dialects, etc.)
- The extension will use a smart compression technology which will be processed locally or in the cloud which ensure that the Google Meet gets recorded in 720p or 1080p smartly in small chunks, concurrently and gets uploaded small parts just like the working of Internet Download Manager (which uses concurrent requests to download large file) (Note: This technology needs a heavy research that how it's going to be implemented as function, pipeline, local utilization or cloud function, etc.)
- The extension dashboard page will be deployed on Cloudflare Pages at (https://openmeet.panotech.uk/dashboard), which can be a React+Vite application with TailwindCSS, ShadcnUI for styling and Cloudflare Workers for backend services.