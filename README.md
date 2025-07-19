
# 📬 Notification System

This project implements a **Push Notification System** using:

* **Expo** for sending push notifications
* **Firebase Cloud Messaging (FCM)** for delivery
* **Express.js** as backend API
* **React Native** (with `expo-notifications`) as the client

---

## 🚀 Features

* 🔔 Send push notifications to specific devices
* 📲 Expo push token registration from mobile client
* ✅ Supports Expo Go and standalone apps

---

## 📦 Backend Setup (Express.js)

### 1. Clone and Install

```bash
git clone https://github.com/smart-fish-ponds-algeria/notification-system.git
cd notification-system
npm install
```

### 2. Environment Variables

Create a `.env` file:

```
EXPO_ACCESS_TOKEN=your_expo_access_token
PORT=3001
```

> ✅ You can generate an Expo access token from [https://expo.dev/accounts](https://expo.dev/accounts)

### 3. Start the server

```bash
node index.js
```

### 4. Example Notification Route

```http
POST /send-notification
Content-Type: application/json

{
  "title": "Hello",
  "body": "This is a test notification",
  "expoPushToken": "ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]"
}
```

---

## 📱 Mobile App Setup (React Native with Expo)

### 1. Install Dependencies

```bash
npx create-expo-app your-app
cd your-app
npx expo install expo-notifications
```

### 2. Configure Notifications

In your `App.js` or `notifications.js`:

```js
import * as Notifications from 'expo-notifications';
import Constants from 'expo-constants';

async function registerForPushNotificationsAsync() {
  const { status: existingStatus } = await Notifications.getPermissionsAsync();
  const finalStatus = existingStatus === 'granted'
    ? existingStatus
    : (await Notifications.requestPermissionsAsync()).status;

  if (finalStatus !== 'granted') return;

  const token = (await Notifications.getExpoPushTokenAsync()).data;

  // Send token to your backend
  await fetch('http://your-backend/send-notification', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: 'Test',
      body: 'Token Registered',
      expoPushToken: token,
    }),
  });
}
```

---

## 🔧 FCM Setup (For Production)

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a project
3. Go to **Project Settings > Service Accounts**
4. Generate a new private key (JSON) for Firebase Admin SDK
5. Follow the [Expo guide to use FCM V1](https://docs.expo.dev/push-notifications/using-fcm/)

---

## 🧪 Testing

You can test push notifications with:

* Expo Go App
* Your physical device (required for push)

> 🔥 Notifications won't work on iOS Simulator or Android Emulator.
