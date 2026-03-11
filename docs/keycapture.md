# Automation Assist: using Key capture

When enabled, the card captures button presses and Activity changes on your virtual remote and sends a Notification, available in your Home Assistant sidebar, containing YAML to reproduce that button press in:
  - your dashboard (a Lovelace button that triggers the same command)
  - a script / automation action (a ready-to-use service call)

    
## ℹ️ Capture a button press

### 1. In the card's configuration editor: **Automation Assist > enable Key capture**
<img height="300" alt="image" src="https://github.com/user-attachments/assets/81ecb091-edf9-40f9-b147-ac4698f9accb" />



### 2. Exit EditMode and press buttons or change Activities in the card. Notifications will appear in the Home Assistant sidebar.
<img height="100" alt="image" src="https://github.com/user-attachments/assets/a38546ce-2ff6-49ca-99d2-e0791f0250d4" />



### 3. Copy the generated YAML from the notification
<img height="300" alt="image" src="https://github.com/user-attachments/assets/ab3f8b35-e118-4936-9863-324796026339" />



## ℹ️ Create a button from the generated YAML

### 1. Enable the dashboard's Edit Mode and click Add card
<img height="300" alt="image" src="https://github.com/user-attachments/assets/cfce05c1-bf80-47aa-848d-41ee8bf8ee3c" />

### 2. In the card selection menu, select Manual
<img height="200" alt="image" src="https://github.com/user-attachments/assets/c2a06172-7a9c-426e-be90-861eaf900405" />

### 3. In the card's configuration editor, replace whatever's there with the code copied from the Notification
<img height="200" alt="image" src="https://github.com/user-attachments/assets/7ed2fc9a-6bdc-4c50-86af-0baed8d2d749" />

### 4. Click "Show visual editor" for further customization
<img height="200" alt="image" src="https://github.com/user-attachments/assets/2d06b38d-37f3-4e2f-9b38-d474e024a427" />


