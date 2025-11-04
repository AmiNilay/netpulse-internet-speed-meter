# NetPulse v2.0: The Redesign Update

This is a major release of NetPulse, featuring a complete UI overhaul, persistent data history, a dynamic status bar icon, and a host of new settings. The app has been redesigned from the ground up to be more modern, intuitive, and feature-rich.

## ✨ What's New

* **Complete UI Redesign:** A modern, card-based interface fully supports both **Light and Dark modes**.
* **Dynamic Status Bar Icon:** The static notification icon has been replaced with a dynamic, text-based icon that shows your **live download speed** directly in the status bar.
* **Data History:** Each data card on the home screen is now clickable, opening a new screen that shows your **last 60 days** of usage history.
* **Expanded Settings:** A new settings screen gives you full control over the app's theme, units, and notifications.
* **Data Limit Reminders:** You can now set data limits for both Mobile and WiFi to receive alerts.
* **Persistent Data:** All data usage is now saved, surviving app restarts and phone reboots.

---

## 🏠 Home Screen

The home screen has been rebuilt with a clean, single-column layout:

* **Header:** Features the "NetPulse" title and a new Settings icon.
* **Date Card:** Displays the current date and time.
* **Speed Cards:** Side-by-side cards show your real-time **Download** (mint/green) and **Upload** (yellow/amber) speeds.
* **Status Card:** Shows your current network type (e.g., "WiFi Connected" or "Mobile Connected") with a green status dot.
* **Data Usage Cards:**
    * **Mobile Data:** Shows daily usage and your set limit (e.g., `5.2 GB / 10 GB`).
    * **WiFi Data:** Shows total daily usage (e.g., `88.7 GB`).
    * **Total Usage:** Shows combined daily usage (e.g., `93.9 GB`).
* **Clickable History:** Tapping the Mobile, WiFi, or Total data cards will take you to their respective history screens.
* **Limit Shortcut:** A new alarm icon on the Mobile/WiFi cards takes you directly to the settings page to set your data limit.
* **Stop Button:** A large, red "Stop Monitoring" button is fixed at the bottom.

## 📊 Data History

Tapping on a data card opens a dedicated history screen with:

* A list of data usage for the **last 60 days**.
* A **"Today"** item at the top that updates in real-time.
* Each past day entry shows the **Date**, **Total Data Used**, and a **Percentage Change** badge (e.g., `+15%` or `-10%`) compared to the previous day.
* An empty state message ("No history data available") if no data has been logged.

## ⚙️ Settings

The new settings screen is organized into clear sections:

### Data Reminder
* **Mobile Data Limit:** Set a usage limit in MB for your mobile data.
* **WiFi Data Limit:** Set a usage limit in MB for your WiFi data.

### Display Settings
* **Theme Selection:** Choose between **Light**, **Dark**, or **Auto (Follow System)**.
* **Speed Units:** Toggle between `bytes per second (B/s)` and `bits per second (bps)`.
* **Hide Lock Screen Notification:** A toggle to hide the notification from appearing on your lock screen for privacy.
* **Show Upload/Download Speed:** A toggle to show or hide the Upload/Download speeds in the notification panel.

### Actions
* **Clear Data:** Resets all data usage statistics and history.
* **Close App:** Stops the monitoring service and completely exits the application.
* **About:** Shows app information, including **Version 2.0**.

## 🛠️ Core Improvements & Fixes

* **Dynamic Notification Icon:** The app now generates a custom bitmap icon on the fly to render the live speed as text, making it visible in the status bar.
* **Data Persistence:** All daily usage is saved to SharedPreferences, ensuring your history and totals are kept even if you restart your phone.
* **Automatic Daily Reset:** Data usage counters automatically reset to **zero at midnight** every day.
* **Auto-Save:** Current usage data is automatically saved to the history log every 60 seconds and when the service stops, ensuring history is always up-to-date.
