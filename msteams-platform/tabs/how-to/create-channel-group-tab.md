---
title: Create a channel or group tab
author: laujan
description: A quickstart guide to creating a channel and group tab with the Yeoman Generator for Microsoft Teams.
localization_priority: Normal
ms.topic: quickstart
ms.author: lajanuar
---

# Create a channel or group tab

You can create a channel or group tab using Node.js and the Yeoman Generator, using ASP.NETCore, and using ASP.NETCore MVC.

# [Node.js](#tab/nodejs)

## Create a custom channel and group tab using Node.js and the Yeoman Generator

> [!NOTE]
> This article follows the steps outlined in the [build Your First Microsoft Teams app](https://github.com/OfficeDev/generator-teams/wiki/Build-Your-First-Microsoft-Teams-App) Wiki found in the Microsoft OfficeDev GitHub repository.

You can create a custom channel or group tab using the [Teams Yeoman generator](https://github.com/OfficeDev/generator-teams/).

[!INCLUDE [node-js-yeoman-prereq](~/includes/tabs/node-js-yeoman-prereq.md)]

   **What kind of Tab would you like to create?**

   Use the arrow keys to select **Configurable** tab.

   **What scopes do you intend to use for your Tab?**

   You can select a Team or a group chat.

   **Do you require Azure AD Single-Sign-On support for the tab?**

   Choose **not** to include Azure AD Single-Sign-On support for the tab. The default is yes, enter **n**.

   **Do you want this tab to be available in SharePoint Online? (Y/n)** 

   Select **n**.

> [!IMPORTANT]
> The path component **yourDefaultTabNameTab**, is the value that you entered in the generator for **Default Tab Name** plus the word **Tab**.
>
> For example: DefaultTabName: **MyTab** => **/MyTabTab/**

In your project directory, navigate to the following:

```bash
./src/app/scripts/<yourDefaultTabNameTab>/<yourDefaultTabNameTab>.tsx
```

Locate the `render()` method and add the following `<div>` tag and content to the top of the `<PanelBody>` container code:

```html
    <PanelBody>
        <div style={styles.section}>
            Hello World! Yo Teams rocks!
        </div>
    </PanelBody>
```

Make sure to save the updated file.

### Build and run your application

Open a command prompt in your project directory to complete the next tasks.

[!INCLUDE [node-js-yeoman-gulp-tasks](~/includes/tabs/node-js-yeoman-gulp-tasks.md)]

To view your tab configuration page, go to `https://localhost:3007/<yourDefaultAppNameTab>/config.html`. The following is shown:

![Configuration page screenshot](~/assets/images/tab-images/configurationPage.png)

### Establish a secure tunnel to your tab

Microsoft Teams is a cloud-based product and requires that your tab content be available from the cloud using HTTPS endpoints. Teams does not allow local hosting. You must either publish your tab to a public URL or use a proxy that exposes your local port to an internet-facing URL.

To test your tab extension, use [ngrok](https://ngrok.com/docs), which is built into this application. Ngrok is a reverse proxy software tool that creates a tunnel to your locally running web server's publicly-available HTTPS endpoints. Your server's web endpoints are available during the current session on your computer. When the computer is shut down or goes to sleep the service is no longer available.

In your command prompt, exit localhost and enter the following:

```bash
gulp ngrok-serve
```

> [!IMPORTANT]
> After your tab has been uploaded to Microsoft Teams and successfully saved, you can view it in the tabs gallery, add it to the tabs bar, and interact with it until your ngrok tunnel session ends. If you restart your ngrok session, you'll need to update your app with the new URL.

### Upload your application to Teams

**To upload your application to Teams**

1. Open Microsoft Teams. If you use the [web-based version](https://teams.microsoft.com) you can inspect your front-end code using your browser's [developer tools](~/tabs/how-to/developer-tools.md).
1. From your teams on the left panel, select the ellipses &#x25CF;&#x25CF;&#x25CF; next to the team that you are using to test your tab and choose **Manage team**.
1. In the main panel, select **Apps** from the tab bar and choose **Upload a custom app** located in the lower right corner of the page.
1. Open your project directory, browse to the **./package** folder, select the app package zip folder, and choose **Open**.
1. Select **Add** in the pop-up dialog box. Your tab uploads into Teams.
1. Return to your team, choose the channel where you want to display the tab, select ➕ from the tab bar, and choose your tab from the gallery.
1. Follow the directions for adding a tab. There is a custom configuration dialog box for your channel or group tab.
1. Select **Save** and your tab is added to the channel's tab bar.

# [ASP.NET Core](#tab/aspnetcore)

## Create a custom channel or group tab with ASP.NET Core

You can create a custom channel or group tab using C# and ASP.Net Core Razor page. [App Studio for Microsoft Teams](~/concepts/build-and-test/app-studio-overview.md) is also used to finalize your app manifest and deploy your tab to Teams.

### Prerequisites for Teams apps

You must have an understanding of the following prerequisites:

- You must have an Office 365 tenant and a team configured with **Allow uploading custom apps** enabled. For more information, see [prepare your Office 365 tenant](~/concepts/build-and-test/prepare-your-o365-tenant.md).

    - If you do not currently have a Microsoft 365 account, you can sign up for a free subscription through the [Microsoft Developer Program](https://developer.microsoft.com/en-us/microsoft-365/dev-program). The subscription remains active as long as you are using it for ongoing development.

- Use App Studio to import your application to Teams. To install App Studio, select **Apps** ![Store App](~/assets/images/tab-images/storeApp.png) at the lower left corner of the Teams app, and search for **App Studio**. After you find the tile, select it and choose **Add** in the pop-up dialog box to install it.

In addition, this project requires that you have the following installed in your development environment:

- The current version of the Visual Studio IDE with the **.NET CORE cross-platform development** workload installed. If you do not already have Visual Studio, you can download and install the latest [Microsoft Visual Studio Community](https://visualstudio.microsoft.com/downloads) version for free.

- The [ngrok](https://ngrok.com) reverse proxy tool. Use ngrok to create a tunnel to your locally running web server's publicly-available HTTPS endpoints. You can [download ngrok](https://ngrok.com/download).

### Get the source code

Open a command prompt and create a new directory for your tab project. A simple project is provided to get you started. To retrieve the source code you can download the zip folder and extract the files or clone the sample repository into your new directory using the following command:

```bash
git clone https://github.com/OfficeDev/microsoft-teams-sample-tabs.git
```

**To build and run the tab project**

1. After you have the source code, open Visual Studio and select **Open a project or solution**.
1. Navigate to the tab application directory and open **ChannelGroupTab.sln**.
1. To build and run your application press **F5** or choose **Start Debugging** from the **Debug** menu.
1. In a browser navigate to the following URLs and verify the application loaded properly:

- `http://localhost:44355`
- `http://localhost:44355/privacy`
- `http://localhost:44355/tou`

### Review the source code

#### Startup.cs

This project was created from an ASP.NET Core 2.2 Web Application empty template with the **Advanced - Configure for HTTPS** check box selected at setup. The MVC services are registered by the dependency injection framework's `ConfigureServices()` method. Additionally, the empty template does not enable serving static content by default, so the static files middleware is added to the `Configure()` method using the following code:

```csharp
public void ConfigureServices(IServiceCollection services)
  {
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
  }
public void Configure(IApplicationBuilder app)
  {
    app.UseStaticFiles();
    app.UseMvc();
  }
```

#### wwwroot folder

In ASP.NET Core, the web root folder is where the application looks for static files.

#### Index.cshtml

ASP.NET Core treats files called **Index** as the default or home page for the site. When your browser URL points to the root of the site, **Index.cshtml** is displayed as the home page for your application.

#### Tab.cs

This C# file contains a method that is called from **Tab.cshtml** during configuration.

#### AppManifest folder

This folder contains the following required app package files:

- A **full color icon** measuring 192 x 192 pixels.
- A **transparent outline icon** measuring 32 x 32 pixels.
- A **manifest.json** file that specifies the attributes of your app.

These files need to be zipped in an app package for use in uploading your tab to Teams. When a user chooses to add or update your tab, Microsoft Teams loads the `configurationUrl` specified in your manifest, embeds it in an IFrame, and renders it in your tab.

#### .csproj

In the Visual Studio Solution Explorer window, right-click on the project and select **Edit Project File**. At the end of the file you see the following code that creates and updates your zip folder when the application builds:

```xml
<PropertyGroup>
    <PostBuildEvent>powershell.exe Compress-Archive -Path \"$(ProjectDir)AppManifest\*\" -DestinationPath \"$(TargetDir)tab.zip\" -Force</PostBuildEvent>
  </PropertyGroup>

  <ItemGroup>
    <EmbeddedResource Include="AppManifest\icon-outline.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\icon-color.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\manifest.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
  </ItemGroup>
```

### Establish a secure tunnel to your tab for Teams

Microsoft Teams is a cloud-based product and requires that your tab content be available from the cloud using HTTPS endpoints. Teams does not allow local hosting. You must either publish your tab to a public URL, or use a proxy that exposes your local port to an internet-facing URL.

To test your tab, use [ngrok](https://ngrok.com/docs). Your server's web endpoints are available while ngrok is running on your computer. If you close ngrok, the URLs are different the next time you start it.

- Open a command prompt in the root of your project directory and run the following command:

    ```bash
    ngrok http https://localhost:44355 -host-header="localhost:44355"
    ```

- Ngrok listens to requests from the internet and routes them to your application when it is running on port 44355. It should resemble `https://y8rCgT2b.ngrok.io/` where **y8rCgT2b** is replaced by your ngrok alpha-numeric HTTPS URL.

- Ensure that you keep the command prompt with ngrok running and make a note of the URL.

### Update your application

Within **Tab.cshtml** the application presents the user with two option buttons for displaying the tab with either a red or gray icon. Choosing the **Select Gray** or **Select Red** button triggers `saveGray()` or `saveRed()`, respectively, sets `settings.setValidityState(true)`, and enables the **Save** button on the configuration page. This code lets Teams know that you have completed the configuration requirements and the installation can proceed. The parameters of `settings.setSettings` are set. Finally, `saveEvent.notifySuccess()` is called to indicate that the content URL has been successfully resolved.

#### _Layout.cshtml

For your tab to display in Teams, you must include the **Microsoft Teams JavaScript client SDK** and include a call to `microsoftTeams.initialize()` after your page loads. This is how your tab and the Teams client communicate:

Navigate to the **Shared** folder, open **_Layout.cshtml**, and add the following to the `<head>` tag:

```html
<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js"></script>
<script src="https://statics.teams.cdn.office.net/sdk/v1.6.0/js/MicrosoftTeams.min.js"></script>
```

> [!IMPORTANT]
> Do not copy and paste the `<script src="...">` URLs from this page, as they do not represent the latest version. To get the latest version of the SDK, always go to [Microsoft Teams JavaScript API](https://www.npmjs.com/package/@microsoft/teams-js).

#### Tab.cshtml

**To update Tab.cshtml**

1. Open **Tab.cshtml** and update the embedded `<script>`.

1. At the top of the script, call `microsoftTeams.initialize()`.

1. Update the `websiteUrl` and `contentUrl` values in each function with the HTTPS ngrok URL to your tab.

    Your code should now include the following with **y8rCgT2b** replaced with your ngrok URL:

    ```javascript
        microsoftTeams.initialize();

        let saveGray = () => {
            microsoftTeams.settings.registerOnSaveHandler(function (saveEvent) {
                microsoftTeams.settings.setSettings({
                    websiteUrl: `https://y8rCgT2b.ngrok.io`,
                    contentUrl: `https://y8rCgT2b.ngrok.io/gray/`,
                    entityId: "grayIconTab",
                    suggestedDisplayName: "MyNewTab"
                });
                saveEvent.notifySuccess();
            });
        }

        let saveRed = () => {
            microsoftTeams.settings.registerOnSaveHandler(function (saveEvent) {
                microsoftTeams.settings.setSettings({
                    websiteUrl: `https://y8rCgT2b.ngrok.io`,
                    contentUrl: `https://y8rCgT2b.ngrok.io/red/`,
                    entityId: "redIconTab",
                    suggestedDisplayName: "MyNewTab"
                });
                saveEvent.notifySuccess();
        });
        }
    ```

1. Save the updated **Tab.cshtml**.

### Build and run your application for Teams

**To build and run your application**

1. In Visual Studio press **F5**, or choose **Start Debugging** from the **Debug** menu.
1. Verify that **ngrok** is running and working properly by opening your browser and going to your content page via the ngrok HTTPS URL that was provided in your command prompt window.

> [!TIP]
> You need to have both your application in Visual Studio and ngrok running to complete the steps provided in this article. If you need to stop running your application in Visual Studio to work on it, **keep ngrok running**. It listend and resumes routing your application's request when it restarts in Visual Studio. If you have to restart the ngrok service it returns a new URL and you have to update your application with the new URL.

### Upload your tab for Teams

> [!NOTE]
> We use App Studio to edit your **manifest.json** file and upload the completed package to Teams. You can also manually edit the **manifest.json** file. If you do, ensure that you build the solution again to create the **tab.zip** file to upload.

**To upload your tab with App Studio**

1. Open Microsoft Teams. If you use the [web-based version](https://teams.microsoft.com), you can inspect your front-end code using your browser's [developer tools](~/tabs/how-to/developer-tools.md).

1. Open **App Studio** and select the **Manifest editor** tab.

1. Select **Import an existing app** in the **Manifest editor** to begin updating the app package for your tab. The source code comes with its own partially complete manifest. The name of your app package is **tab.zip**. It is available from the following path:

    ```bash
    /bin/Debug/netcoreapp2.2/tab.zip
    ```

1. Upload **tab.zip** to App Studio.

#### Update your app package with Manifest editor

After you have uploaded your app package into App Studio, you must configure it.

Select the tile for your newly imported tab in the right panel of the Manifest editor welcome page.

There is a list of steps on the left side of the Manifest editor, and on the right, a list of properties that must have values for each of those steps. Much of the information has been provided by your **manifest.json** but there are fields that you must update.

##### Details: App details

In the **App details** section:

1. Under **Identification**, select **Generate** to replace the placeholder ID with the required GUID for your tab.

1. Under **Developer information**, update the **Website URL** field with your **ngrok** HTTPS URL.

1. Under **App URLs**, update the **Privacy statement** to `https://<yourngrokurl>/privacy` and **Terms of use** to `https://<yourngrokurl>/tou`>.

##### Capabilities: Tabs

In the **Tabs** section:

1. Under **Team Tab**, select **Add**.

1. In the **Team Tab** pop-up window, update the **Configuration URL** to `https://<yourngrokurl>/tab`.

1. Ensure the **can update configuration? Team**, and **Group chat** boxes are checked and select **Save**.

##### Finish: Domains and permissions

In the **Domains and permissions** section, **Domains from your tabs** must contain your ngrok URL without the HTTPS prefix `<yourngrokurl>.ngrok.io/`.

##### Finish: Test and distribute

> [!IMPORTANT]
> On the right, in **Description**, you see the following warning:
>
> &#9888; "**The 'validDomains' array cannot contain a tunneling site...**"
>
> This warning can be ignored while testing your tab.

1. In the **Test and Distribute** section, select **Install**.

1. In the pop-up window, in the **Add to a team or chat** field, enter your team and select **Install**.

1. In the next pop-up window, choose the team channel where you want the tab to be displayed and select **Set up**.

1. In the final pop-up window, select a value for the tab page either a red or gray icon and select **Save**.

1. To view your tab, navigate to the team you installed it on, and select it from the tab bar. The page that you chose during configuration is displayed.

# [ASP.NET Core MVC](#tab/aspnetcoremvc)

## Create a custom channel or group tab with ASP.NET Core MVC

You can create a custom channel or group tab using C# and ASP.Net Core MVC. [App Studio for Microsoft Teams](~/concepts/build-and-test/app-studio-overview.md) is also used to finalize your app manifest and deploy your tab to Teams.

## Prerequisites for custom channel or group tab

- You must have a Microsoft 365 tenant and a team configured with **Allow uploading custom apps** enabled. For more information, see [prepare your Office 365 tenant](~/concepts/build-and-test/prepare-your-o365-tenant.md).

  - If you do not currently have a Microsoft 365 account, you can sign up for a free subscription through the [Microsoft Developer Program](https://developer.microsoft.com/en-us/microsoft-365/dev-program). The subscription remains active as long as you are using it for ongoing development.

- Use App Studio to import your application to Teams. To install App Studio, select **Apps** ![Store App](~/assets/images/tab-images/storeApp.png) at the lower left corner of the Teams app, and search for **App Studio**. After you find the tile, select it and choose **Add** in the pop-up dialog box to install it.

In addition, this project requires that you have the following installed in your development environment:

- The current version of the Visual Studio IDE with the **.NET CORE cross-platform development** workload installed. If you do not already have Visual Studio, you can download and install the latest [Microsoft Visual Studio Community](https://visualstudio.microsoft.com/downloads) version for free.

- The [ngrok](https://ngrok.com) reverse proxy tool. Use ngrok to create a tunnel to your locally running web server's publicly-available HTTPS endpoints. You can [download ngrok](https://ngrok.com/download).

### Get the source code

Open a command prompt and create a new directory for your tab project. A simple [Channel Group Tab](https://github.com/OfficeDev/microsoft-teams-sample-tabs/tree/master/ChannelGroupTabMVC) project is provided to get you started. To retrieve the source code you can download the zip folder and extract the files or clone the sample repository into your new directory using the following command:

```bash
git clone https://github.com/OfficeDev/microsoft-teams-sample-tabs.git
```

**To build and run the tab project**

1. After you have the source code, open Visual Studio and select **Open a project or solution**.
1. Navigate to the tab application directory and open **ChannelGroupTabMVC.sln**.
1. To build and run your application, press **F5** or choose **Start Debugging** from the **Debug** menu.
1. In a browser, navigate to the following URLs and verify that the application loaded properly:

- `http://localhost:44360`
- `http://localhost:44360/privacy`
- `http://localhost:44360/tou`

### Review the source code

#### Startup.cs

This project was created from an ASP.NET Core 2.2 Web Application empty template with the **Advanced - Configure for HTTPS** check box selected at setup. The MVC services are registered by the dependency injection framework's `ConfigureServices()` method. Additionally, the empty template does not enable serving static content by default, so the static files middleware is added to the `Configure()` method using the following code:

```csharp
public void ConfigureServices(IServiceCollection services)
  {
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
  }
public void Configure(IApplicationBuilder app)
  {
    app.UseStaticFiles();
    app.UseMvc();
  }
```

#### wwwroot folder

In ASP.NET Core, the web root folder is where the application looks for static files.

#### AppManifest folder

This folder contains the following required app package files:

- A **full color icon** measuring 192 x 192 pixels.
- A **transparent outline icon** measuring 32 x 32 pixels.
- A **manifest.json** file that specifies the attributes of your app.

These files need to be zipped in an app package for use in uploading your tab to Teams.

#### .csproj

In the Visual Studio Solution Explorer window, right-click on the project and select **Edit Project File**. At the end of the file you see the following code that creates and updates your zip folder when the application builds:

```xml
<PropertyGroup>
    <PostBuildEvent>powershell.exe Compress-Archive -Path \"$(ProjectDir)AppManifest\*\" -DestinationPath \"$(TargetDir)tab.zip\" -Force</PostBuildEvent>
  </PropertyGroup>

  <ItemGroup>
    <EmbeddedResource Include="AppManifest\icon-outline.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\icon-color.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\manifest.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
  </ItemGroup>
```

#### Models

**ChannelGroup.cs** presents a Message object and methods that will be called from the controllers during configuration.

#### Views

##### Home

ASP.NET Core treats files called **Index** as the default or home page for the site. When your browser URL points to the root of the site, **Index.cshtml** will be displayed as the home page for your application.

##### Shared

The partial view markup **_Layout.cshtml** contains the application's overall page structure and shared visual elements. It will also reference the Teams Library.

#### Controllers

The controllers use the `ViewBag` property to transfer values dynamically to the views.

[!INCLUDE [dotnet-ngrok-intro](~/includes/tabs/dotnet-ngrok-intro.md)]

* Open a command prompt in the root of your project directory and run the following command:

    ```bash
    ngrok http https://localhost:443560 -host-header="localhost:44360"
    ```

* Ngrok will listen to requests from the internet and will route them to your application when it is running on port 44355. It should resemble `https://y8rCgT2b.ngrok.io/` where **y8rCgT2b** is replaced by your ngrok alpha-numeric HTTPS URL.

* Ensure that you keep the command prompt with ngrok running and make a note of the URL.

### Update your application

Within **Tab.cshtml** the application presents the user with two option buttons for displaying the tab with either a red or gray icon. Choosing the **Select Gray** or **Select Red** button, triggers `saveGray()` or `saveRed()`, respectively, sets `settings.setValidityState(true)`, and enables the **Save** button on the configuration page. This code lets Teams know that you have completed the configuration requirements and the installation can proceed. On save, the parameters of `settings.setSettings` are set. Finally, `saveEvent.notifySuccess()` is called to indicate that the content URL has been successfully resolved.

[!INCLUDE [dotnet-update-app](~/includes/tabs/dotnet-update-chan-grp-app.md)]