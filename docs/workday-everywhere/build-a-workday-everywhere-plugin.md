# Build a Workday Everywhere Plugin

## Prerequisites
* Install Cursor or another integrated development environment (IDE).
* Install `brew.sh`.
* Verify that you have the login credentials as a Workday Implementer, which are provided as part of the DevCon Hackathon.

## Context
You can use these steps to set up your development machine, configure local project files, authenticate to the Builder Service, deploy an Extend application, and build a custom plugin for the Microsoft Teams simulator using agentic coding tools.

## Steps

### 1. Setup
1. From the terminal, run brew update.
2. Install NPM through the terminal.
3. From the terminal, run `brew install node`.

### 2. Configure local project
1. Setup your local project through the terminal. From the terminal, run `mkdir my-plugin && cd my-plugin`. See [Workday/Everywhere.](https://github.com/Workday/everywhere)
  > **Note:** We develop using agentic coding tools like Cursor. See [Cursor Documentation](https://cursor.com/docs). Use Cursor as your IDE, your terminal, and your agent window.
2. Verify that you are in the `my-plugin` directory.
3. Run `npx @workday/everywhere init`.
4. Run `npx @workday/everywhere view`.
  > **Note:** This command opens a window in your browser at *http://localhost:4242/*. An initial *Hello World* message displays on the screen.

### 3. Edit `package.json` file
1. Locate the `package.json` file in your project folder.
2. Change both the name and title of your plugin in the `package.json` file. Complete the task parameters:
  
  | Parameter | Description / Actions |
  | :--- | :--- |
  | name | <ul><li>On line 2 of the `package.json` file, edit the name of the plugin. The default value is `my-plugin`.</li><li>Set a unique name to avoid tenant collisions.</li><li>Users do not view this name.</li><li>You can enter any name that follows NPM package naming conventions. Permitted characters include lowercase letters, numbers, hyphens (-), underscores (\_), and dots (.). See [Naming Rules](https://github.com/npm/validate-npm-package-name#naming-rules).</li></ul> |
  | title | <ul><li>Set the user-facing title to define the human-readable name of your plugin.</li><li>Add a new line on line 3 written as `"title": "Your plugin name",`.</li><li>Keep the title to 20 characters or less to prevent the name from stretching across the screen.</li><li>Character restrictions do not apply to this field.</li><li>Users view this title in the Microsoft Teams simulator menu bar.</li></ul> |

![WE_myplugin|690x204](upload://sJuCH9NRrilWaJJaCtoJW4XcsJP.png)

3. Run `npx @workday/everywhere view` again and verify that the name in the upper-left corner updates to the new name you entered.
  > **Note:** You can view the title after you build and publish the plugin to the Microsoft Teams simulator in later steps.
![WE_package_json|643x500](upload://haGpKsAatCNARo2EquzeS9gnJFA.png)
  

### 4. Connect to App Builder
1. Go to [https://next.we.megaleo.com](https://next.we.megaleo.com/).
2. Enter the tenant alias for your developer tenant. You can find your tenant alias at [https://developer.workday.com/console/tenants](https://developer.workday.com/console/tenants).
  > **Note:** You must use a Hack Tenant. Your tenant name must follow a format similar to `hack119_wcpdev1`.
3. Register an API Client if you use a new tenant. Complete the configuration through the setup wizard:
   * Redirect URI: `https://next.we.megaleo.com/builder/auth/callback`.
   * CORS: `next.we.megaleo.com`.
   * Select **Integration** or any other scope you prefer.
     > **Note:** You cannot replace your API Client or your client secret after creation, so use care during this step.
4. Sign in using a username and password from the tenant.
5. In the Builder Service, get your CLI keys by accessing the ellipsis upper-right side and selecting **Get CLI Token**.
6. Copy the Workday Everywhere CLI key to the clipboard.

![WE_get_CLI_token|690x372](upload://tzyyhKQiaELBdcrSUg01tZBAbfs.png)

### 5. Gateway Authentication
1. Go back to the terminal.
2. Use `Ctrl + C` to clear the terminal.
3. Run `npx @workday/everywhere auth login --gateway next.we.megaleo.com`.
4. Paste your CLI token. Verify that the terminal displays a success message.
5. Run `npx @workday/everywhere auth status`.
  > **Note:** The status indicator must be green. Tokens remain active for 1 hour. You can refresh access by repeating the authentication steps.

### 6. Initial Simulator Publication
1. Publish your Hello World application to the Microsoft Teams simulator from the `my-plugin` directory. Run `npx @workday/everywhere build`.
  > **Note:** If errors occur, verify that you did not use a forbidden character in your `package.json` file. See Step 3.b.
2. Run `npx @workday/everywhere publish`.
3. Go to `https://next.we.megaleo.com/ to view your application`. The title you configured in Step 3.b is displayed in the Microsoft Teams view.

### 7. Extend Application Integration
1. Design an Extend application from scratch, copy an application from the App Catalog, or use an existing application. Go to [https://developer.workday.com/console/apps](https://developer.workday.com/console/apps?page=1) and click **Create**. Move to the next step after creating your application.
2. Set your Domain Security Policy and deploy your Extend app. See [Adding Security Domain Policy](https://drive.google.com/file/d/1kLEMryXfWP_T6lj7IHbwu7XqKk3bpd4w/view) on deploying applications and setting domain security policies.
   * See the developer documentation [ Add Security Domains in Visual Mode](https://developer.workday.com/documentation/igm1659447621522) on setting your domain security policy.
   * See the developer documentation [Create Extend Apps Using App Builder](https://developer.workday.com/documentation/xer1651695264256) on deploying your Extend app.
3.  Move to the next step after deployment finishes.
4. Walk through your Extend application in a web browser. Click **View in Tenant** from the deployment success window to open the application.
5. Take screenshots (save them) and evaluate which user flows to bring over into the Microsoft Teams simulator. Depending on the complexity of your Extend application, some functions and user flows might not fit the Microsoft Teams environment - it’s your call. You'll use these user flows and screenshots in Step 8.a.

![WE_deploy_successful|690x495](upload://j2SsOKu4CaGO2h3pZeIYeQI8XWR.png)

6. Go to **App Hub** > [https://developer.workday.com/console/apps](https://developer.workday.com/console/apps?page=1).
7. Click the ellipsis following the application name and click Download Source. You will locate this application package later.

![WE_downlod_source|526x287](upload://xYElmegApOIhwN2X4VW3bOlWWxv.png)

8. **Bind** your application source code to your local project folder to pull your Extend application functions into the local folder structure. Run `npx @workday/everywhere bind ~/downloads/THE_NAME_OF_YOUR_APP_DOWNLOAD.zip.`

### 8. Plugin Development and Iteration
1. Begin local development after your local viewer renders your *Hello World* message. Use your project folder and files to have the agent assist you in creating your plugin in Cursor. You can use any model you prefer, such as the default Composer or Composer2 models. Here are a few development guidelines that will set you up for success:
   * Instruct the agent to build functionality based on the files available in the project folder, which is how Cursor and Claude interpret projects.
   * Provide the agent with screenshots of the web Extend application you took earlier to ground the user interface design.
   * Describe the exact user flows you want to establish in the plugin.
   * Describe the different personas that will use the plugin if you manage multiple user roles, such as User, Manager, or Admin.
   * Include a brand style sheet containing your colors, fonts, line weights, and general brand feel. You can instruct the agent to generate one by providing links to your website or brand identity assets.
    > **Note:** Workday Canvas Kit is supported by default. If you want to use a custom style other than Canvas Kit, instruct your agent to apply inline styles in React.
   * You can specify a design system. Workday uses Canvas Kit and Microsoft Teams uses FluentUI if you want your plugin to match those specific styles. You can also select an alternative design system.
   * Include your logo and organization name. Instruct the Agent to place these elements at the top of the plugin page so users know who built the asset. You can attach images to your agent conversation using the Picture icon on the lower-right side of the prompt interface in Cursor.
   * You can supply a single large prompt or build iteratively. You can use Planning Mode to create an operational plan before the agent writes code.
2. Save your changes in Cursor using `[cmd + s]`.
3. Run `npx @workday/everywhere` view to refresh the local viewer. Verify that you can interact with your plugin and prompt your agent to make changes based on your visual results. Take time to iterate as needed.
  > **Note:** The local viewer features hot reloading to display your changes quickly. The viewer uses tenanted data after you authenticate with your CLI key. You must maintain an active authentication status to prevent errors.
4. Run the build and publish commands again to view your plugin in the Microsoft Teams Teams simulator.
   * build command: `npx @workday/everywhere build`
   * publish command: `npx @workday/everywhere publish`
5. Go to `https://next.we.megaleo.com/builder/preview` to view your application and verify that your plugin title is displayed in the interface.

## Results
You can now use the Microsoft Teams plugin to read and write data back to your tenant. Depending on your goals, you can also use the Extend application in your tenant to make changes and verify that those changes reflect in the Microsoft Teams environment.
> **Note:** Monitor your connection status, as access tokens remain active for 1 hour. See Step 5.e to retrieve your token and authenticate through the CLI.

## Provide Feedback and Register for Early Adopter Programs
You can provide development feedback or register your interest in upcoming Early Adopter programs for Workday Everywhere features. This information helps optimize the plugin configuration experience.

1. Access the Builder menu.
2. Select **Feedback & EA**.
3. Enter your contact information and evaluation details.
4. Click **Submit**.

![WE_feedback|690x393](upload://dE5OPzEagN5yD7SSvadrKBOnr5J.png)

## Remove registered tenants and Extend API Client
When you remove your tenant registration and Extend API Client, Workday automatically removes all of your published applications. You can register your tenant and clients again by completing the configuration process.

  1. Access the Builder menu.
2. Click **Forget Me**.

![WE_forget_me|690x378](upload://ou3IHwKrmviCX5j7Jj4VkCw73hq.png)

## Remove your plugin from the Microsoft Teams Simulator
To remove your plugin from the simulator, run `npx @workday/everywhere unpublish`. You can publish the application again at any time using the publish command in the terminal.

## Demonstrate Workday Everywhere Features Using Multiple Roles
You can demonstrate Workday Everywhere functionality across multiple concurrent sessions to evaluate multi-user and role-based workflows.

1. In your main browser window, sign in to Workday as your first test user, such as Logan McNeil, using your standard development path.
2. Open an Incognito window or alternative browser application for your second user.
3. Access `https://next.we.megaleo.com/builder/preview`.
4. Sign in as your second test user, such as Betty Liu.
5. Switch between the browser windows to demonstrate multi-user interactions.