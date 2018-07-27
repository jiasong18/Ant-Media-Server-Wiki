WebRTC Native Android SDK lets developers build their own native WebRTC applications that with just some clicks. WebRTC Android SDK has built-in functions such as publishing to Ant Media Server for one-to-many streaming, playing a stream in Ant Media Server and lastly P2P communication by using Ant Media Server as a signalling server. Anyway, no need to say too much things let's make our hands dirty. 

### Download the WebRTC Native Android SDK
We provide WebRTC Native Android SDK to Ant Media Server Enterprise users for free. Please keep in touch for getting WebRTC Native Android SDK. As a result download the SDK and open it to a directory that we will import it to Android App Project
    
## Creating Android Project
### Open Android Studio and Create a New Android Project
Just Click **`File > New > New Project`** . A window should open as shown below for the project details. Fill the form according to your Organization and Project Name
![Create Android Studio Project For WebRTC Native Android SDK](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.05.59.png)

Click Next button and Choose **`Phone and Tablet`** as below. 
![WebRTC Native Android App](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.06.08.png)

Lastly Choose **`Empty Activity`** in the next window
![Choose Empty Activity](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.19.11.png)

Let the default settings for activity name and layout. Click Next and finish creating the project.

### Import WebRTC SDK to Android Project 
After creating the project. Let's import the WebRTC Android SDK to the project. For doing that click
**`File > New > Import Module`** . Choose directory of the WebRTC Android SDK and click Finish button.
![Import Native WebRTC Android SDK](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.11.49.png)

If module is not included in the project, add the module name into `settings.gradle` file as shown in the image below.
![Import module in setting.gradle](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.20.22-3.png)

### Add dependency to Android Project App Module
Right click `app`, choose `Open Module Settings`and click the `Dependencies` tab. Then a window should appear as below. Click the `+` button at the bottom and choose `Module Dependency``
![Add Module Dependeny WebRTC Android SDK](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.34.56.png)

Choose WebRTC Native Android SDK and click OK button

![Native WebRTC Android SDK](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.21.06.png)

**One critical thing about that** You need import Module as an API as shown in the image below. You can change it from Implementation to API in the drop down list
![Choose API in drop down list](https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.39.27.png)



